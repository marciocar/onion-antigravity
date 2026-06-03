---
name: onion
description: >
  Orquestrador mestre do Sistema Onion. Use quando o usuário precisar de orientação
  sobre por onde começar, qual workflow ou persona usar, como executar um fluxo de
  trabalho (feature, hotfix, PR, documentação, produto, testes), como configurar
  integrações (Jira, ClickUp, Asana, Linear), ou qualquer navegação dentro do
  framework. Ative também quando perguntarem "o que faço agora?", "próximos passos",
  "como funciona o sistema?", "qual persona para X?", "como crio Y?", mesmo sem
  mencionar "onion" explicitamente.
---

## Estado atual do projeto

Antes de rotear, verifique o contexto:
- **Provider ativo**: ler `TASK_MANAGER_PROVIDER` no `.env` (ver regra `task-manager-routing`)
- **Contexto de feature**: checar `docs/sessions/` (ou os Artifacts ativos do Antigravity)
- **Branch atual**: `git branch --show-current`

---

## Routing por intenção

### Desenvolvimento de Feature
| Intenção | Workflow |
|----------|----------|
| Criar task / planejar feature | `/product-task` |
| Iniciar desenvolvimento | `/engineer-start` |
| Continuar trabalho em feature | `/engineer-work` |
| Preparar PR (lint, testes, review) | `/engineer-pre-pr` |
| Criar Pull Request | `/engineer-pr` |
| Atualizar PR existente | `/engineer-pr-update` |
| Hotfix urgente em produção | `/engineer-hotfix` |
| Bump de versão (semver) | `/engineer-bump` |

**Sequência completa de feature:**
```
/product-task → /engineer-start → /engineer-work → /engineer-pre-pr → /engineer-pr
```
**Sequência de hotfix:**
```
/engineer-hotfix → /engineer-work → /engineer-pr → /git-hotfix-finish
```

---

### Produto e Discovery
| Intenção | Workflow |
|----------|----------|
| Coletar requisitos / ideias | `/product-collect` |
| Refinar requisitos | `/product-refine` |
| Especificação de produto | `/product-spec` |
| Estimar story points | `/product-estimate` |
| Warm-up de produto | `/product-warm-up` |
| Transcrever áudio / reunião | `/product-whisper` |
| Extrair ata de reunião | `/product-extract-meeting` |
| Consolidar múltiplas reuniões | `/product-consolidate-meetings` |
| Converter documento em tasks | `/product-convert-to-tasks` |
| Analisar dor do cliente | `/product-analyze-pain-price` |
| Branding e posicionamento | `/product-branding` |

**Sequência de discovery:**
```
/product-collect → /product-refine → /product-spec → /product-feature
```

---

### Documentação
| Intenção | Workflow |
|----------|----------|
| Documentação técnica | `/docs-build-tech-docs` |
| Documentação de negócio | `/docs-build-business-docs` |
| Documentação de compliance | `/docs-build-compliance-docs` |
| Atualizar índice de docs | `/docs-build-index` |
| Engenharia reversa de projeto | `/docs-reverse-consolidate` |
| Validar documentação | `/docs-validate-docs` |

**Sequência de documentação:**
```
/docs-build-tech-docs → /docs-build-business-docs → /docs-build-index
```

---

### Criar componentes do Onion
| Intenção | Workflow |
|----------|----------|
| Novo workflow | `/meta-create-command` |
| Nova skill | `/meta-create-skill` |
| Nova persona/subagent | `/meta-create-agent` |
| Nova knowledge base | `/meta-create-knowledge-base` |
| Configurar integração | `/meta-setup-integration` |
| Análise de problema complexo | `/meta-analyze-complex-problem` |
| Validar contra meta-specs | `/meta-metaspec-validate` |

---

### Qualidade e Revisão
| Intenção | Workflow / Persona |
|----------|--------------------|
| Testes unitários | `/test-unit` |
| Testes de integração | `/test-integration` |
| Testes E2E | `/test-e2e` |
| Criar estratégia de teste | `/validate-test-strategy-create` |
| Estimar QA points | `/validate-qa-points-estimate` |
| Validar workflow do Onion | `/validate-workflow` |
| Code review | subagent de review |
| Conformidade arquitetural | `/meta-metaspec-validate` |

---

### Git e Versionamento
| Intenção | Workflow |
|----------|----------|
| Iniciar feature branch | `/git-feature-start` |
| Finalizar feature | `/git-feature-finish` |
| Publicar branch remota | `/git-feature-publish` |
| Sincronizar com GitFlow | `/git-sync` |
| Iniciar release | `/git-release-start` |
| Finalizar release | `/git-release-finish` |
| Commit rápido | `/git-fast-commit` |

---

### Task Managers (provider-aware)
Roteamento conforme `TASK_MANAGER_PROVIDER` (ver regra `task-manager-routing`):
- **ClickUp** — tasks, listas, custom fields, comments Unicode (via MCP)
- **Jira** — JQL, ADF, transitions, sprints/bulk
- **Asana / Linear** — agnóstico (decomposição)
- **Estratégia/gestão** — persona `@product` (qualquer provider)

---

### Contexto e navegação
| Intenção | Workflow |
|----------|----------|
| Warm-up geral | `/warm-up` |
| Warm-up de engenharia | `/engineer-warm-up` |
| Warm-up de produto | `/product-warm-up` |
| Listar ferramentas | `/meta-all-tools` |

---

## Gotchas críticos

**Task Manager Provider obrigatório** — antes de qualquer operação com tasks, ler
`TASK_MANAGER_PROVIDER` no `.env`. Se ausente/inválido: avisar e sugerir
`/meta-setup-integration`. Nunca inventar valores.

**Feature slug: sempre kebab-case** — `user-authentication` ✅, não `user_authentication`/`UserAuth`. Usado em branch Git e em `docs/sessions/<feature-slug>/`.

**Contexto de feature** — persistido como Artifacts do Antigravity (task lists,
implementation plans, walkthroughs) e/ou em `docs/sessions/<feature-slug>/`.

**Workflows vs Skills vs Personas**
- `/<categoria>-<comando>` → workflow (`.agents/workflows/`)
- conhecimento contextual → skill (`.agents/skills/`)
- persona/subagent especializado → `.agents/AGENTS.md` + Agent Manager

**Search Jira 2025** — `GET /rest/api/3/search` foi removido. Usar
`POST /rest/api/3/search/jql` com `nextPageToken`.

---

## Referências
- Personas: `.agents/AGENTS.md`
- Regras: `.agents/rules/`
- Task Manager Abstraction: `docs/knowledge-base/concepts/task-manager-abstraction.md`
- Índice geral: `docs/INDEX.md`
- Guia de workflows: `docs/onion/commands-guide.md`
