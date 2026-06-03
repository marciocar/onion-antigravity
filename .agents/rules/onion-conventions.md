# Regra: Estrutura, Naming e Convenções do Onion (Antigravity)

Aplica-se ao criar ou editar qualquer artefato em `.agents/`.

## Estrutura de diretórios
```
.agents/
├── AGENTS.md                 # personas / equipe de IA
├── rules/*.md                # system instructions always-on
├── workflows/<n>.md          # saved prompts /-invocáveis (flat + prefixo de categoria)
├── skills/<n>/SKILL.md       # conhecimento contextual (+ scripts/, references/, assets/)
├── hooks.json                # hooks de ciclo de vida
└── mcp_config.example.json   # template MCP → ~/.gemini/config/mcp_config.json
```

## Naming
- **Workflows**: `<categoria>-<comando>.md`, kebab-case; invocados `/<categoria>-<comando>`.
  - Categorias: `engineer`, `product`, `git`, `docs`, `meta`, `validate`, `test`, `quick`, `development`.
  - Nested vira composto: `git/feature/start` → `git-feature-start` → `/git-feature-start`.
  - Prefixo **obrigatório** — a lista de workflows é flat; sem prefixo há colisão (vários `start`, `finish`, `help`, `warm-up`).
- **Skills**: pasta `<nome>/SKILL.md`, `name` lowercase-hífen (= nome do dir).
- **Rules**: kebab-case temático.
- **Feature slug**: kebab-case obrigatório (`user-authentication`, não `user_authentication`/`UserAuth`). Usado em branch Git e em `docs/sessions/<feature-slug>/`.

## Frontmatter
### Workflow (`.agents/workflows/*.md`)
```yaml
---
description: O que faz + quando usar (frase clara; aparece no menu /)
---
```
> O Antigravity só consome `description` no frontmatter de workflow. Metadados ricos
> (modelo, parâmetros, tags, relacionados) viram prosa no corpo, não frontmatter.

### Skill (`.agents/skills/<nome>/SKILL.md`)
```yaml
---
name: nome-skill
description: Frase de trigger específica (obrigatória — matching semântico do LLM)
---
```
> O SKILL.md do Antigravity consome `name` + `description` — o trigger de ativação
> vai na `description` (matching semântico); restrições de tool via permissions/hooks.

## Limites e métricas
| Métrica | Limite | Razão |
|---------|--------|-------|
| Workflow | < 400 linhas | otimização de tokens |
| Skill | < 500 linhas | lifecycle persistente em contexto |
| Rule | curta e imperativa | system instruction, não prosa |
| `description` de skill | < 1024 chars | trigger budget |

## Fluxos invariantes (não consolidar)
- **Feature**: `/product-task` → `/engineer-start` → `/engineer-work` → `/engineer-pre-pr` → `/engineer-pr`
- **Discovery**: `/product-collect` → `/product-refine` → `/product-spec` → `/product-feature`
- **Hotfix**: `/engineer-hotfix` → `/engineer-work` → `/engineer-pr` → `/git-hotfix-finish`

## Gotchas
- Feature slug com underscore quebra GitFlow — kebab-case obrigatório
- Workflow sem prefixo de categoria colide na lista `/`
- `description` vaga em skill impede matching — ser específico
- Persistência: usar Artifacts do Antigravity; contexto versionado em `docs/sessions/`
