# AGENTS.md — Instruções de Operação para o Google Antigravity

> **Leia este arquivo primeiro.** Ele diz ao agente do Antigravity **o que este
> repositório é e como operar nele**. Os detalhes vivem em `.agents/` (rules,
> workflows, skills) e em `docs/`.

---

## 1. O que é este projeto

Este é o **Sistema Onion** — um **framework template em `.agents/`** para
orquestrar o ciclo completo de desenvolvimento no **Google Antigravity**, em
**três dimensões peer**: **produto**, **engenharia** e **compliance/governança**.

Não é produto npm, não é distribuído publicamente, não tem CLI standalone próprio.
Plataforma única: Google Antigravity.

---

## 2. Como operar (sempre)

1. **Aplique as rules always-on** em `.agents/rules/` antes de qualquer ação:
   - `onion-identity.md` — o que é (e o que não é) o Onion
   - `language-standards.md` — código em inglês; docs/comentários/commits-descrição em pt-BR
   - `task-manager-routing.md` — **detecte `TASK_MANAGER_PROVIDER` no `.env` antes de operar com tasks**
   - `onion-conventions.md` — estrutura, naming e limites dos artefatos
2. **Responda em português brasileiro** (pt-BR). Código e identificadores em inglês.
3. **Antes de mexer com tasks**: leia o `.env`. Provider válido = `jira | clickup | asana | linear | none`. Se faltar configuração, avise e sugira `/meta-setup-integration`. Nunca invente valores.
4. **Use Planning Mode + Artifacts** (implementation plan, task list, walkthrough) para mudanças não-triviais; deixe o plano revisável antes de executar.
5. **Não reintroduza** `.claude/` nem `CLAUDE.md` — o Onion migrou para Antigravity.

---

## 3. Como rotear o pedido do usuário

Quando estiver em dúvida sobre por onde começar, ative a skill **`onion`**
(`.agents/skills/onion/SKILL.md`) — ela é o índice de roteamento por intenção.

- **Workflows** (`.agents/workflows/`) — invocáveis com `/<categoria>-<comando>`.
  Categorias: `product-`, `engineer-`, `git-`, `docs-`, `meta-`, `validate-`, `test-`, `quick-`, `development-`. Entrada: `/onion` e `/warm-up`.
- **Skills** (`.agents/skills/`) — conhecimento contextual: `onion` (navegação), `onion-validation` (validar artefatos).
- **Personas** (`.agents/AGENTS.md`) — leads das dimensões (`@product`, `@engineer`, `@compliance`) e o orquestrador `@onion`, além de subagents de execução.

### Fluxos faseados invariantes (não pule etapas)
- **Feature**: `/product-task` → `/engineer-start` → `/engineer-work` → `/engineer-pre-pr` → `/engineer-pr`
- **Discovery**: `/product-collect` → `/product-refine` → `/product-spec` → `/product-feature`
- **Hotfix**: `/engineer-hotfix` → `/engineer-work` → `/engineer-pr` → `/git-hotfix-finish`

---

## 4. Onde tudo vive

```
.agents/
├── AGENTS.md                 # Personas / equipe de IA (detalhe)
├── rules/                    # System instructions always-on (§2)
├── workflows/                # 78 workflows /-invocáveis
├── skills/                   # onion, onion-validation
├── hooks.json                # Hooks de ciclo de vida
└── mcp_config.example.json   # Template MCP → copie para ~/.gemini/config/mcp_config.json

docs/
├── meta-specs/               # Constituição L0 (padrões obrigatórios)
├── knowledge-base/           # KBs estruturadas (incl. platforms/antigravity.md)
├── reference/task-manager/   # Task Manager Abstraction (Jira/ClickUp/Asana/Linear)
├── onion/                    # Guias operacionais
└── analysis/                 # Análises + ADRs (incl. ADR-001 da migração)
```

Config global do Antigravity (não versionada): `~/.gemini/` (`mcp_config.json`, `GEMINI.md`, skills compartilhadas).

---

## 5. Integrações (MCP / Task Manager)

- Copie `.agents/mcp_config.example.json` → `~/.gemini/config/mcp_config.json` e ajuste.
- Formato: `{"mcpServers": {...}}`; use `serverUrl` para remoto (não `httpUrl`); sem `timeout` no top-level; sem comentários inline.
- Detalhes por provider e formatação (ADF Jira, Markdown/Unicode ClickUp): `.agents/rules/task-manager-routing.md` + `docs/reference/task-manager/`.

---

## 6. Padrões e qualidade

- **Workflows** usam frontmatter só com `description`; **skills** usam `name` + `description`.
- Limites: workflow ≤ 400 linhas; skill ≤ 500 linhas.
- Para criar/validar artefatos: skill `onion-validation` e workflow `/meta-metaspec-validate`.
- A fonte canônica de padrões é `docs/meta-specs/` (constituição L0).

---

**Comece por:** `/warm-up` (contexto geral) e depois `/onion` (roteamento inteligente).
