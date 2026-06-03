---
description: Orquestra criação, validação, otimização e migração de skills do Antigravity (.agents/skills/) via @agent-skills-specialist.
---

# 🧩 Agent Skills — Criar / Validar / Otimizar / Migrar

Orquestrador para o `@agent-skills-specialist`. Detecta o modo de operação, coleta contexto e delega. No Antigravity, skills ficam em `.agents/skills/<name>/SKILL.md`.

Entrada do usuário: o texto após o comando — pode trazer `action`
(`create` default | `validate` | `optimize` | `migrate` | `eval`), `skill_name`
(kebab-case ou path) e/ou o `context` (domínio/problema/expertise que o skill encapsula).

## 🎯 Objetivo

Guiar a criação e manutenção de **skills** — pastas com `SKILL.md` que estendem capacidades do agente. No Sistema Onion sobre Antigravity, o padrão é `.agents/skills/`.

---

## ⚡ Fluxo de Execução

### Passo 1: Detectar Modo de Operação

| Argumento | Modo | O que faz |
|-----------|------|-----------|
| `create` (default) | **Criar** | Novo skill em `.agents/skills/<name>/` |
| `validate` | **Validar** | Checar skill existente (estrutura, frontmatter, lifecycle cost) |
| `optimize` | **Otimizar** | Melhorar description trigger via eval set |
| `migrate` | **Migrar** | Converter workflow `.agents/workflows/X.md` em skill |
| `eval` | **Avaliar** | Configurar test cases de qualidade |

Defaults inteligentes:
- Sem modo + path para SKILL.md existente → perguntar: validate/optimize/eval?
- Sem modo + path para `.agents/workflows/*.md` → sugerir `migrate`
- Sem modo + descrição livre → `create`

### Passo 2: Coletar Inputs

**Modo `create`** — perguntar se não fornecidos:
1. **Nome do skill** (kebab-case, ex: `pdf-processing`)
2. **Expertise/domínio**: que erro sistemático ou conhecimento não-óbvio o skill encapsula?
3. **Artefatos de referência**: runbooks, schemas, histórico de PRs, API specs?
4. **Scripts necessários?** Python/Bash/Deno?
5. **Features aplicáveis?**
   - `paths` glob (só ativar em certos arquivos)?
   - skill destrutivo (deploy) que não deve ser auto-invocado?
   - dados live (ex.: diff git) embutidos?

**Modo `validate`** — coletar:
1. Path do skill: procurar `SKILL.md` sob `.agents/skills/`
2. Checar: frontmatter, contagem de linhas (< 500), description quality, lifecycle cost

**Modo `optimize`** — coletar:
1. Path do skill
2. Exemplos de queries que deveriam (e não deveriam) ativar

**Modo `migrate`** — coletar:
1. Path do workflow legacy em `.agents/workflows/`
2. Razões para migrar (precisa scripts? supporting files? ativação automática?)

**Modo `eval`** — coletar:
1. Path do skill
2. 2-3 tasks reais como ponto de partida

### Passo 3: Verificações Rápidas

- Conferir se `.agents/skills/` existe.
- Verificar duplicação de skill pelo nome/description.
- Listar workflows legacy candidatos a migrar em `.agents/workflows/`.

### Passo 4: Delegar para @agent-skills-specialist

Invocar com contexto estruturado:

```
@agent-skills-specialist

Modo: {{action}}
Skill: {{skill_name}}
Localização alvo: .agents/skills/{{skill_name}}/SKILL.md
KB de referência: docs/knowledge-base/tools/agent-skills.md

Contexto do domínio: {{context coletado}}
Artefatos disponíveis: {{runbooks, schemas, exemplos}}
Features desejadas: {{paths, dados live, etc.}}
```

O agente vai:
- **create**: gerar `SKILL.md` + estrutura de pasta, com frontmatter adequado
- **validate**: checar coerência (description quality, lifecycle cost, paths, etc.)
- **optimize**: propor iterações da description com técnica train/validation
- **migrate**: converter workflow em skill mantendo o `/comando` funcional
- **eval**: estruturar `evals/evals.json` com test cases e assertions

### Passo 5: Confirmação Pós-Operação

- Listar a estrutura criada em `.agents/skills/<name>`.
- Conferir tamanho do `SKILL.md` (< 500 linhas).
- Preview do frontmatter.

---

## 🗺️ Estrutura Esperada

```
.agents/skills/<name>/
├── SKILL.md              # < 500 linhas
├── scripts/              # *.py / *.sh
├── references/           # Carregados sob demanda
└── examples/             # Exemplos de output
```

### SKILL.md mínimo (Antigravity)

Skills do Antigravity usam `name` + `description` no frontmatter:

```markdown
---
name: <skill-name>
description: >
  [O que faz — verbos de ação].
  Use quando [contexto explícito], mesmo que o usuário
  não mencione [keyword] diretamente.
---

## Instruções
[Passo a passo concreto]

## Gotchas
- [Erros sistemáticos do domínio]
```

---

## 📤 Output Esperado

```
✅ SKILL {{action}}
━━━━━━━━━━━━━━

📁 Arquivo: .agents/skills/{{skill_name}}/SKILL.md
📏 Linhas: N (< 500)

📋 RESULTADO:
   ∟ name + description: [preview]
   ∟ Features: [paths, dados live, etc.]
   ∟ Scripts: sim/não · References: sim/não

🔍 QUALIDADE:
   ∟ Frontmatter (name + description): ✅ válido
   ∟ Description: ✅ imperativa + contexto
   ∟ Lifecycle cost: ✅ < 500 linhas
   ∟ Gotchas: ✅/⚠️ presente/ausente

🚀 PRÓXIMOS PASSOS:
   ∟ Testar: /{{skill_name}} ou pergunta que bate com description
   ∟ Otimizar trigger: /meta-create-skill optimize {{skill_name}}
   ∟ Avaliar qualidade: /meta-create-skill eval {{skill_name}}

━━━━━━━━━━━━━━
```

---

## 💡 Exemplos de Uso

```bash
# Criar skill (default)
/meta-create-skill "processar faturas no formato TISS"

# Validar skill existente
/meta-create-skill validate .agents/skills/pdf-processing/

# Otimizar description (trigger accuracy)
/meta-create-skill optimize pdf-processing

# Migrar workflow legacy para skill
/meta-create-skill migrate .agents/workflows/engineer-deploy.md

# Configurar evals de qualidade
/meta-create-skill eval csv-analyzer
```

---

## 🔗 Referências

- **Persona principal**: `@agent-skills-specialist`
- **KB**: `docs/knowledge-base/tools/agent-skills.md`
- **Docs oficiais**: https://agentskills.io/specification (spec aberta)
- **Exemplos reais**: https://github.com/anthropics/skills
- **Relacionado**: `/meta-create-command` (workflows em `.agents/workflows/`)
- **Relacionado**: `/meta-create-agent` (personas em `.agents/AGENTS.md`)

## ⚠️ Notas

- **Padrão Onion sobre Antigravity**: skills em `.agents/skills/`
- Skill ativado permanece em contexto pelo resto da sessão — cada linha extra é custo recorrente
- Skill gerado sem contexto de domínio real tem valor mínimo — sempre extrair de runbooks/schemas/PRs
- Se a persona já lida bem com o task sem o skill → o skill não agrega valor
