# 📚 Índice Central de Documentação

> **Última atualização**: 2026-06-03 | **Gerado por**: `/docs-build-index` | **Revisado**: auditoria manual (migração Claude Code → Google Antigravity)

Bem-vindo ao índice central de documentação do projeto. Este documento serve como hub de navegação para toda a documentação disponível.

---

## 🎯 Visão Geral

Este projeto é o **Sistema Onion** — um framework de workflows `.agents/` para Google Antigravity, de uso interno, com:

- 🤖 **78 workflows invocáveis** (`.agents/workflows/`, achatados com prefixo de categoria → `/cat-cmd`)
- 🎯 **Personas / subagents** consolidados a partir dos 49 agentes especializados (em `.agents/AGENTS.md`)
- 🧩 **2 skills** em `.agents/skills/` (`onion` — cérebro do sistema; `onion-validation`) + **4 rules** always-on em `.agents/rules/`
- 📚 **Knowledge Bases estruturadas** para consumo por IA
- 🧅 **Skill + Workflow `/onion`** — ponto de entrada inteligente com ativação automática
- 🔗 **Task Manager Abstraction** plugável (Jira, ClickUp, Asana, Linear) — em `docs/reference/task-manager/`
- 🏗️ **Spec as Code Multi-Context** — separação entre business, technical e meta-specs

---

## 📊 Estatísticas da Documentação

### Documentação Principal
- **64 arquivos markdown** em `docs/`
- **26 arquivos** em `docs/onion/` (Sistema Onion)
- **26 arquivos** em `docs/knowledge-base/` (Knowledge Bases)
  - 13 arquivos em `concepts/` (Conceitos fundamentais)
  - 7 arquivos em `frameworks/` (Frameworks e metodologias)
  - 3 arquivos em `tools/` (Ferramentas, incl. Agent Skills)
  - 2 arquivos em `platforms/` (Plataformas)
  - 1 `index.md`
- **1 arquivo** em `docs/meta-specs/` (Meta Especificações)
- Arquivos adicionais em `docs/analysis/`, `docs/plans/`, `docs/business-context/`, `docs/technical-context/`

### Sistema Onion (`.agents/`)
- **78 workflows invocáveis** (`.agents/workflows/`, flat com prefixo de categoria → `/cat-cmd`), originados das 9 categorias:
  - `product-*` (gestão de produto e descoberta)
  - `git-*` (GitFlow e versionamento, com feature/hotfix/release achatados)
  - `engineer-*` (engenharia e desenvolvimento)
  - `docs-*` (geração e validação de documentação)
  - `meta-*` (criação de artefatos do Onion e validação)
  - `validate-*` (validação e testes)
  - `test-*` (unit, integration, e2e)
  - `development-*`, `quick-*`
  - root: `onion.md`, `warm-up.md`
  - **fragmentos compartilhados** (templates/prompts) acompanham as skills em `.agents/skills/`
- **2 skills** em `.agents/skills/` (`onion`, `onion-validation`) + **4 rules** always-on em `.agents/rules/` (`onion-identity`, `language-standards`, `task-manager-routing`, `onion-conventions`)
- **Personas / subagents** em `.agents/AGENTS.md`, consolidados a partir dos 49 agentes especializados que cobrem development, product, compliance, meta, git, testing, review, research e deployment

### Total
- **64 arquivos** de documentação markdown
- **78 workflows invocáveis** (`.agents/workflows/`) achatados com prefixo de categoria
- **Personas / subagents** consolidados de 49 agentes (`.agents/AGENTS.md`)
- **2 skills** + **4 rules** (`.agents/skills/`, `.agents/rules/`)

---

## 📁 Estrutura do Framework

### Operacional — `.agents/` (consumido pelo Google Antigravity)

```
.agents/
├── AGENTS.md                   # Personas / "equipe de IA" (subagents)
├── rules/                      # System instructions always-on (4 rules)
├── workflows/                  # Saved prompts /-invocáveis (78, flat + prefixo)
├── skills/                     # Conhecimento contextual on-demand (onion, onion-validation)
├── hooks.json                  # Hooks de ciclo de vida (Pre/PostToolUse, Pre/PostInvocation)
└── mcp_config.example.json     # Template MCP → ~/.gemini/config/mcp_config.json
```

> Config global do Antigravity (não versionada) vive em `~/.gemini/`
> (`mcp_config.json`, `GEMINI.md`, skills compartilhadas). Estado de workflows
> faseados usa Artifacts do Antigravity + (opcional) `docs/sessions/<feature>/`.

### Documentação — `docs/`

```
docs/
├── INDEX.md                    # Este arquivo (hub central)
│
├── onion/                      # Sistema Onion (26 arquivos)
│   ├── commands-guide.md       # Guia completo de comandos
│   ├── agents-reference.md     # Referência de agentes
│   ├── engineering-flows.md    # Fluxos de engenharia
│   ├── practical-examples.md   # Exemplos práticos
│   ├── getting-started.md      # Configuração inicial
│   ├── testing-validation-system.md  # Sistema de testes e validação
│   ├── tools-reference.md      # Referência de ferramentas
│   ├── claude-code-commands-architecture.md  # Arquitetura de comandos
│   ├── end-to-end-validation-tests.md  # Testes de validação E2E
│   ├── sistema-engenharia-reversa-guia-uso.md  # Engenharia reversa
│   ├── levels-system.md        # Sistema de níveis progressivos
│   ├── INSTALLATION.md         # Guia de instalação
│   ├── RELEASE-NOTES-v4.0-beta.md  # Notas de release v4.0
│   └── [outros arquivos de release e implementação]
│
├── knowledge-base/             # Knowledge Bases (25 arquivos)
│   ├── concepts/               # Conceitos fundamentais (13 arquivos)
│   │   ├── abstraction-patterns-catalog.md
│   │   ├── ai-agent-design-patterns.md
│   │   ├── branding-posicionamento-marca.md
│   │   ├── configuration-management.md
│   │   ├── context-window-optimization.md
│   │   ├── framework_story_points.md
│   │   ├── framework_testes.md
│   │   ├── identificar-precificar-dor-cliente.md
│   │   ├── meeting-transcription-to-knowledge-base.md
│   │   ├── spec-as-code-strategy.md
│   │   ├── spec-driven-development.md  # ✨ NOVO
│   │   ├── specification-driven-ai-abstraction-layer.md
│   │   └── task-manager-abstraction.md
│   ├── frameworks/             # Frameworks e metodologias (7 arquivos)
│   │   ├── framework_story_points.md
│   │   ├── framework_testes.md
│   │   ├── onion-complete-cycle-understanding.md
│   │   ├── onion-ide-integration-strategy.md
│   │   ├── onion-multi-context-orchestrator-vision.md
│   │   ├── onion-system-critical-analysis-2025.md
│   │   └── spec-driven-development-tools-2025.md
│   ├── platforms/              # Plataformas e tecnologias (2 arquivos)
│   │   ├── antigravity.md      # Google Antigravity — agentic coding platform ✨ NOVO
│   │   └── runflow.md
│   ├── providers/              # Provedores de serviços (1 arquivo)
│   │   └── microsoft-graph-teams-api-guia-completo.md
│   └── tools/                  # Ferramentas e recursos (2 arquivos)
│       ├── claude-code-commands-best-practices-2025.md
│       └── whisper.md          # Knowledge base do Whisper
│
├── meta-specs/                 # Meta Especificações (constituição L0)
│   ├── index.md · agents.md · commands.md · architecture.md
│   └── code-standards.md · integrations.md
│
├── analysis/                   # Análises críticas datadas + ADRs
│   ├── onion-antigravity-migration-adr-2026-06.md  # ADR migração de plataforma ✨ NOVO
│   └── [demais análises]
│
├── plans/                      # Planos de execução
│   └── [arquivos de planejamento]
│
├── reference/                  # Task Manager Abstraction + utilitários (consumidos por workflows) ✨ NOVO
│   └── task-manager/           # interface, types, detector, factory, adapters/
│
├── sdaal/                      # Specification-Driven AI Abstraction Layer
│   └── [documentação SDAAL]
│
├── sessions/                   # (opcional) contexto versionado de features
│
└── onion/                      # Documentação operacional (guias, referências, releases)
    └── [guias e referências]
```

---

## 🧅 Sistema Onion

### 📖 Documentação Principal

#### Guias Essenciais
- **[Guia de Comandos](onion/commands-guide.md)** - Documentação completa de todos os comandos disponíveis
- **[Referência de Agentes](onion/agents-reference.md)** - Lista e descrição de todos os agentes especializados
- **[Fluxos de Engenharia](onion/engineering-flows.md)** - Workflows detalhados para desenvolvimento
- **[Sistema de Testes e Validação](onion/testing-validation-system.md)** - Framework completo de testes e validação
- **[Sistema de Níveis](onion/levels-system.md)** - Descoberta progressiva de comandos (v4.0) ✨ NOVO

#### Integrações e Configuração
- **[Configuração Inicial](onion/getting-started.md)** - Setup completo do sistema
- **[Instalação](onion/INSTALLATION.md)** - Guia de instalação do Sistema Onion
- Integração com Task Manager (Jira/ClickUp/Asana/Linear): use `/meta-setup-integration` — adapters em `docs/reference/task-manager/adapters/`

#### Referências Técnicas
- **[Exemplos Práticos](onion/practical-examples.md)** - Casos de uso reais com exemplos
- **[Referência de Ferramentas](onion/tools-reference.md)** - Todas as ferramentas disponíveis
- **[Arquitetura de Comandos](onion/claude-code-commands-architecture.md)** - Estrutura interna dos comandos

#### Documentação Avançada
- **[Testes de Validação E2E](onion/end-to-end-validation-tests.md)** - Testes end-to-end do sistema
- **[Guia de Engenharia Reversa](onion/sistema-engenharia-reversa-guia-uso.md)** - Engenharia reversa de projetos

#### Release Notes e Anúncios
- **[Release Notes v4.0 Beta](onion/RELEASE-NOTES-v4.0-beta.md)** - Novidades da versão 4.0 ✨ NOVO
- **[Anúncio v4.0 Beta.1](onion/ANNOUNCEMENT-v4.0-beta.1-docs.md)** - Anúncio da versão beta

### 🚀 Início Rápido

**Novo no sistema?** Comece aqui:

1. **[Configuração Inicial](onion/getting-started.md)** - Setup do ambiente
2. **[Guia de Comandos](onion/commands-guide.md)** - Aprenda os comandos principais
3. **[Exemplos Práticos](onion/practical-examples.md)** - Veja casos de uso reais
4. **[Sistema de Níveis](onion/levels-system.md)** - Entenda a descoberta progressiva (v4.0) ✨ NOVO

**Workflow de entrada:**
```bash
/onion "Sou novo aqui, me ajude a começar"
```

---

## 📚 Knowledge Bases

Knowledge Bases estruturadas para consumo por IA e referência técnica:

### Conceitos Fundamentais (13 arquivos)
- **Task Manager Abstraction** - Abstração de gerenciadores de tarefas
- **Framework de Story Points** - Sistema de estimativas ágeis
- **Framework de Testes** - Metodologias de teste completas
- **Spec-as-Code Strategy** - Estratégia de especificações como código
- **Spec-Driven Development** - Metodologia emergente de desenvolvimento com IA ✨ NOVO
- **AI Agent Design Patterns** - Padrões de design para agentes IA
- **Abstraction Patterns Catalog** - Catálogo de padrões de abstração
- **Context Window Optimization** - Otimização de contexto para IA
- **Configuration Management** - Gestão de configurações
- **Branding e Posicionamento** - Estratégias de marca
- **Identificar e Precificar Dor do Cliente** - Metodologias de produto
- **Meeting Transcription to Knowledge Base** - Processamento de reuniões
- **Specification-Driven AI Abstraction Layer** - Camada de abstração orientada a especificações

### Frameworks e Metodologias (7 arquivos)
- **Framework de Story Points** - Estimativas ágeis
- **Framework de Testes** - White-box, Grey-box, Black-box
- **Onion Complete Cycle Understanding** - Sistema completo de 5 camadas
- **Onion IDE Integration Strategy** - Estratégia multi-IDE
- **Onion Multi-Context Orchestrator Vision** - Visão arquitetural
- **Onion System Critical Analysis 2025** - Análise crítica do sistema
- **Spec-Driven Development Tools 2025** - Ferramentas e análise

### Plataformas e Tecnologias (2 arquivos)
- **Google Antigravity** - Plataforma de coding agêntico da Google (MCP, Skills, Hooks, Subagents) ✨ NOVO
- **Runflow** - Documentação da plataforma

### Provedores de Serviços (1 arquivo)
- **Microsoft Graph Teams API** - Guia completo de integração

### Ferramentas (2 arquivos)
- **Claude Code Commands Best Practices 2025** - Boas práticas de comandos Claude Code
- **Whisper** - Sistema de transcrição de áudio (OpenAI)

**Localização:** `docs/knowledge-base/`

---

## 🏗️ Meta Especificações

Especificações de nível mais alto que servem como "constituição" do Sistema Onion. **As 5 meta-specs L0 foram criadas em 2026-05-18** como parte do saneamento e ativam a validação via `@metaspec-gate-keeper`:

- **[Índice de Meta Specs](meta-specs/index.md)** - Visão geral das meta especificações
- **[agents.md](meta-specs/agents.md)** ✨ NOVO - Padrões obrigatórios para agentes (YAML, categorias, naming, limites)
- **[commands.md](meta-specs/commands.md)** ✨ NOVO - Padrões para comandos + **workflows faseados como invariante**
- **[architecture.md](meta-specs/architecture.md)** ✨ NOVO - Estrutura de diretórios, framework instalável, dependências
- **[code-standards.md](meta-specs/code-standards.md)** ✨ NOVO - Idioma, formatação, naming, estilo
- **[integrations.md](meta-specs/integrations.md)** ✨ NOVO - Task Manager Abstraction como referência canônica, padrão de adapter, MCPs

**Localização:** `docs/meta-specs/`

---

## 🚀 Aplicação em Projetos-alvo

Guias de aplicação do Onion em projetos novos, legados ou regulados:

- **[Guias de Aplicação — README](applying/README.md)** ✨ NOVO - Visão geral e árvore de decisão
- **[Onion em Projeto Novo (Greenfield)](applying/applying-greenfield.md)** ✨ NOVO - Passo a passo desde `git init`
- **[Onion em Projeto Legado](applying/applying-legacy.md)** ✨ NOVO - Engenharia reversa + migração gradual
- **[Onion em Projeto Regulado](applying/applying-regulated.md)** ✨ NOVO - ISO 27001, ISO 22301, SOC2, PMBOK

---

## 📊 Análises e Planos

### Análises
- **[Revisão Analítica do Sistema Onion — Maio/2026](analysis/onion-review-2026-05.md)** ✨ NOVO - Análise crítica completa sob a lente "framework template instalável"; documenta abandono de `.onion/`, plano v4.0 e `packages/onion-cli/`
- **[Retrospectiva T2.6 — Validação das Meta-specs](analysis/metaspec-validation-2026-05-18.md)** ✨ NOVO - Validação das 5 meta-specs contra artefatos reais
- **[Retrospectiva P1 — Saneamento Estrutural](analysis/p1-saneamento-retrospectiva-2026-05-18.md)** ✨ NOVO - Decisões registradas das tarefas T1.1-T1.5
- **[Baseline de Verificação e Validação — Junho/2026](analysis/onion-vv-baseline-2026-06.md)** ✨ NOVO - Estado "antes" (tamanhos + conformidade de plataforma) do plano de V&V
- **[Retrospectiva T3.2 — Validação de build-*-docs](analysis/t32-pilot-retrospectiva-2026-06.md)** ✨ NOVO - Auto-piloto que validou os comandos de geração de docs; resolve T3.6
- **[Análise de Alternativas Unleash](analysis/unleash-alternatives-analysis.md)** - Análise comparativa

### Planos de Execução
- **[Plano de Saneamento Onion 2026-05](plans/onion-saneamento-plan-2026-05.md)** ✨ NOVO - Roadmap executável das 19 recomendações da análise de maio/2026 (status: aprovado, em execução)
- **[Onion v4 Epic](plans/onion-v4-epic.md)** ⚠️ HISTÓRICO - Plano abandonado em 2026-05-18
- **[Onion v4 Migration Plan](plans/onion-v4-migration-plan.md)** ⚠️ HISTÓRICO - Plano abandonado em 2026-05-18

---

## 🧭 Navegação por Perfil

### 👨‍💻 Para Desenvolvedores

**Comece com:**
1. [Configuração Inicial](onion/getting-started.md)
2. [Guia de Comandos](onion/commands-guide.md) - Seção "Comandos de Engenharia"
3. [Fluxos de Engenharia](onion/engineering-flows.md)
4. [Sistema de Testes e Validação](onion/testing-validation-system.md)
5. [Sistema de Níveis](onion/levels-system.md) - Descoberta progressiva ✨ NOVO

**Comandos essenciais:**
- `/engineer-start` - Iniciar desenvolvimento
- `/engineer-work` - Trabalhar em feature
- `/engineer-pr` - Criar Pull Request
- `/test-unit` - Testes unitários
- `/test-integration` - Testes de integração

**Agentes especializados:**
- `@react-developer` - Desenvolvimento React
- `@nodejs-specialist` - Backend Node.js
- `@nx-monorepo-specialist` - Monorepos NX
- `@c4-architecture-specialist` - Arquitetura C4
- `@whisper-specialist` - Transcrição de áudio com Whisper

### 📋 Para Product Owners

**Comece com:**
1. [Guia de Comandos](onion/commands-guide.md) - Seção "Comandos de Produto"
2. [Exemplos Práticos](onion/practical-examples.md)
3. [Knowledge Base - Story Points](knowledge-base/frameworks/framework_story_points.md)
4. [Knowledge Base - Spec-Driven Development](knowledge-base/concepts/spec-driven-development.md) ✨ NOVO

**Comandos essenciais:**
- `/product-task` - Criar tasks estruturadas
- `/product-spec` - Especificações técnicas
- `/product-estimate` - Estimar story points
- `/product-extract-meeting` - Extrair insights de reuniões
- `/product-consolidate-meetings` - Consolidação de múltiplas reuniões
- `/product-convert-to-tasks` - Converter documentos consolidados em tasks
- `/product-whisper` - Facilitador para uso do Whisper
- `/docs-consolidate-documents` - Consolidar múltiplos documentos
- `/validate-collab-three-amigos` - Sessões colaborativas

**Agentes especializados:**
- `@product-agent` - Orquestração de produto
- `@story-points-framework-specialist` - Estimativas ágeis
- `@storytelling-business-specialist` - Narrativas de negócio
- `@branding-positioning-specialist` - Branding e posicionamento
- `@extract-meeting-specialist` - Extração de reuniões
- `@meeting-consolidator` - Consolidação de reuniões

### 🧪 Para QA/Test Engineers

**Comece com:**
1. [Sistema de Testes e Validação](onion/testing-validation-system.md)
2. [Framework de Testes](knowledge-base/frameworks/framework_testes.md)
3. [Guia de Comandos](onion/commands-guide.md) - Seção "Comandos de Validação"

**Comandos essenciais:**
- `/test-unit` - Testes unitários (White-box)
- `/test-integration` - Testes de integração (Grey-box)
- `/test-e2e` - Testes end-to-end (Black-box)
- `/validate-test-strategy-create` - Criar estratégias de teste
- `/validate-qa-points-estimate` - Estimar QA points
- `/validate-collab-pair-testing` - Teste em par

**Agentes especializados:**
- `@test-agent` - Estratégias completas de teste
- `@test-engineer` - Implementação prática
- `@test-planner` - Planejamento e cobertura

### 🏗️ Para Arquitetos

**Comece com:**
1. [Arquitetura de Comandos](onion/claude-code-commands-architecture.md)
2. [Meta Especificações](meta-specs/index.md)
4. [Release Notes v4.0](onion/RELEASE-NOTES-v4.0-beta.md) - Arquitetura Multi-Context ✨ NOVO

**Recursos:**
- Agentes de arquitetura: `@c4-architecture-specialist`, `@mermaid-specialist`
- Comandos de documentação: `/docs-build-tech-docs`, `/docs-reverse-consolidate`
- Knowledge Bases: [SDAAL](knowledge-base/concepts/specification-driven-ai-abstraction-layer.md), [Spec-Driven Development](knowledge-base/concepts/spec-driven-development.md) ✨ NOVO

### 🔧 Para Administradores do Sistema

**Comece com:**
1. [Configuração Inicial](onion/getting-started.md)
2. [Instalação](onion/INSTALLATION.md)
3. [Referência de Ferramentas](onion/tools-reference.md)

**Comandos essenciais:**
- `/meta-setup-integration` - Configurar Task Manager (Jira/ClickUp/Asana/Linear) e demais integrações
- `/meta-all-tools` - Listar todas as ferramentas
- `/docs-build-index` - Reconstruir índices

### 🛡️ Para Compliance/Security

**Comece com:**
1. [Agentes de Compliance](onion/agents-reference.md#️-agentes-de-compliance)
2. [Comandos de Validação](onion/commands-guide.md#-comandos-de-validação)

**Agentes especializados:**
- `@iso-27001-specialist` - ISO 27001:2022
- `@iso-22301-specialist` - ISO 22301:2019
- `@soc2-specialist` - SOC2 Type II
- `@security-information-master` - Segurança da informação
- `@corporate-compliance-specialist` - Compliance corporativo

---

## 🗺️ Mapa de Navegação Rápida

### Por Tipo de Documento

| Tipo | Localização | Descrição |
|------|-------------|-----------|
| 📖 **Guias** | `docs/onion/` | Guias de uso e referência |
| 📚 **Knowledge Bases** | `docs/knowledge-base/` | Conhecimento estruturado para IA |
| 🏗️ **Meta Specs** | `docs/meta-specs/` | Especificações de alto nível |
| 📊 **Análises** | `docs/analysis/` | Análises e estudos |
| 📋 **Planos** | `docs/plans/` | Planos de execução |
| 🔧 **SDAAL** | `docs/sdaal/` | Specification-Driven AI Abstraction Layer |

### Por Categoria de Workflow

| Categoria | Workflows | Documentação |
|-----------|---------|--------------|
| 🔧 **Engenharia** | `/engineer-*` | [Guia de Comandos](onion/commands-guide.md#-comandos-de-engenharia) |
| 📋 **Produto** | `/product-*` | [Guia de Comandos](onion/commands-guide.md#-comandos-de-produto) |
| 🧪 **Testes** | `/test-*` | [Sistema de Testes](onion/testing-validation-system.md) |
| ✅ **Validação** | `/validate-*` | [Sistema de Testes](onion/testing-validation-system.md) |
| 📚 **Documentação** | `/docs-*` | [Guia de Comandos](onion/commands-guide.md#-comandos-de-documentação) |
| 🌿 **Git** | `/git-*` | [Guia de Comandos](onion/commands-guide.md#-comandos-git) |
| ⚙️ **Meta** | `/meta-*` | [Guia de Comandos](onion/commands-guide.md#-comandos-meta) |
| 🧅 **Onion** | `/onion` | [Sistema Onion](onion/) |
| ⚡ **Quick** | `/quick-*` | [Guia de Comandos](onion/commands-guide.md) |

### Por Categoria de Persona / Subagent

| Categoria | Personas | Documentação |
|-----------|---------|--------------|
| 🛡️ **Compliance** | `compliance` (5) | [Referência de Agentes](onion/agents-reference.md#️-agentes-de-compliance) |
| 🔴 **Meta** | `meta` (4) | [Referência de Agentes](onion/agents-reference.md#-agentes-meta) |
| ⚙️ **Deployment** | `deployment` (1) | [Referência de Agentes](onion/agents-reference.md) |
| 🟣 **Pesquisa** | `research` (1) | [Referência de Agentes](onion/agents-reference.md#-agentes-de-pesquisa) |
| 🟢 **Review** | `review` (1) | [Referência de Agentes](onion/agents-reference.md#-agentes-de-review) |

---

## 🔗 Links Rápidos

### Documentação Essencial
- [README Principal](../../README.md) - Visão geral do Sistema Onion v4.0 Beta
- [Guia de Comandos](onion/commands-guide.md) - Todos os comandos
- [Referência de Agentes](onion/agents-reference.md) - Todos os agentes
- [Sistema de Testes e Validação](onion/testing-validation-system.md) - Framework completo
- [Sistema de Níveis](onion/levels-system.md) - Descoberta progressiva ✨ NOVO
- [Release Notes v4.0](onion/RELEASE-NOTES-v4.0-beta.md) - Novidades da versão ✨ NOVO

### Knowledge Bases
- [Task Manager Abstraction](knowledge-base/concepts/task-manager-abstraction.md)
- [Framework de Story Points](knowledge-base/frameworks/framework_story_points.md)
- [Framework de Testes](knowledge-base/frameworks/framework_testes.md)
- [AI Agent Design Patterns](knowledge-base/concepts/ai-agent-design-patterns.md)
- [Spec-as-Code Strategy](knowledge-base/concepts/spec-as-code-strategy.md)
- [Spec-Driven Development](knowledge-base/concepts/spec-driven-development.md) ✨ NOVO
- [Google Antigravity](knowledge-base/platforms/antigravity.md) - Plataforma agentic coding Google ✨ NOVO
- [Whisper](knowledge-base/tools/whisper.md) - Transcrição de áudio

### Configuração
- [Configuração Inicial](onion/getting-started.md)
- [Instalação](onion/INSTALLATION.md)
- [Adapters de Task Manager](reference/task-manager/adapters/) (Jira, ClickUp, Asana, Linear)

---

## 🆕 Novidades

### ✨ Documentação Adicionada Recentemente

- **[Spec-Driven Development](knowledge-base/concepts/spec-driven-development.md)** (2025-12-02)
  - Knowledge base completa sobre metodologia emergente
  - Análise de ferramentas (Kiro, Spec-Kit, Tessl)
  - Níveis de implementação (Spec-First, Spec-Anchored, Spec-as-Source)
  - Comparação com TDD, BDD, MDD
  - Benefícios e desafios

- **[Sistema de Testes e Validação](onion/testing-validation-system.md)** (2025-12-02)
  - Framework completo de testes e validação
  - 4 camadas integradas (Knowledge Base, Agentes, Comandos de Teste, Comandos de Validação)
  - Guia completo para desenvolvedores, QA e times cross-funcionais

- **[Sistema de Níveis](onion/levels-system.md)** (v4.0)
  - Descoberta progressiva de comandos
  - Organização por níveis (starter, intermediate, advanced)
  - Redução de 90% no tempo de onboarding

- **[Release Notes v4.0 Beta](onion/RELEASE-NOTES-v4.0-beta.md)**
  - Arquitetura Multi-Context
  - Melhorias significativas em onboarding e organização
  - Suporte multi-IDE

- **Comandos de Produto Expandidos**
  - `/product-extract-meeting` - Extração inteligente de insights de reuniões
  - `/product-consolidate-meetings` - Consolidação de múltiplas reuniões
  - `/product-convert-to-tasks` - Converter documentos consolidados em tasks
  - `/product-whisper` - Facilitador para uso do Whisper
  - `/docs-consolidate-documents` - Consolidar múltiplos documentos
  - Agente `@meeting-consolidator` - Consolidação avançada de reuniões
  - Agente `@whisper-specialist` - Especialista em transcrição de áudio
  - Knowledge Base Whisper - Documentação completa do Whisper

---

## 📞 Suporte e Recursos

### 🆘 Resolução de Problemas

1. **Comandos**: Consulte [Guia de Comandos](onion/commands-guide.md)
2. **Exemplos**: Veja casos práticos em [Exemplos Práticos](onion/practical-examples.md)
3. **Configuração**: Siga [Configuração Inicial](onion/getting-started.md)
4. **Instalação**: Consulte [Instalação](onion/INSTALLATION.md) ✨ NOVO
5. **Testes**: Consulte [Sistema de Testes e Validação](onion/testing-validation-system.md)

### 🔧 Workflows de Debug

```bash
/onion "ajuda"                  # Ponto de entrada inteligente
/meta-all-tools                 # Lista todos os workflows
/docs-build-index               # Reconstruir este índice
@onion "sua pergunta"           # Persona orquestradora master
```

---

## 🔄 Manutenção

Este índice é gerado automaticamente pelo workflow `/docs-build-index`.

**Para atualizar:**
```bash
/docs-build-index              # Reconstruir índice principal
/docs-build-index onion        # Reconstruir índice da seção onion
```

**Última atualização:** 2026-06-03
**Mantido por:** Sistema Onion

---

**Sistema Onion** - Multi-Context Development Orchestrator 🧅
