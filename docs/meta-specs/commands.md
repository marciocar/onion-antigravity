---
title: Meta-spec — Padrões para Workflows do Sistema Onion
date: 2026-06-03
version: 2.0.0
level: L0
status: active
gate-keeper: "@metaspec-gate-keeper"
changelog: "v2.0.0 — migração de plataforma Claude Code → Google Antigravity; comandos (.claude/commands/) → workflows (.agents/workflows/); invocação /cat/cmd → /cat-cmd; frontmatter reduzido a description; limites 500/800 → workflow ≤400 linhas"
---

# Meta-spec — Padrões para Workflows do Sistema Onion

## Propósito

Define os padrões imutáveis (L0) que **todos os workflows** em `.agents/workflows/` devem seguir. Inclui o conceito **invariante** de workflows faseados retomáveis, mecanismo que distingue o Onion de coleções de prompts avulsos.

No Google Antigravity, os artefatos `/`-invocáveis do Onion são **workflows** (saved prompts) em `.agents/workflows/`, em estrutura flat com prefixo de categoria no nome do arquivo. Esta é a evolução dos antigos "comandos" do Claude Code (`.claude/commands/`).

Aplica-se ao **Sistema Onion**, não ao projeto-alvo onde o Onion é instalado.

Referências relacionadas:

- [agents.md](./agents.md) — padrões para personas/subagents
- [architecture.md](./architecture.md) — estrutura de diretórios e dependências
- [code-standards.md](./code-standards.md) — padrões de código e idioma
- [integrations.md](./integrations.md) — padrões para integrações

---

## 1. Estrutura obrigatória

Todo workflow vive em `.agents/workflows/<categoria>-<nome>.md` (flat, com prefixo de categoria) e deve conter:

### 1.1 Frontmatter

O schema de frontmatter de workflow do Antigravity usa **apenas** o campo `description`:

```yaml
---
description: <descrição em uma linha — aparece na lista de workflows ao digitar />
---
```

- `description` é **obrigatório** e único campo normativo do frontmatter
- O schema rico do Claude Code (`allowed-tools`, `argument-hint`, `category`, `tags`, `version`, `model`, `parameters`) **não se aplica** — escopo de tools, hints de argumento e contexto devem virar **prosa no corpo** do workflow
- Automação a nível de evento (hooks) **não** vai no frontmatter — vive em `.agents/hooks.json` (ver §1.3 e [integrations.md](./integrations.md))

### 1.2 Corpo do workflow

Após o frontmatter:

```markdown
# <Título descritivo do workflow>

## Objetivo
<O que este workflow entrega>

## Quando usar
<Gatilhos, casos de uso típicos>

## Etapas
<Passo a passo executável>

## Saída esperada
<Artefatos, mudanças, output ao usuário>

## Exemplos
<Invocações reais>
```

Workflows curtos (< 50 linhas) podem omitir seções não aplicáveis, mas **devem manter frontmatter `description` + título + objetivo**.

Quando o workflow executa **ações sensíveis** (git, escrita de arquivos, operações de Task Manager), deve descrever **em prosa** no corpo o escopo de operação esperado e o uso real de ferramentas — não há mais campo `allowed-tools`. Permissões efetivas (Allow/Deny/Ask) são configuradas no IDE do Antigravity e documentadas no getting-started.

### 1.3 Hooks de ciclo de vida

Automação por evento vive em `.agents/hooks.json` (não no workflow). O Antigravity suporta:

- `PreToolUse` / `PostToolUse` — antes/depois de uma ferramenta executar (ex: lint/validação pós-escrita)
- `PreInvocation` / `PostInvocation` — antes/depois de invocar um workflow/persona (ex: detecção de provider de Task Manager via `.env`)

Workflows não devem reimplementar lógica que pertence a um hook. Referência: [integrations.md](./integrations.md) e [architecture.md](./architecture.md).

---

## 2. Categorias válidas

Workflows são flat, mas o **prefixo de categoria** no nome do arquivo classifica o workflow. Categorias com asterisco representam **as três dimensões peer do ciclo Onion**.

| Prefixo de categoria | Função | Volume típico |
|---|---|---|
| `product-` (*) | Discovery, especificação, decomposição de tarefas, branding, reuniões | 20+ |
| `engineer-` (*) | Planejamento e implementação faseada de features | 10+ |
| `docs-` | Geração e validação de documentação (incluindo `/docs-build-compliance-docs` da dimensão compliance) | 10+ |
| `git-` | GitFlow, feature/release/hotfix, code review | 10+ |
| `meta-` | Criação de workflows/personas/skills/KBs, integração | 8+ |
| `validate-` | Validação de testes, QA, workflows colaborativos | 4+ |
| `test-` | Estratégias de teste (unit, integration, e2e) | 3 |
| `development-` | Workflows de desenvolvimento específicos | 1+ |
| `quick-` | Análises pontuais rápidas | 1+ |
| (root) | `onion.md` e `warm-up.md` — pontos de entrada | 2 |

> Os antigos "comandos comuns" (`common/templates/`, `common/prompts/`) e READMEs de categoria **não são workflows** — viram docs/includes e não vivem em `.agents/workflows/` (ver [ADR-001](../analysis/onion-antigravity-migration-adr-2026-06.md)).

Subcategorias do Claude Code (ex: `git/feature/start`) são **achatadas** no nome com hífen: `git-feature-start.md`.

---

## 3. Workflows faseados — INVARIANTE DO FRAMEWORK

**Princípio**: o Onion implementa workflows faseados como **mecanismo central**. Múltiplos workflows cobrindo fases distintas de um mesmo fluxo, com estado retomável persistido em **Artifacts do Antigravity** (task lists, implementation plans, walkthroughs) e/ou em `docs/sessions/<feature>/`, são **valor de design**, não duplicação.

### 3.1 Workflows canônicos

Os **dois workflows abaixo são invariantes** do framework. Devem ser preservados intactos. Qualquer proposta de fusão deve ser rejeitada por `@metaspec-gate-keeper`.

**Workflow de Engenharia** (6 fases):

```
engineer-plan → engineer-start → engineer-work → engineer-pre-pr → engineer-pr → engineer-pr-update
```

- `plan` — analisa requisitos e cria plano estruturado
- `start` — cria sessão de desenvolvimento e analisa tasks
- `work` — retoma sessão e identifica próxima fase
- `pre-pr` — valida padrões e qualidade antes do PR
- `pr` — cria Pull Request com GitFlow e sync
- `pr-update` — atualiza PR existente

**Workflow de Produto** (6 fases):

```
product-collect → product-refine → product-spec → product-task → product-estimate → product-feature
```

- `collect` — coleta ideias de features ou bugs
- `refine` — refina via perguntas de esclarecimento
- `spec` — cria especificação a partir de requisitos
- `task` — decompõe em tasks/subtasks/action items
- `estimate` — aplica framework de story points
- `feature` — cria task no gerenciador configurado

### 3.2 Regras para workflows faseados

1. Cada fase deve ter **input claro** (estado do artifact/sessão ou argumentos), **output claro** (próximo estado) e ser **invocável isoladamente** quando o estado permite
2. Estado entre fases é persistido em **Artifacts do Antigravity** e/ou `docs/sessions/<feature>/`
3. Fases nomeadas explicitamente, sem ambiguidade de ordem
4. Novos workflows similares devem seguir o mesmo padrão (estado persistente, fases nomeadas, retomável)
5. **Proibido fundir fases** de workflow ativo sem justificativa formal aprovada via PR específico para esta meta-spec

### 3.3 Padrão para identificar workflow faseado

Características de um workflow que faz parte de fluxo faseado:

- Tem prefixo de categoria que representa dimensão do ciclo (`product-`, `engineer-`)
- Lê ou escreve estado em Artifacts do Antigravity e/ou `docs/sessions/`
- Tem nome que sugere fase explícita (verbo de ação temporal: `start`, `work`, `pre-pr`, `pr-update`)
- Documenta a posição no ciclo no corpo do workflow

---

## 4. Convenção de naming

- **Filename**: `<categoria>-<nome>.md`, kebab-case (`engineer-pre-pr.md`, `docs-build-tech-docs.md`)
- **Path completo**: `.agents/workflows/<categoria>-<nome>.md` (sempre flat — sem subdiretórios)
- **Invocação**: usuário invoca com `/<categoria>-<nome>`; subcategorias achatam com hífen → `/<categoria>-<sub>-<nome>` (ex: `/git-feature-start`)

### 4.1 Política de duplicação de nomes entre categorias

O prefixo de categoria obrigatório no nome flat **resolve por construção** a maior parte das colisões que existiam no Claude Code (vários `start`/`finish`/`help`/`warm-up`). Mesmo assim, esta política torna a régua explícita.

| Nome base | Categorias | Workflow canônico | Variantes (sempre com prefixo) |
|---|---|---|---|
| `warm-up` | `product-`, `engineer-`, root (`warm-up.md`) | root (`/warm-up`) | `/product-warm-up`, `/engineer-warm-up` são specializations contextuais |
| `start` | `engineer-`, `git-feature-`, `git-hotfix-`, `git-release-` | `/engineer-start` (sessão de desenvolvimento) | `/git-feature-start`, `/git-hotfix-start`, `/git-release-start` são fluxos GitFlow específicos |
| `finish` | `git-feature-`, `git-hotfix-`, `git-release-` | Específico por subcategoria GitFlow | Sempre invocar com prefixo completo |
| `help` | `git-`, `docs-` | Específico por categoria | Ajuda contextual da categoria |
| `estimate` | `product-`, `validate-qa-points-` | `/product-estimate` (story points de feature) | `/validate-qa-points-estimate` é QA story points |
| `plan` | `engineer-`, `product-light-arch` (similar) | `/engineer-plan` (planejamento de implementação) | `/product-light-arch` é design de arquitetura leve |
| `check` | `product-`, `product-task-check` | `/product-check` (verificação contra meta-specs) | `/product-task-check` é verificação de task |

**Regra geral**:

- O prefixo de categoria no nome do arquivo é **obrigatório** e garante unicidade do slug de invocação
- Quando há canônico para um nome base curto, novos workflows devem usar o canônico ou nome explícito com prefixo
- Renomes para resolver ambiguidade devem manter aliases temporários para não quebrar invocações existentes

---

## 5. Limites de tamanho

| Limite | Linhas | Tratamento |
|---|---|---|
| Recomendado | até 400 | OK |
| Acima do limite | > 400 | Refatoração obrigatória antes de merge — extrair conteúdo |

Workflows que excederem 400 linhas devem extrair partes para:

- Skills em `.agents/skills/` (cérebro de orquestração reutilizável, ≤ 500 linhas)
- Knowledge bases em `docs/knowledge-base/`
- Includes/fragmentos de documentação em `docs/`
- Sub-workflows referenciados

### 5.1 Isenções (não são workflows invocáveis)

O limite acima aplica-se a **workflows invocáveis** (`/<categoria>-<nome>`). São **isentos** por natureza:

- **Fragmentos de template e prompt** (antigos `common/templates/`, `common/prompts/`) — migraram para `docs/` como includes/referência; tamanho é inerente ao template e não contam como workflow.
- **READMEs / índices** de categoria — são índices e não contam como workflow.

---

## 6. Modularização

Workflows podem reaproveitar:

- **Skills** em `.agents/skills/` (cérebro de orquestração)
- **Personas/subagents** declaradas em `.agents/AGENTS.md` (delegação especializada)
- **Referência** em `docs/reference/` (ex: Task Manager Abstraction)
- **Knowledge bases** em `docs/knowledge-base/`

Workflow que duplica >50 linhas de outro workflow deve refatorar para skill ou fragmento de documentação compartilhado.

---

## 7. Exemplos de conformidade

### Exemplo conforme (workflow faseado)

Arquivo: `.agents/workflows/engineer-start.md`

- Frontmatter com `description`
- Prefixo `engineer-` (dimensão de engenharia)
- Faz parte do workflow canônico
- Persiste estado em Artifact / `docs/sessions/`
- Nome reflete fase explícita

**Veredito**: `@metaspec-gate-keeper` aprova.

### Exemplo conforme (workflow atômico)

Arquivo: `.agents/workflows/meta-setup-integration.md`

- Frontmatter com `description`
- Prefixo `meta-` (categoria válida)
- Não faz parte de workflow faseado — função atômica clara
- Tamanho dentro do limite (≤ 400)
- Escopo de operação descrito em prosa no corpo

**Veredito**: aprovado.

### Exemplo quase-conforme

Arquivo hipotético: `.agents/workflows/validate-test-strategy-analyze.md` (520 linhas)

- Frontmatter correto
- Prefixo de categoria válido
- Tamanho acima do limite (400)

**Veredito**: requer refatoração antes de próximo merge tocando este arquivo (extrair skill/KB).

### Exemplo não-conforme

Arquivo hipotético: `.agents/workflows/MyCommand.md`

- Sem prefixo de categoria válido
- Filename PascalCase em vez de kebab-case
- Frontmatter com `allowed-tools`/`category` (schema do Claude Code, removido)

**Veredito**: rejeitado.

---

## 8. Proibições explícitas

- **Proibido** fundir workflows de fluxo faseado canônico (`engineer-*` ou `product-*`) sem PR específico para esta meta-spec
- **Proibido** criar workflow com prefixo de categoria fora da lista válida
- **Proibido** criar workflow sem frontmatter `description`
- **Proibido** usar schema de frontmatter do Claude Code (`allowed-tools`, `category`, `tags`, `version`, `model`, `parameters`)
- **Proibido** filename em formato diferente de kebab-case ou com subdiretórios em `.agents/workflows/`

---

## 9. Versionamento e mudanças

Mudanças nesta spec exigem:

1. PR específico para `docs/meta-specs/commands.md`
2. Atualização do campo `version` no frontmatter
3. Validação por `@metaspec-gate-keeper` em workflows existentes
4. Especificamente para mudança em workflows canônicos (Seção 3.1): aprovação registrada em commit message com link para issue de discussão
