---
title: Meta-spec — Padrões de Integração do Sistema Onion
date: 2026-06-03
version: 2.0.0
level: L0
status: active
gate-keeper: "@metaspec-gate-keeper"
changelog: "v2.0.0 — migração de plataforma Claude Code → Google Antigravity; adapter docs (.claude/utils/task-manager/) → docs/reference/task-manager/; MCP config → ~/.gemini/config/mcp_config.json (serverUrl, sem timeout top-level); hooks em .agents/hooks.json"
---

# Meta-spec — Padrões de Integração do Sistema Onion

## Propósito

Define os padrões obrigatórios para integrações com sistemas externos (Task Managers, MCPs, APIs). Usa **Task Manager Abstraction** como referência canônica de design de adapter — toda nova integração deve seguir o mesmo padrão.

Aplica-se ao **Sistema Onion**, não ao projeto-alvo onde o Onion é instalado.

Referências relacionadas:

- [agents.md](./agents.md), [commands.md](./commands.md)
- [architecture.md](./architecture.md), [code-standards.md](./code-standards.md)

Referência técnica: [docs/reference/task-manager/](../reference/task-manager/).

---

## 1. Task Manager Abstraction como referência canônica

A Task Manager Abstraction é o padrão **SDAAL** (Specification-Driven AI Abstraction Layer) implementado de referência. A documentação da abstração vive em `docs/reference/task-manager/` (consumida por workflows e personas); os servidores MCP reais são configurados em `~/.gemini/config/mcp_config.json`. Estrutura:

```
docs/reference/task-manager/
├── factory.md           # Instancia o adapter via TASK_MANAGER_PROVIDER
├── interface.md         # Contrato ITaskManager
├── types.md             # Tipos e DTOs
├── detector.md          # Detecção automática de provider
└── adapters/
    ├── jira.md          # Adapter Jira (REST v3, ADF)
    ├── clickup.md       # Adapter ClickUp (MCP)
    ├── asana.md         # Adapter Asana (HTML notes)
    └── linear.md        # Adapter Linear (Markdown)
```

Toda nova integração (ex: novo Task Manager, novo serviço de comunicação) deve replicar essa estrutura.

---

## 2. Estrutura obrigatória de adapter

Para cada integração com sistema externo:

```
docs/reference/<dominio>/
├── factory.md           # Roteamento por variável de ambiente
├── interface.md         # Contrato comum (operações independentes de provider)
├── types.md             # Tipos compartilhados
├── detector.md          # Detecção automática (opcional)
└── adapters/
    └── <provider>.md    # Um arquivo por provider suportado
```

### 2.1 Conteúdo de cada arquivo

**factory.md**:

- Lê variável de ambiente do `.env` para decidir provider
- Valida variáveis obrigatórias do provider escolhido
- Retorna instância do adapter ou erro descritivo
- Lida com `none` ou ausência (fallback gracioso)

**interface.md**:

- Lista operações que **todo provider deve suportar**
- Define inputs e outputs em formato neutro
- Não vaza detalhes de provider

**types.md**:

- DTOs comuns
- Enums
- Tipos compartilhados

**detector.md** (opcional):

- Detecção automática quando variável não é declarada explicitamente
- Heurísticas (existência de outras variáveis, presença de arquivos config)

**adapters/<provider>.md**:

- Implementação concreta para o provider
- Lista variáveis específicas necessárias
- Formatação requerida (ADF, Markdown, HTML, Unicode)
- Tratamento de erros específicos do provider
- Limites e cotas conhecidas

---

## 3. Gestão de `.env` e variáveis de ambiente

### 3.1 Convenção de nomes

- Prefixo do domínio em UPPER_SNAKE_CASE: `TASK_MANAGER_PROVIDER`, `JIRA_API_TOKEN`, `CLICKUP_WORKSPACE_ID`
- Sufixo descritivo do que a variável contém (`_TOKEN`, `_HOST`, `_ID`, `_URL`)
- Booleanos como string: `"true"` / `"false"`

### 3.2 Obrigatórias vs opcionais

Cada adapter deve documentar em sua seção:

| Variável | Tipo | Obrigatoriedade | Default | Descrição |
|---|---|---|---|---|
| `JIRA_HOST` | URL | Obrigatória | — | URL base da instância Jira |
| `JIRA_EMAIL` | email | Obrigatória | — | Email do usuário Jira |
| `JIRA_API_TOKEN` | secret | Obrigatória | — | Token gerado em Atlassian |
| `JIRA_PROJECT_KEY` | string | Opcional | — | Filtro default |
| `JIRA_AUTH_TYPE` | enum | Opcional | `basic` | `basic` ou `bearer` |
| `JIRA_API_VERSION` | enum | Opcional | `3` | `2` (Server/DC) ou `3` (Cloud) |

### 3.3 Fallback gracioso

Quando o usuário invoca um workflow que requer integração mas a variável obrigatória está ausente:

1. **Não inventar** valor nem assumir provider alternativo
2. Reportar em pt-BR qual variável falta
3. Sugerir workflow para configurar: `/meta-setup-integration`
4. Continuar offline quando possível (ex: `@task-specialist` decompõe localmente sem persistir)

Exemplo de mensagem:

```
Não foi possível conectar ao Jira: variável JIRA_API_TOKEN está vazia.
Para configurar, execute: /meta-setup-integration
Para operar offline, defina TASK_MANAGER_PROVIDER=none no .env.
```

### 3.4 `.env.example` versionado

- `.env.example` no root deve conter **todas** as variáveis documentadas com valor placeholder
- Comentários explicando obrigatoriedade e formato
- `.env` no `.gitignore` sempre

---

## 4. MCPs (Model Context Protocol) suportados

### 4.1 Como personas/workflows acessam MCP

No Antigravity, MCPs **não são declarados em frontmatter** de persona ou workflow. O acesso é resolvido pela configuração global do Antigravity em `~/.gemini/config/mcp_config.json`. A persona/workflow documenta em prosa quais providers/MCPs espera (ex: ClickUp MCP para `@clickup-specialist`).

### 4.2 MCPs comuns no framework atual

| MCP | Provedor | Usado por |
|---|---|---|
| `ClickUp_*` | ClickUp MCP | `@clickup-specialist`, workflows `/product-*` quando provider é ClickUp |
| `Asana__*` | Conector hospedado | Provider Asana |
| `Linear__*` | Conector hospedado | Provider Linear |
| `Atlassian__*` | Conector hospedado | Provider Jira |
| `Slack__*` | Conector hospedado | Notificações (opcional) |
| `Notion__*` | Conector hospedado | Documentação externa (opcional) |

### 4.3 Configuração

- **MCP servers** são declarados em `~/.gemini/config/mcp_config.json` (config global do usuário no Antigravity), no formato:

  ```json
  {
    "mcpServers": {
      "clickup": {
        "command": "npx",
        "args": ["-y", "@clickup/mcp-server"],
        "env": { "CLICKUP_API_TOKEN": "${CLICKUP_API_TOKEN}" }
      },
      "exemplo-remoto": {
        "serverUrl": "https://mcp.exemplo.com/sse"
      }
    }
  }
  ```

- Servidores **remotos** usam o campo `serverUrl` (não `httpUrl`), e **não há** campo `timeout` no nível top-level da config.
- O framework versiona um template **`.agents/mcp_config.example.json`** — o usuário copia o conteúdo para `~/.gemini/config/mcp_config.json` e ajusta ao provider ativo.
- **NUNCA** colar tokens diretamente: usar interpolação `${VAR}` resolvida do `.env`/ambiente.
- Aprovação/habilitação de MCP servers é feita na configuração do IDE do Antigravity (Allow/Deny/Ask), documentada no getting-started.
- `/meta-setup-integration` guia a configuração de `.env` + `mcp_config.json` quando aplicável.
- Eventos de ciclo de vida (ex: detecção de provider antes de invocar workflow) vivem em `.agents/hooks.json` via `PreInvocation` — não no MCP config nem no frontmatter.

---

## 5. Formatação por provider

Cada provider tem formato preferido para descrições, comentários e payloads. **Adapter é responsável por traduzir** dados internos para formato do provider.

| Provider | Descrições de task | Comments | Estrutura |
|---|---|---|---|
| Jira Cloud (v3) | ADF (Atlassian Document Format) — JSON estruturado | ADF | Bulk via `/issue/bulk` |
| Jira Server/DC (v2) | Wiki markup ou plain text (string) | Wiki markup | Search via `/search` (paginated) |
| ClickUp | Markdown nativo em `markdown_description` | Unicode visual em `commentText` (`━━━`, `∟`, `▶`, `◆`, `✅`) | API REST + MCP |
| Asana | HTML notes (subset) ou plain text | HTML | API REST |
| Linear | Markdown nativo (suporte rico) | Markdown | API GraphQL |

### 5.1 Templates por provider

Templates de formatação para cada provider devem viver em:

```
docs/reference/<dominio>/adapters/<provider>.md
docs/reference/<dominio>/templates/<provider>-<tipo>.md   # quando aplicável
```

Para ClickUp especificamente, existe documento de referência: `docs/reference/task-manager/clickup-formatting.md`.

---

## 6. Bulk operations e performance

### 6.1 Bulk-first

Quando operar em lote (>5 itens), preferir operação bulk do provider:

- Jira: `POST /rest/api/3/issue/bulk` (até 50/req)
- ClickUp: endpoints bulk quando disponíveis
- Evitar loops N+1 em criação/update

### 6.2 Field selection

Ao buscar itens, declarar apenas campos necessários para reduzir payload:

- Jira: `fields=summary,status,assignee`
- Linear: query GraphQL com seleção explícita

### 6.3 Paginação

- Implementar paginação consistente
- Jira Cloud v3: usar `nextPageToken` (o antigo `/search` foi removido em maio/2025)
- ClickUp: usar `page` parameter
- Não iterar todas as páginas quando não necessário

---

## 7. Tratamento de erros

### 7.1 Categorias

| Erro | Resposta esperada do adapter |
|---|---|
| Variável de ambiente ausente | Fallback gracioso (Seção 3.3) |
| Token inválido / expirado | Mensagem clara em pt-BR + sugestão de regeneração |
| Rate limit | Retry com backoff exponencial, máximo 3 tentativas |
| Recurso não encontrado | Reportar ID + provider + sugestão de verificação |
| Erro de validação do provider | Reportar mensagem original do provider + tradução pt-BR |
| Erro de rede transitório | Retry com backoff |
| Erro inesperado | Logar e reportar, não silenciar |

### 7.2 Não silenciar

- Adapter nunca deve "engolir" erro sem reportar
- Workflows chamadores devem propagar erro ao usuário com contexto

---

## 8. Adicionar novo adapter — checklist

Ao adicionar suporte a novo provider:

1. Criar `docs/reference/<dominio>/adapters/<provider>.md` seguindo estrutura de Seção 2.1
2. Atualizar `factory.md` para reconhecer o novo provider
3. Atualizar `detector.md` se houver detecção automática
4. Documentar variáveis de ambiente em `.env.example`
5. Atualizar a rule `.agents/rules/task-manager-routing.md` com a tabela "Provider → Variáveis → Persona → Adapter"
6. Declarar a persona especialista do provider em `.agents/AGENTS.md` (opcional, mas recomendado)
7. Adicionar a esta meta-spec (Seções 4.2 e 5)
8. Validar com `@metaspec-gate-keeper`

---

## 9. Proibições explícitas

- **Proibido** integração que requer credencial fora de `.env` / `${VAR}` interpolada
- **Proibido** invocar API externa diretamente em workflow sem passar pelo adapter
- **Proibido** adapter que vaza tipos específicos do provider para o nível de interface
- **Proibido** assumir provider sem ler `.env` primeiro
- **Proibido** colar token literal em `~/.gemini/config/mcp_config.json` ou em `.agents/mcp_config.example.json`

---

## 10. Versionamento e mudanças

Mudanças nesta spec exigem:

1. PR específico para `docs/meta-specs/integrations.md`
2. Atualização do campo `version`
3. Migração de adapters existentes quando aplicável
4. Validação por `@metaspec-gate-keeper`
