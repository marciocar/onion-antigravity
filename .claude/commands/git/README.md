# 🌿 Git Commands - Sistema Onion

Índice da categoria `git/` — comandos de versionamento e workflows GitFlow do Sistema Onion, com integração ao Task Manager ativo (`TASK_MANAGER_PROVIDER`) e a sessões em `.claude/sessions/`.

> **Para detalhes de workflow, troubleshooting e versionamento semântico**, consulte a referência canônica:
> [`docs/knowledge-base/frameworks/gitflow-patterns.md`](../../../docs/knowledge-base/frameworks/gitflow-patterns.md)
> e a skill `common:prompts:git-workflow-patterns`. Este README não duplica esse conteúdo — apenas aponta para os comandos.

## ⚡ Como usar

Todos são [Claude Code Commands](https://docs.claude.com/en/docs/claude-code/slash-commands), digitados **no chat da Claude Code** (não no terminal):

```
/git/init
/git/feature/start "user-authentication"
/git/release/start "minor"
```

## 📋 Comandos

| Comando | Finalidade |
|---------|-----------|
| `/git/help` | Ajuda contextual e quick reference dos workflows GitFlow |
| `/git/init` | Inicializar repositório com GitFlow e convenções padrão |
| `/git/fast-commit` | Adicionar todas as mudanças e fazer commit rápido |
| `/git/code-review` | Setup, validação e otimização do ChatGPT-CodeReview no projeto |
| `/git/sync [branch]` | Sincronização pós-merge (checkout + pull + cleanup de branch) |
| `/git/feature/start "nome"` | Iniciar feature branch GitFlow com ambiente configurado |
| `/git/feature/publish` | Publicar feature branch no remote para colaboração |
| `/git/feature/finish` | Merge feature → develop com cleanup |
| `/git/release/start "ver"` | Iniciar release branch com versionamento e changelog |
| `/git/release/finish` | Finalizar release: merge, tag e publicação |
| `/git/hotfix/start "nome"` | Iniciar hotfix branch para correção emergencial em produção |
| `/git/hotfix/finish` | Finalizar hotfix: merge main + develop, tag e deploy |

`/git/release/start` aceita `"vX.Y.Z"` (versão exata) ou `patch` / `minor` / `major` (auto-bump semver).

## 🔁 Fluxos principais (resumo)

Os passos detalhados de cada fluxo estão no KB [`gitflow-patterns.md`](../../../docs/knowledge-base/frameworks/gitflow-patterns.md).

- **Feature**: `/git/feature/start` → desenvolvimento (`/engineer/start` → `/engineer/work`) → `/git/feature/finish` → `/git/sync develop`
- **Release**: `/git/release/start "minor"` → testes/validação → `/git/release/finish` → `/git/sync main`
- **Hotfix (separado)**: `/git/hotfix/start "bug"` → fix → `/git/hotfix/finish`
- **Hotfix (híbrido)**: `/engineer/hotfix "desc" --params` (cria task + sessão + branch) → fix → `/git/hotfix/finish`

## 🔗 Integração e referências

- **Engineering**: `/engineer/start`, `/engineer/work`, `/engineer/pr`, `/engineer/hotfix`
- **Product / Task Manager**: `/product/task`, `/product/task-check`
- **Agentes**: `@gitflow-specialist` (orientação detalhada), especialista do provider ativo (`@jira-specialist`, `@clickup-specialist`, …) para sync de tasks
- **Referência GitFlow**: [`docs/knowledge-base/frameworks/gitflow-patterns.md`](../../../docs/knowledge-base/frameworks/gitflow-patterns.md) · skill `common:prompts:git-workflow-patterns`

**Para começar**: `/git/help` ou `/git/init`.
