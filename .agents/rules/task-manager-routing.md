# Regra: Detecção e Roteamento de Task Manager

O Sistema Onion é **provider-agnóstico**. Antes de operar com tasks, **sempre
verifique qual provider está ativo** lendo `TASK_MANAGER_PROVIDER` no `.env`.

> A detecção do provider é responsabilidade desta regra: ao iniciar qualquer
> trabalho com tasks, o agente deve ler o `.env` primeiro. Um hook `PreInvocation`
> opcional em `.agents/hooks.json` pode surfacear o provider automaticamente.

## Fluxo obrigatório
1. **Carregar** variáveis do `.env` (ex.: `set -a; source .env; set +a`)
2. **Ler** `TASK_MANAGER_PROVIDER` → `jira` | `clickup` | `asana` | `linear` | `none`
3. **Conferir** variáveis específicas do provider ativo (tabela abaixo)
4. **Rotear** para a persona/skill correta e usar a formatação adequada

## Mapa Provider → Variáveis → Persona/Skill
| Provider | Variáveis obrigatórias | Opcionais | Roteamento |
|----------|------------------------|-----------|------------|
| `jira` | `JIRA_HOST`, `JIRA_EMAIL`, `JIRA_API_TOKEN` | `JIRA_PROJECT_KEY`, `JIRA_AUTH_TYPE`, `JIRA_API_VERSION` | especialista Jira (JQL, ADF, transitions, bulk) |
| `clickup` | `CLICKUP_API_TOKEN` | `CLICKUP_WORKSPACE_ID`, `CLICKUP_DEFAULT_LIST_ID` | especialista ClickUp (MCP, custom fields, comments Unicode) |
| `asana` | `ASANA_ACCESS_TOKEN` | `ASANA_WORKSPACE_ID`, `ASANA_DEFAULT_PROJECT_ID` | agnóstico (decomposição de tasks) |
| `linear` | `LINEAR_API_KEY` | `LINEAR_TEAM_ID` | agnóstico (decomposição de tasks) |
| `none` | — | — | operar offline, decompor localmente, **não** fazer API calls |

## Regras de delegação
- **Estratégia / gestão / priorização** → persona `@product` (qualquer provider)
- **Decomposição hierárquica** (agnóstica) → skill/workflow de decomposição
- **Operação técnica do provider** → especialista do provider ativo
- **Sem provider** (`none`) → offline; **não** tentar API calls

## Fallback gracioso
Se variáveis obrigatórias faltarem:
1. **Avisar** o usuário em pt-BR indicando qual variável falta
2. **Sugerir** `/meta-setup-integration`
3. **Não inventar** valores nem assumir outro provider

## Formatação por provider
- **Jira Cloud (v3)**: descrições e comments em **ADF** (JSON estruturado). Status só via `POST /issue/{key}/transitions`. Bulk via `POST /rest/api/3/issue/bulk`. Search via `POST /rest/api/3/search/jql` com `nextPageToken` (o antigo `/search` foi removido em maio/2025).
- **Jira Server/DC (v2)**: wiki markup ou plain text; search via `GET /rest/api/2/search`.
- **ClickUp**: descriptions em Markdown nativo; comments em Unicode visual (`━━━`, `∟`, `▶`, `◆`, `✅`) com timestamp + status obrigatórios.
- **Asana**: descrição em HTML notes (subset) ou plain text.
- **Linear**: Markdown nativo.

## MCP
Servidores de task manager são configurados em `~/.gemini/config/mcp_config.json`
(ver template `.agents/mcp_config.example.json`). Bulk-first ao operar em lote.
