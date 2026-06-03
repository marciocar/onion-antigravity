# Contribuindo com o Sistema Onion 🧅

Obrigado por considerar contribuir com o Onion!

O Onion é um **framework template em `.agents/`** — instalável em qualquer
projeto (novo, legado ou regulado) para orquestrar produto, engenharia e
compliance com o **Google Antigravity**. **Não é produto npm, não é distribuído
publicamente e não tem CLI standalone próprio.** Plataforma única: **Google Antigravity**.

Por isso, contribuir aqui é **escrever Markdown + YAML** (workflows, personas,
skills, rules, knowledge bases e documentação) — não código JavaScript/Node.

---

## 📋 Índice

- [Código de Conduta](#-código-de-conduta)
- [Pré-requisitos](#-pré-requisitos)
- [Estrutura do projeto](#-estrutura-do-projeto)
- [Tipos de contribuição](#-tipos-de-contribuição)
- [Padrões (meta-specs)](#-padrões-meta-specs)
- [Fluxo de Pull Request](#-fluxo-de-pull-request)
- [Idioma e commits](#-idioma-e-commits)
- [Validação](#-validação)

---

## 📜 Código de Conduta

Seja respeitoso, colaborativo, inclusivo e profissional em todas as interações.

---

## 🚀 Pré-requisitos

- **Git**
- **Google Antigravity** (plataforma única do framework)

Não há toolchain de build: o Onion é interpretado em runtime pelo Google
Antigravity a partir de `.agents/` (Markdown + YAML). Não há `package.json`,
Node ou pnpm.

```bash
# 1. Fork e clone
git clone https://github.com/your-username/onion-antigravity.git
cd onion-antigravity

# 2. Abra no Google Antigravity — rules, workflows e skills carregam automaticamente.
#    Para começar: /warm-up e depois /onion
```

---

## 🛠️ Estrutura do projeto

```
onion-antigravity/
├── .agents/                # Sistema Onion operacional (Antigravity)
│   ├── AGENTS.md           # Personas / equipe de IA
│   ├── rules/              # System instructions always-on
│   ├── workflows/          # Workflows /-invocáveis (Markdown + frontmatter)
│   ├── skills/             # Skills (conhecimento contextual)
│   ├── hooks.json          # Hooks de ciclo de vida (versionado)
│   └── mcp_config.example.json  # Template MCP → ~/.gemini/config/
├── docs/                   # Documentação (Spec as Code)
│   ├── meta-specs/         # L0 — "constituição" do framework
│   ├── knowledge-base/     # Knowledge bases estruturadas
│   ├── reference/          # Task Manager Abstraction e utilitários
│   ├── business-context/   # Gerado por /docs-build-business-docs
│   ├── technical-context/  # Gerado por /docs-build-tech-docs
│   └── onion/              # Guias e referências
└── (config global do usuário em ~/.gemini/, não versionado)
```

---

## 🤝 Tipos de contribuição

- **🐛 Bugs** — abra uma issue com: workflow/persona envolvida, o que aconteceu,
  comportamento esperado, passos de reprodução.
- **✨ Novos workflows/personas/skills** — use os criadores do próprio framework:
  `/meta-create-command`, `/meta-create-agent`, `/meta-create-skill`. Eles já
  aplicam os padrões das meta-specs.
- **📚 Documentação e knowledge bases** — correções, clareza, exemplos,
  `/meta-create-knowledge-base`.
- **🔌 Integrações (Task Manager)** — novos adapters seguindo o padrão SDAAL em
  `docs/reference/task-manager/` (ver `docs/meta-specs/integrations.md`).

---

## 📏 Padrões (meta-specs)

As meta-specs L0 em `docs/meta-specs/` são a **fonte canônica** de padrões.
Consulte antes de criar/alterar artefatos:

| Você vai mexer em… | Consulte |
|---|---|
| Persona/subagent | [`agents.md`](docs/meta-specs/agents.md) — frontmatter, categorias, limites de tamanho |
| Workflow | [`commands.md`](docs/meta-specs/commands.md) — frontmatter, workflows faseados, limites |
| Arquitetura/estrutura | [`architecture.md`](docs/meta-specs/architecture.md) — framework instalável, dependências |
| Idioma/estilo/naming | [`code-standards.md`](docs/meta-specs/code-standards.md) |
| Integração externa | [`integrations.md`](docs/meta-specs/integrations.md) — adapters, `.env`, `mcp_config.json` |

Pontos-chave:

- **Tamanho**: workflow ≤400 linhas; skill ≤500 linhas. Excedeu? Extraia conteúdo
  de referência para `docs/knowledge-base/` e mantenha o artefato enxuto.
- **Frontmatter YAML**: workflows usam `description`; skills usam `name` + `description`.
- **Naming**: workflows são `<categoria>-<comando>.md` (prefixo obrigatório).
- **Sem assunções sobre o projeto-alvo**: nada de path absoluto; o framework é
  instalável em qualquer repo.

---

## 🔀 Fluxo de Pull Request

1. **Branch** a partir de `main` (GitFlow): `feature/...` ou `fix/...`
   (ou use `/git-feature-start`).
2. **Mude** seguindo as meta-specs; atualize docs/índices afetados.
3. **Valide** localmente (ver abaixo).
4. **Commit** com Conventional Commits, descrição **em pt-BR** (ver próxima seção).
5. **Abra o PR** com título claro, descrição do quê/porquê, issues relacionadas
   e breaking changes (se houver).

---

## 🌍 Idioma e commits

Convenção do Onion (ver `code-standards.md`):

- **Código, nomes de arquivo, slugs, branches, variáveis**: inglês.
- **Comentários, documentação, mensagens ao usuário**: português brasileiro.
- **Mensagens de commit**: Conventional Commits (tipo em inglês, descrição em pt-BR).

```bash
git commit -m "feat(product): adiciona workflow de priorização de backlog"
git commit -m "fix(task-manager): corrige detecção de provider ausente no .env"
git commit -m "docs(meta-specs): esclarece convenção de naming de workflows"
```

Tipos: `feat`, `fix`, `docs`, `refactor`, `chore`, `test`, `style`, `perf`.

---

## 🧪 Validação

Antes de abrir o PR:

- `/validate-workflow` — completude de workflows.
- Skills `onion-validation` e a rule `onion-conventions` — conformidade de
  artefatos (frontmatter, categorias, limites de tamanho, naming).
- `/meta-metaspec-validate` — validação de conformidade arquitetural contra as
  5 meta-specs.

Checklist:

- [ ] Segue as meta-specs aplicáveis.
- [ ] Frontmatter YAML correto.
- [ ] Dentro dos limites de tamanho (ou refatorado com extração para KB).
- [ ] Documentação/índices atualizados (`/docs-build-index` se necessário).
- [ ] Commits Conventional, descrição em pt-BR.

---

## 🔗 Links úteis

- [Identidade e visão geral (README)](README.md)
- [Índice da documentação](docs/INDEX.md)
- [Meta-specs (constituição)](docs/meta-specs/index.md)
- [Guias de aplicação](docs/applying/) — greenfield, legado, regulado

---

**Obrigado por contribuir com o Onion! 🧅**
