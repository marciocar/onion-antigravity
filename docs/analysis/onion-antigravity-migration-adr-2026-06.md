---
template:
  type: adr
  version: 2.0
  category: architecture-decision
  adr_number: "001-migration"
decision_metadata:
  status: "Accepted"
  date: "2026-06-03"
  deciders: ["Marcio Carvalho", "Onion (assistido por IA)"]
  technical_story: "Migração Onion: Claude Code → Google Antigravity"
quality_attributes:
  maintainability: true
  usability: true
  reliability: true
related_decisions:
  supersedes: []
  relates_to:
    - docs/analysis/onion-review-2026-05.md
    - docs/knowledge-base/platforms/antigravity.md
  impacts:
    - docs/meta-specs/architecture.md
    - CLAUDE.md
    - README.md
implementation:
  timeline:
    short_term: "Fases 2-3 (camada Antigravity + docs)"
    medium_term: "Fases 4-5 (validação + remoção Claude)"
  next_review: "Após Fase 4 (gate de aceitação)"
---

# ADR-001: Migração do Sistema Onion de Claude Code para Google Antigravity

**Status:** Accepted
**Date:** 2026-06-03
**Deciders:** Marcio Carvalho, Onion (assistido por IA)
**Technical Story:** Plano `~/.claude/plans/onion-monte-um-plano-wise-hartmanis.md`

---

## 📋 Context and Problem Statement

O Sistema Onion é hoje um framework template em `.claude/` **acoplado exclusivamente ao Claude Code** (92 comandos, 49 agentes, 4 skills, `CLAUDE.md`, `settings.json`, Task Manager Abstraction). A constituição crava a plataforma única (`docs/meta-specs/architecture.md:216`). O objetivo é **portar o Onion para o Google Antigravity** preservando seu propósito (orquestração tri-dimensional produto/engenharia/compliance, workflows faseados retomáveis, Task Manager Abstraction multi-provider) e, ao final, **remover totalmente o Claude Code**.

A **Fase 0 (verificação)** estabeleceu o fato decisivo: o modelo de extensibilidade do Antigravity é **quase isomórfico** ao do Claude Code (ver [KB Antigravity](../knowledge-base/platforms/antigravity.md)). Logo, a migração é majoritariamente **estrutural**, não uma reescrita conceitual.

### Quality Attributes Affected
- [x] Maintainability — estrutura nativa do Antigravity, sem camada de tradução
- [x] Usability — workflows `/`-invocáveis, rules always-on, artifacts revisáveis
- [x] Reliability — gate de aceitação (Fase 4) antes de remover o Claude

---

## 🎯 Decision

Adotar **Adaptive Redesign**: re-expressar o Onion nas primitivas nativas do Antigravity, consolidando onde a fidelidade 1:1 não agrega, e remover o Claude Code ao final (hard removal). A estrutura-alvo é `.agents/` na raiz do repositório.

### Estrutura-alvo (`.agents/`)

```
.agents/
├── AGENTS.md                       # Personas core (onion + leads das 3 dimensões)
├── rules/                          # System instructions always-on
│   ├── onion-identity.md           # ← identidade (de CLAUDE.md + README)
│   ├── language-standards.md       # ← skill language-standards (pt-BR/en) + Formatação por Provider
│   ├── task-manager-routing.md     # ← seção "Task Manager" do CLAUDE.md (detecção .env)
│   └── onion-conventions.md        # ← skill onion-patterns (padrões estruturais)
├── workflows/                      # Comandos → saved prompts /-invocáveis (flat + prefixo)
│   ├── engineer-start.md           # ← /engineer/start
│   ├── engineer-plan.md  …         # ← workflow faseado de engenharia (invariante)
│   ├── product-collect.md … product-feature.md   # ← workflow faseado de produto (invariante)
│   ├── git-feature-start.md  …     # ← /git/feature/start  (nested vira cat-sub-cmd)
│   ├── docs-*.md · meta-*.md · validate-*.md · test-*.md
│   └── onion.md                    # ← /onion (entrypoint)
├── skills/                         # Conhecimento contextual (on-demand)
│   ├── onion/SKILL.md              # ← skill onion (navegação/orquestração)
│   ├── onion-validation/SKILL.md   # ← skill onion-validation
│   └── <specialist>/SKILL.md       # ← expertise reutilizável dos 49 agentes (story-points, testes, branding, …)
├── hooks.json                      # PreInvocation: detecção de provider; PostToolUse: lint/validação
└── mcp_config.example.json         # Template → ~/.gemini/config/mcp_config.json (task managers)
```

> Referência de abstração do Task Manager (`interface`/`types`/`detector`/`factory`/`adapters`) migra para `docs/` como documentação; os MCPs reais são configurados em `~/.gemini/config/mcp_config.json`.

### Política de mapeamento por primitiva

| Origem (`.claude/`) | Destino (`.agents/`) | Regra |
|---|---|---|
| `CLAUDE.md` | `AGENTS.md` + `rules/*.md` | Identidade+personas → AGENTS.md; seções normativas → rules |
| `skills/language-standards` | `rules/language-standards.md` | Always-on → vira rule |
| `skills/onion-patterns` | `rules/onion-conventions.md` | Padrões sempre válidos → rule |
| `skills/onion`, `skills/onion-validation` | `skills/<n>/SKILL.md` | On-demand → skills; frontmatter `name`+`description` (remover `paths`/`allowed-tools`, dobrar trigger na description) |
| `commands/<cat>/<n>.md` (92) | `workflows/<cat>-<n>.md` → `/<cat>-<n>` | Flatten + prefixo de categoria; nested = `<cat>-<sub>-<cmd>` |
| `commands/common/*` (12) + READMEs (3) | `docs/` ou includes | **Não** viram workflows (fragmentos/docs) |
| `agents/<cat>/<n>.md` (49) | `AGENTS.md` (personas core) + `skills/` (expertise) + subagents 2.0 | Consolidar: ~8-10 personas core; expertise profunda → skills lastreadas em `docs/knowledge-base/` |
| `settings.json` hooks (SessionStart) | `rules/task-manager-routing.md` + `hooks.json` (PreInvocation) | Antigravity não tem SessionStart → rule instrui ler `.env`; hook opcional injeta provider |
| `settings.json` permissions | Allow/Deny/Ask + Browser Allowlist | Config manual no IDE, documentada em getting-started |
| `utils/task-manager/**` + MCP | `docs/` (referência) + `mcp_config.example.json` | Lógica vira doc; servidores em `~/.gemini/config/` |
| `sessions/<feature>/` | Artifacts (task lists, implementation plans, walkthroughs) | Sessão persistente → artifacts do IDE; opcional `docs/sessions/` versionado |

### Convenções de naming
- **Workflows**: `<categoria>-<comando>.md`, kebab-case, invocados `/<categoria>-<comando>`. Resolve colisões (vários `start`/`finish`/`help`/`warm-up`).
- **Skills**: `<nome>/SKILL.md`, `name` lowercase-hífen = nome do dir.
- **Rules**: kebab-case temático.
- Código em inglês, docs/comentários em pt-BR (regra preservada).

---

## 🔍 Alternatives Considered

### Alternative 1: Faithful 1:1 port (92 workflows + 49 subagents)
- **Pros:** preserva granularidade total; menor curadoria
- **Cons:** workflows flat com 92 entradas poluem o menu `/`; 49 subagents excedem o modelo de personas; campos ricos do frontmatter Onion sem suporte nativo geram lixo
- **Cost/Effort:** Alto

### Alternative 2: Adaptive Redesign **(SELECIONADA)**
- **Pros:** estrutura nativa enxuta; preserva propósito e workflows invariantes; expertise reaproveita knowledge-bases; menu `/` curado
- **Cons:** decisões de consolidação exigem julgamento; perda de granularidade fina dos 49 agentes
- **Cost/Effort:** Médio

### Alternative 3: Platform-agnostic source (gerador Claude+Antigravity)
- **Pros:** evita re-trabalho para o próximo IDE
- **Cons:** contradiz "remover Claude"; sobre-engenharia para um framework single-platform; maior superfície de manutenção
- **Cost/Effort:** Alto

---

## 📊 Consequences

### ✅ Positive
- Onion roda nativamente no Antigravity sem camada de tradução
- Skills migram quase 1:1 (mesmo schema SKILL.md)
- Hooks/MCP/permissions têm equivalente direto → baixo risco técnico
- Menu de workflows curado melhora descoberta vs 92 comandos

### ⚠️ Negative
- **Consolidação dos 49 agentes** perde granularidade → *mitigar*: expertise preservada como skills lastreadas em `docs/knowledge-base/`
- **Workflows flat** perdem a navegação por categoria → *mitigar*: prefixo de categoria + `onion` workflow como índice
- **Sem SessionStart** para detecção de provider → *mitigar*: rule + hook PreInvocation
- **Frontmatter rico** (model/parameters/tags/related) sem suporte → *mitigar*: virar prosa no corpo do workflow

### 🔄 Neutral
- `.claude/` e `.agents/` coexistem durante Fases 2-4; remoção só na Fase 5 após gate
- Histórico Claude Code permanece no git (hard removal não reescreve história)

---

## 📈 Success Metrics
- [ ] Rules carregam no Antigravity (3 dimensões + idioma + provider routing)
- [ ] ≥1 workflow por dimensão executa via `/` (produto, engenharia, compliance)
- [ ] MCP Task Manager (provider de teste) cria/atualiza task
- [ ] Workflow faseado roda fim-a-fim com artifact/implementation-plan
- [ ] `grep` final: zero refs a Claude Code como plataforma atual

---

## 🔗 Related Decisions
- Relaciona-se a [onion-review-2026-05.md](onion-review-2026-05.md) (identidade single-platform)
- Impacta `docs/meta-specs/architecture.md` §5 (plataforma alvo), `CLAUDE.md`, `README.md`
- Baseado em [KB Antigravity](../knowledge-base/platforms/antigravity.md)

---

## 📝 Implementation Notes (entrada para Fase 2)

### Action Items
- [ ] Criar `.agents/` com `AGENTS.md`, `rules/`, `workflows/`, `skills/`, `hooks.json`, `mcp_config.example.json`
- [ ] Destilar `CLAUDE.md` → `AGENTS.md` + 4 rules
- [ ] Portar 4 skills (2 → rules, 2 → skills)
- [ ] Portar workflows por dimensão (priorizar faseados invariantes)
- [ ] Consolidar 49 agentes em personas core + skills de expertise
- [ ] Re-expressar Task Manager Abstraction (doc + mcp_config + hook)

### Risk Mitigation
- **Risk:** hooks/subagents 2.0 com docs esparsas → **Mitigation:** validar formato exato na Fase 4 com instância real
- **Risk:** colisão de nomes de workflow → **Mitigation:** prefixo de categoria obrigatório

---

**📅 Created:** 2026-06-03
**👤 Author:** Onion (assistido por IA)
**📋 Status:** Accepted — contrato da Fase 2
