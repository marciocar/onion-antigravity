# Google Antigravity — Knowledge Base

---

## 📋 Metadata

| Campo | Valor |
|-------|-------|
| **Versão** | 2.0.0 |
| **Data de Criação** | 2026-06-03 |
| **Última Atualização** | 2026-06-03 |
| **Categoria** | platforms |
| **Status de Documentação** | ✅ Verificado — fontes oficiais (codelabs/blog) + comunidade indexada. O site `antigravity.google/docs/*` é SPA JS (não-crawlável); fatos abaixo vêm de Google Codelabs, Medium/Google Cloud Community e fóruns oficiais |
| **Fontes Principais** | Codelabs Google · Medium Google Cloud Community · antigravity.google (sitemap) — ver §Referências |

---

## 📋 Visão Geral

**Google Antigravity** é uma **plataforma de desenvolvimento agêntica** ("agentic development platform") da Google — um IDE agent-first baseado em fork do VS Code (download para Mac/Windows/Linux), lançado em nov/2025 junto ao Gemini 3. É descrito como *"Mission Control for managing autonomous agents that can plan, code, and even browse the web"*.

Premissa central: a IA não é só autocomplete, é um **ator autônomo** que planeja, executa, valida e itera tarefas de engenharia com mínima intervenção humana. Concorrente direto de Claude Code, Cursor e Windsurf (derivado da linhagem Windsurf).

**Duas superfícies principais:**
- **Editor View** — VS Code familiar (explorer, syntax highlight, terminal integrado `Ctrl+\``). Toggle com `Cmd+E`.
- **Agent Manager** — dashboard de orquestração que roda **múltiplos agentes concorrentes**, mostrando status, artifacts gerados e aprovações pendentes.

**Três surfaces de atuação do agente:** Editor · Terminal · Browser (Chrome via plugin).

---

## 🎯 Casos de Uso

- **Desenvolvimento agêntico multi-tarefa** — despachar vários agentes em paralelo pelo Agent Manager
- **Planning Mode** — gerar plano estruturado (implementation plan) antes de codar, com revisão humana
- **Automação repetível** — Workflows (`/comando`) para processos multi-step recorrentes
- **Verificação via browser** — agente navega/grava interações para validar mudanças de frontend

**Quando NÃO usar:**
- Pipelines headless/CI puros sem IDE (use a Antigravity CLI em Go, não o IDE)
- Projetos que exigem controle determinístico total sem ator autônomo
- Quando já há investimento alto em outro IDE agêntico sem ganho claro de migração

---

## ⚡ Quick Start

```bash
# 1. Baixar o IDE em https://antigravity.google/download (Mac/Win/Linux)
# 2. Autenticar
antigravity auth login            # (CLI, escrita em Go)
# 3. Migrar de Gemini CLI (opcional)
antigravity migrate --from-gemini-cli
# 4. Invocar via CLI
antigravity "Add input validation to the registration endpoint"
```

Customizações (Rules/Workflows/Skills) via menu **`...` → Customizations** no Editor, com escopo **global ou por workspace**.

---

## 🔧 Configuração e Estrutura de Arquivos (VERIFICADO)

### Workspace (versionado no repositório) — diretório `.agents/`

> ⚠️ **Naming**: é `.agents/` (plural) desde a v1.18.4; antes era `.agent/` (singular). Há docs/tutoriais que ainda citam `.agent/` — usar **`.agents/`**.

```
<PROJECT>/.agents/
├── AGENTS.md                      # Personas da "equipe IA" (PM, Engineer, QA, DevOps)
├── rules/*.md                     # Rules — system instructions always-on
├── workflows/<nome>.md            # Workflows — saved prompts invocados com /<nome>
├── skills/<nome>/SKILL.md         # Skills — pacotes de conhecimento contextual
└── hooks.json                     # Hooks de ciclo de vida (workspace tem precedência)
```

### Global (home do usuário, NÃO versionado) — `~/.gemini/`

```
~/.gemini/
├── GEMINI.md                      # Rules globais
├── config/
│   ├── mcp_config.json            # Servidores MCP
│   └── plugins/
├── skills/<nome>/SKILL.md         # Skills compartilhadas (todas as ferramentas Agy)
├── antigravity/
│   ├── browserAllowlist.txt       # Allowlist de URLs do browser-agent
│   └── global_workflows/*.md      # Workflows globais
├── antigravity-cli/{mcp/, settings.json, skills/}
└── antigravity-ide/brain/
```

### Rules (`.agents/rules/*.md`)
System-level instructions always-on que guiam geração de código/testes. Ex.: *"All code must follow PEP 8"*, *"Each new feature goes in its own file"*. Análogo ao `CLAUDE.md` + skills always-on do Claude Code.

### Workflows (`.agents/workflows/<nome>.md`)
Saved prompts disparados sob demanda com `/<nome>`. **Frontmatter mínimo:**
```markdown
---
description: Start the Autonomous AI Developer Pipeline with a new idea
---
<instruções de orquestração / corpo do prompt>
```
Invocação: `/startcycle <idea>`. São **flat** (sem categorias aninhadas) → exige prefixo para evitar colisão (ex.: `engineer-start`, `product-task`).

### Skills (`.agents/skills/<nome>/SKILL.md`)
**Formato idêntico ao padrão agentskills.io / Claude Code** — port quase 1:1:
```yaml
---
name: skill-name              # lowercase-hyphen; default = nome do dir
description: frase de trigger (OBRIGATÓRIA — usada para matching semântico)
---
```
+ corpo Markdown (Goal, instruções, exemplos, constraints). Pode incluir `scripts/`, `references/`, `assets/`. Escopos: workspace (`.agents/skills/`), shared (`~/.gemini/skills/`), CLI-only (`~/.gemini/antigravity-cli/skills/`).

### AGENTS.md (personas / "AI team")
Define papéis com template: **Role (@handle) · Goal · Traits · Constraint**. Ex. de 4 papéis: `@pm` (Product Manager), `@engineer` (Full-Stack), `@qa`, `@devops`.

### Hooks (`hooks.json` — global + workspace, workspace tem precedência)
```json
{
  "my-linter-hook": {
    "PostToolUse": [
      { "matcher": "run_command",
        "hooks": [ { "type": "command", "command": "./scripts/lint.sh", "timeout": 10 } ] }
    ]
  }
}
```
**Estágios:** `PreToolUse` · `PostToolUse` · `PreInvocation` (antes do model call) · `PostInvocation`. Quase idêntico aos hooks do `settings.json` do Claude Code.

### MCP (`~/.gemini/config/mcp_config.json`)
```json
{ "mcpServers": { "gcloud": { "command": "npx", "args": ["-y", "@google-cloud/gcloud-mcp"] } } }
```
Use `serverUrl` para remoto (não o deprecado `httpUrl`); **sem** `timeout` no top-level; **sem** comentários inline. Editar via `...` → MCP Servers → View raw config.

### Permissions
Listas **Allow / Deny / Ask** controlando execução de comandos e acesso a arquivos + **Browser URL Allowlist** (anti prompt-injection).

### Modelos & Modos
Seleção de modelo (Gemini 3 Pro et al.); **Fast Mode** (execução direta) vs **Planning Mode** (plano antes de implementar).

---

## 🎨 Artifacts (conceito central)

Em vez de logs opacos de tool-calls, agentes emitem **Artifacts** verificáveis, com feedback estilo Google Docs:
- **Task Lists** — planejamento antes de codar
- **Implementation Plans** — arquitetura técnica das mudanças propostas
- **Walkthroughs** — resumo pós-conclusão com resultados de verificação
- **Code Diffs & Screenshots** · **Browser Recordings**

---

## 💡 Best Practices

- **`description` de skills/workflows deve ser específica** — *"Database tools"* é insuficiente; o LLM usa para matching semântico
- **Rules curtas e imperativas** — system instructions, não prosa
- **Workflows prefixados por domínio** — evita colisão na lista flat de `/comandos`
- **Skills com scripts determinísticos** — delegar validação a `scripts/*.py` quando há lógica procedural
- **Hooks de workspace** sobrepõem globais — usar para lint/teste pós-tool
- **Planning Mode + artifact review** para mudanças grandes — nunca aplicar sem revisar o implementation plan

---

## ⚠️ Limitações e Gotchas

- **Docs oficiais são SPA JS** — não-crawláveis; depender de codelabs/comunidade para detalhes
- **Naming `.agent` vs `.agents`** — tutoriais antigos divergem; usar `.agents/` (plural, ≥1.18.4)
- **Workflows são flat** — sem hierarquia de categorias como `/engineer/start` do Claude Code
- **`mcp_config.json`**: sem comentários inline, sem `timeout` top-level, `serverUrl` (não `httpUrl`)
- **Skills `paths`/`allowed-tools`** do Claude Code não têm equivalente direto no SKILL.md do Antigravity — dobrar trigger na `description` e restrições em permissions/hooks
- **Vulnerabilidade conhecida** — pesquisa Mindgard reportou execução de código persistente; revisar permissions/allowlist
- **Antigravity 2.0** (I/O 2026) adiciona CLI (Go), subagents registráveis, scheduled tasks (`/schedule`), voice — algumas docs ainda esparsas

---

## 🔗 Integração com o Sistema Onion (mapa de migração)

| Onion (`.claude/`) | Antigravity (`.agents/` + `~/.gemini/`) | Fidelidade |
|--------------------|------------------------------------------|------------|
| `CLAUDE.md` | `.agents/AGENTS.md` + `.agents/rules/*.md` | alta |
| `.claude/skills/<n>/SKILL.md` | `.agents/skills/<n>/SKILL.md` | **quase 1:1** (mesmo schema) |
| `.claude/commands/<cat>/<n>.md` | `.agents/workflows/<cat>-<n>.md` → `/<cat>-<n>` | alta (flatten + prefixo) |
| `.claude/agents/<cat>/<n>.md` (49) | personas em `AGENTS.md` + subagents (2.0) + skills | média (consolidar) |
| `.claude/settings.json` hooks | `hooks.json` (Pre/PostToolUse, Pre/PostInvocation) | alta |
| `settings.json` permissions | Allow/Deny/Ask + Browser Allowlist | alta |
| `.claude/utils/task-manager/**` + MCP | `~/.gemini/config/mcp_config.json` + workflows | alta |
| `.claude/sessions/<feature>/` | Artifacts (task lists, implementation plans, walkthroughs) | conceitual |

**Conclusão para a migração**: o modelo de extensibilidade do Antigravity é **quase isomórfico** ao do Claude Code — Rules/Workflows/Skills/Hooks/MCP têm equivalentes diretos. A migração é majoritariamente **estrutural** (renomear/realocar + ajustar frontmatter), com consolidação adaptativa apenas nos 49 agentes (→ personas/subagents/skills) e no flatten dos workflows.

**Cross-links:** [Runflow](./runflow.md) · [AI Agent Design Patterns](../concepts/ai-agent-design-patterns.md) · [Specification-Driven AI Abstraction Layer](../concepts/specification-driven-ai-abstraction-layer.md)

---

## 🔗 Referências

- [Getting Started with Google Antigravity — Google Codelabs](https://codelabs.developers.google.com/getting-started-google-antigravity)
- [Build Autonomous Developer Pipelines using agents.md and skills.md — Google Codelabs](https://codelabs.developers.google.com/autonomous-ai-developer-pipelines-antigravity)
- [How to Build Custom Skills in Google Antigravity — Medium/Google Cloud](https://medium.com/google-cloud/tutorial-getting-started-with-antigravity-skills-864041811e0d)
- [Configuring MCP Servers and Skills for Antigravity CLI and IDE — Dazbo, Medium/Google Cloud](https://medium.com/google-cloud/configuring-mcp-servers-and-skills-for-antigravity-cli-and-ide-a938c7eebb78)
- [AntiGravity: Full guide — install to custom rules, workflows, MCP — codemeetai](https://codemeetai.substack.com/p/antigravity-full-guide-from-install)
- [Tutorial: Getting Started — Romin Irani, Medium/Google Cloud](https://medium.com/google-cloud/tutorial-getting-started-with-google-antigravity-b5cc74c103c2)
- [New folder for RULES? — Google AI Developers Forum](https://discuss.ai.google.dev/t/new-folder-for-rules/126165)
- [Antigravity hooks templates — GitHub fpozoc/antigravity-hooks](https://github.com/fpozoc/antigravity-hooks)
- [Antigravity 2.0 deep dive (I/O 2026) — antigravity.google/blog](https://antigravity.google/blog/google-io-2026-feature-deep-dive)
- [Forced Descent: persistent code execution vuln — Mindgard](https://mindgard.ai/blog/google-antigravity-persistent-code-execution-vulnerability)

---

**Última atualização**: 2026-06-03
**Fonte principal**: Google Codelabs + Medium/Google Cloud Community (docs oficiais são SPA JS não-crawlável)
