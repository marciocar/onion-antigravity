# 📦 Instalação do Sistema Onion (Google Antigravity)

> **Plataforma**: Google Antigravity | **Estrutura**: `.agents/` + `docs/` | **Data**: 2026-06-03

> **Nota**: versões anteriores deste guia descreviam um CLI npm (`onion init`) e a estrutura `.onion/`. Ambos foram **abandonados** (CLI/`.onion/` em 2026-05-18; migração de plataforma Claude Code → Antigravity em 2026-06, ver [ADR-001](../analysis/onion-antigravity-migration-adr-2026-06.md)). O Onion **não é produto npm** e **não tem CLI standalone**: é um framework instalável copiando `.agents/` e `docs/` para o projeto-alvo.

---

## 🎯 Pré-requisitos

Antes de instalar o Sistema Onion, certifique-se de ter:

| Requisito | Observação | Verificar |
|-----------|------------|-----------|
| **Google Antigravity** | IDE/runtime do Onion | Config global em `~/.gemini/` |
| **Git** | >= 2.20.0 | `git --version` |
| **Task Manager (opcional)** | Jira / ClickUp / Asana / Linear via MCP | Configurado em `~/.gemini/config/mcp_config.json` |

---

## 🚀 Instalação

O Sistema Onion é instalado **copiando ou clonando** `.agents/` e `docs/` para a raiz do projeto-alvo. Não há build, npm install ou link global.

### Método 1: Clonar e copiar

```bash
# 1. Clone o repositório do framework
git clone https://github.com/your-org/onion.git /tmp/onion

# 2. Copie .agents/ e docs/ para o projeto-alvo
cp -r /tmp/onion/.agents  /caminho/do/seu-projeto/
cp -r /tmp/onion/docs     /caminho/do/seu-projeto/   # opcional (spec-as-code)

# 3. Copie o template de ambiente
cp /tmp/onion/.env.example /caminho/do/seu-projeto/.env
```

### Método 2: Submódulo / template

Você também pode usar o repositório como **template do GitHub** ou adicionar `.agents/` como submódulo, conforme a política da sua equipe.

---

## ✅ Verificação da Instalação

### 1. Verificar estrutura `.agents/`

```bash
ls -la .agents/
# Esperado: AGENTS.md, rules/, workflows/, skills/, hooks.json, mcp_config.example.json
```

### 2. Verificar workflows disponíveis

```bash
ls .agents/workflows/   # 78 workflows (flat + prefixo de categoria)
```

No agente do Antigravity, os workflows ficam disponíveis via `/` (ex.: `/onion`, `/engineer-start`, `/product-task`).

### 3. Teste básico (no agente do Antigravity)

```markdown
/onion          # Ponto de entrada inteligente / navegação
/warm-up        # Preparação geral de contexto
```

---

## 🔧 Configuração do Antigravity

A camada operacional do Onion vive em `.agents/` (no projeto) + `~/.gemini/` (config global do usuário).

### Rules e workflows (automático)

1. Abra o projeto no Google Antigravity
2. As **rules** em `.agents/rules/` carregam always-on (identidade, idioma, roteamento de Task Manager, convenções)
3. Os **workflows** ficam disponíveis via `/` (ex.: `/product-help`, `/engineer-start`)
4. As **personas/subagents** documentadas em `.agents/AGENTS.md` ficam acessíveis via `@` (ex.: `@product-agent`, `@onion`)

### MCP e config global (`~/.gemini/`)

```bash
# Template versionado no projeto:
.agents/mcp_config.example.json

# Destino (config global, não versionada):
~/.gemini/config/mcp_config.json
```

Use o workflow `/meta-setup-integration` para configurar com segurança as variáveis de cada provider.

---

## 🛠️ Configuração de Integrações (Opcional)

### Task Manager (Jira / ClickUp / Asana / Linear)

O Onion é **provider-agnóstico**. Defina o provider ativo em `.env`:

```bash
# .env
TASK_MANAGER_PROVIDER=clickup   # jira | clickup | asana | linear | none

# Exemplo ClickUp
CLICKUP_API_TOKEN=pk_xxxxx
```

1. **Obter API Token** do provider escolhido
2. **Configurar `.env`** (e o MCP correspondente em `~/.gemini/config/mcp_config.json`)
3. **Validar** no agente:
   ```markdown
   /meta-setup-integration
   /product-task "criar nova feature"
   ```

> Detalhes por provider: `docs/reference/task-manager/adapters/` (`jira.md`, `clickup.md`, `asana.md`, `linear.md`).

### Whisper (Transcrição de Áudio)

Para habilitar transcrição de reuniões, configure a chave no `.env`:

```bash
# .env
OPENAI_API_KEY=seu_key_aqui   # Whisper Cloud (OpenAI)
```

---

## 📝 Primeiros Passos Após Instalação

```markdown
# No agente do Antigravity, na raiz do projeto:
/onion                                   # Navegação / recomendações
/product-spec "Sistema de autenticação"  # Criar especificação
/engineer-start                          # Iniciar desenvolvimento
```

---

## ❓ Troubleshooting

### Problema: Workflows não aparecem via `/`

```bash
# Verificar se .agents/ existe na raiz do projeto
ls -la .agents/workflows/

# Verificar se está na raiz correta
pwd
```

Se `.agents/` não existir, recopie do repositório do framework (ver "Instalação").

### Problema: Task Manager não conecta

```bash
# Verificar provider ativo
grep TASK_MANAGER_PROVIDER .env

# Validar/reconfigurar via workflow
/meta-setup-integration
```

Confirme que o MCP do provider está em `~/.gemini/config/mcp_config.json` e que as variáveis obrigatórias estão no `.env`.

### Problema: Rules não carregam

Confirme que os arquivos existem em `.agents/rules/` e reabra o projeto no Antigravity para recarregar as system instructions.

---

## 📚 Próximos Passos

Após instalação bem-sucedida:

1. ✅ Leia o [Getting Started](getting-started.md)
2. ✅ Explore o [Guia de Workflows](commands-guide.md)
3. ✅ Conheça as [personas/subagents](agents-reference.md)
4. ✅ Leia o [Contributing Guide](../../CONTRIBUTING.md)

---

## 🎉 Pronto!

Você instalou o **Sistema Onion** no Google Antigravity. Agora você tem acesso a:

- ✅ 78 workflows `/`-invocáveis em `.agents/workflows/`
- ✅ Personas/subagents em `.agents/AGENTS.md` (consolidados de 49 agentes especializados)
- ✅ 2 skills (`onion`, `onion-validation`) + 4 rules always-on
- ✅ Task Manager Abstraction multi-provider (Jira, ClickUp, Asana, Linear)

---

**Última atualização**: 2026-06-03
**Plataforma**: Google Antigravity
