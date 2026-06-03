---
title: Meta-spec — Padrões para Personas/Subagents do Sistema Onion
date: 2026-06-03
version: 2.0.0
level: L0
status: active
gate-keeper: "@metaspec-gate-keeper"
changelog: "v2.0.0 — migração de plataforma Claude Code → Google Antigravity; agentes (.claude/agents/) → personas/subagents em .agents/AGENTS.md; remove schema YAML antigo (name/model/category/tools)"
---

# Meta-spec — Padrões para Personas/Subagents do Sistema Onion

## Propósito

Define os padrões imutáveis (L0) que **todas as personas/subagents** declaradas em `.agents/AGENTS.md` devem seguir. Esta spec é a constituição normativa do `@metaspec-gate-keeper` para validar conformidade em PRs que criam ou modificam personas.

No Google Antigravity, a "equipe de IA" do Onion não vive como arquivos avulsos por categoria (como eram os agentes do Claude Code em `.claude/agents/`); ela é declarada de forma consolidada em `.agents/AGENTS.md`. Personas com expertise profunda e reutilizável são lastreadas em **skills** (`.agents/skills/`) e em **knowledge bases** (`docs/knowledge-base/`).

Aplica-se ao **Sistema Onion**, não ao projeto-alvo onde o Onion é instalado.

Referências relacionadas:

- [commands.md](./commands.md) — padrões para workflows
- [architecture.md](./architecture.md) — estrutura de diretórios e dependências
- [code-standards.md](./code-standards.md) — padrões de código e idioma
- [integrations.md](./integrations.md) — padrões para integrações com sistemas externos

---

## 1. Onde as personas vivem

As personas do Onion são declaradas em **`.agents/AGENTS.md`** — um único documento que define a "equipe de IA" (orquestrador + leads das três dimensões peer + especialistas de apoio). Não há mais um arquivo por agente, nem diretório por categoria.

| Artefato | Localização | Natureza |
|---|---|---|
| Personas core | `.agents/AGENTS.md` | Declaração consolidada da equipe de IA |
| Expertise reutilizável | `.agents/skills/<nome>/SKILL.md` | Conhecimento contextual on-demand (ver [commands.md](./commands.md) §Skills) |
| Conhecimento de referência | `docs/knowledge-base/` | KBs consumidas por personas e skills |
| Regras always-on | `.agents/rules/` | System instructions sempre ativas |

> Migração: os 49 agentes do Claude Code (`.claude/agents/<categoria>/<nome>.md`) foram consolidados em ~8-10 personas core em `AGENTS.md` + skills de expertise. Ver [ADR-001](../analysis/onion-antigravity-migration-adr-2026-06.md).

---

## 2. Schema de declaração de persona

Cada persona declarada em `.agents/AGENTS.md` deve conter, no mínimo, os quatro elementos abaixo:

| Elemento | Conteúdo |
|---|---|
| **Role @handle** | Nome/papel da persona com handle de invocação (`@product-agent`, `@metaspec-gate-keeper`) |
| **Goal** | Objetivo único e claro — o que a persona existe para entregar |
| **Traits** | Características de comportamento/abordagem (ex: rigoroso, evidência-primeiro, conciso) |
| **Constraint** | Limites de escopo, o que a persona NÃO faz, quando delegar para outra |

### Exemplo de declaração (bloco em `AGENTS.md`)

```markdown
### @product-agent — Lead de Produto
- **Goal**: gestão estratégica de produto e coordenação de iniciativas, agnóstico de Task Manager.
- **Traits**: orientado a outcome, faz perguntas de esclarecimento antes de especificar, prioriza por valor.
- **Constraint**: não opera a API do provider diretamente — delega operação técnica para o especialista do provider ativo (`@clickup-specialist`, etc.).
```

### Regras do schema

- **Handle**: kebab-case com prefixo `@` na invocação (`@product-agent`). O handle é referenciado em workflows e em outras personas como `@nome`.
- **Goal único**: uma persona com múltiplos goals desconexos deve ser dividida ou virar skill de expertise.
- **Constraint obrigatório**: toda persona deve declarar onde para seu escopo e para quem delega — evita sobreposição entre personas.
- **Sem frontmatter por-agente**: o schema antigo do Claude Code (`name`, `model`, `category`, `tags`, `version`, `tools`, `color`) **não se aplica** — personas são prosa estruturada em `AGENTS.md`, não arquivos com YAML.

---

## 3. Subagents do Antigravity

Quando uma persona precisa rodar como **subagent** (contexto isolado, delegação de tarefa pesada), aplicam-se os mesmos quatro elementos (Role/Goal/Traits/Constraint). O subagent:

- Herda o roteamento de Task Manager das `rules/` always-on
- Não declara MCPs em frontmatter — o acesso a tools/MCP é resolvido pela configuração do workspace e pelo `.agents/mcp_config.example.json` → `~/.gemini/config/mcp_config.json` (ver [integrations.md](./integrations.md))
- Recebe contexto explícito da persona/workflow que o invocou

---

## 4. Convenção de naming

- **Handle** (referência de invocação): kebab-case com `@` (`@product-agent`, não `@Product-Agent`)
- **Identificador interno** em `AGENTS.md`: mesmo slug kebab-case sem `@` no cabeçalho da seção
- Sufixos comuns aceitos: `-specialist`, `-agent`, `-developer`, `-engineer`, `-reviewer`, `-creator`, `-checker`, `-master`, `-gate-keeper`
- O handle deve ser **único** entre todas as personas declaradas em `AGENTS.md`

---

## 5. Limites de tamanho

Personas não são arquivos avulsos, então o limite antigo de agente (1.200 / 1.500 linhas) **não se aplica**. As regras agora são:

| Artefato | Limite | Tratamento |
|---|---|---|
| Bloco de persona em `AGENTS.md` | conciso (Role/Goal/Traits/Constraint) | Expertise profunda não vai na declaração — vira skill |
| Skill (`.agents/skills/<nome>/SKILL.md`) | ≤ 500 linhas | Acima disso, extrair para `docs/knowledge-base/` |

Quando uma persona acumula conhecimento extenso (procedimentos, checklists, templates), esse conteúdo deve migrar para:

- **Skills** em `.agents/skills/` quando o conhecimento é "cérebro" reutilizável on-demand (limite ≤ 500 linhas)
- **Knowledge bases** em `docs/knowledge-base/` quando é referência documental

---

## 6. Padrões de delegação

### Quando criar uma persona/especialista nova

Justificativa válida exige **pelo menos um** dos critérios:

- Conhecimento técnico específico não coberto pelas personas existentes (linguagem, framework, padrão)
- Framework regulatório específico (ISO, SOC2, PMBOK)
- Integração com sistema externo com formatação/protocolo próprio (Jira ADF, ClickUp Unicode)
- Workflow especializado que justifica contexto próprio (review pré-PR de branch, extração de reuniões)

### Quando usar persona agnóstica em vez de criar especialista

- Decomposição genérica de tarefas → `@task-specialist`
- Análise de produto sem framework específico → `@product-agent`
- Pesquisa multi-fonte → `@research-agent`

### Regra para o Goal/Constraint

O **Goal** deve indicar o resultado que a persona entrega; o **Constraint** deve indicar **quando** delegar para outra persona. Personas referenciam outras pelo handle `@nome`.

---

## 7. Integração com MCPs

Personas/subagents que dependem de MCP (Model Context Protocol) **não declaram MCPs em frontmatter** (esse mecanismo do Claude Code foi removido). Em vez disso:

- O acesso a servidores MCP é resolvido pela configuração do Antigravity em `~/.gemini/config/mcp_config.json` (template versionado em `.agents/mcp_config.example.json`)
- A persona deve documentar, em seu bloco de `AGENTS.md` ou na skill associada, **quais MCPs/providers** ela espera (ex: ClickUp MCP para `@clickup-specialist`)
- Validar configuração via workflow `/meta-setup-integration`

Referência canônica: [integrations.md](./integrations.md).

---

## 8. Estrutura recomendada do bloco de persona

Em `.agents/AGENTS.md`, cada persona segue o padrão Role @handle · Goal · Traits · Constraint, podendo expandir com:

```markdown
### @<handle> — <Papel>
- **Goal**: <o que entrega>
- **Traits**: <características de comportamento>
- **Constraint**: <limites de escopo, para quem delega>
- **Skills/KBs**: <skills .agents/skills/ e KBs docs/knowledge-base/ associadas> (opcional)
- **MCPs/Providers**: <providers esperados, quando aplicável> (opcional)
```

Detalhamento de procedimentos, exemplos e checklists deve viver em skills ou KBs — não no bloco da persona.

---

## 9. Exemplos de conformidade

### Exemplo conforme

Persona `@product-agent` em `.agents/AGENTS.md`:

- Bloco com Role @handle, Goal único, Traits e Constraint explícitos
- Handle em kebab-case, único
- Constraint indica delegação para especialista do provider ativo
- Expertise profunda lastreada em skill/KB, não inflando o bloco

**Veredito**: `@metaspec-gate-keeper` deve aprovar.

### Exemplo quase-conforme

Persona hipotética `@react-developer`:

- Role/Goal/Traits/Constraint presentes
- Bloco carrega procedimentos extensos de React inline (deveria ser skill)

**Veredito**: aprovação condicional com nota de "extrair expertise para skill". Não bloqueia merge, mas registra dívida técnica.

### Exemplo não-conforme

Persona hipotética declarada como arquivo `.agents/agents/MyAgent.md` com frontmatter `name`/`model`/`category`:

- Usa schema YAML do Claude Code (removido na migração)
- Não vive em `AGENTS.md`
- Handle em PascalCase
- Sem Constraint declarado

**Veredito**: `@metaspec-gate-keeper` deve rejeitar com as violações listadas.

---

## 10. Versionamento e mudanças

Mudanças nesta spec exigem:

1. PR específico para `docs/meta-specs/agents.md`
2. Atualização do campo `version` no frontmatter
3. Validação por `@metaspec-gate-keeper` de que personas existentes ainda passam (ou plano de migração explícito)
4. Atualização desta spec não pode ser feita em PR que toca em `AGENTS.md` — separação para evitar mudança normativa "no atacado"
