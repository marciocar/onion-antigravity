---
description: Preparação geral do projeto — estabelece o contexto completo do Sistema Onion (README, documentação, meta especificações e recursos).
---

# 🔥 Warm-up Geral do Projeto

Preparação completa do contexto geral do Sistema Onion (sobre Antigravity) para
sessões de trabalho.

## 🎯 Objetivo

Estabelecer contexto completo incluindo: visão geral do Sistema Onion, estrutura de
documentação, meta especificações (constituição do sistema) e mapeamento de recursos
disponíveis.

## 📋 Checklist de Preparação

### 1. README Principal

- ✅ Revisar `README.md` na raiz e entender a estrutura do Sistema Onion.
- ✅ Identificar workflows e personas principais.
- ✅ Mapear integrações disponíveis (Jira, ClickUp, Asana, Linear).

### 2. Estrutura de Documentação

- ✅ Listar arquivos em `docs/` e revisar `docs/INDEX.md` (índice central).
- ✅ Mapear: `docs/onion/` (Sistema Onion), `docs/knowledge-base/` (Knowledge Bases),
  `docs/meta-specs/` (Meta Especificações), `docs/analysis/` e `docs/plans/`.

### 3. Meta Especificações

- ✅ Revisar `docs/meta-specs/index.md`.
- ✅ Memorizar a hierarquia: Meta Specs (L0) → Domain Specs (L1) → Feature Specs (L2)
  → Task Specs (L3).
- ✅ Conhecer os padrões planejados: `architecture.md`, `code-standards.md`,
  `agents.md`, `commands.md`, `integrations.md`.
- ✅ Conhecer o processo de validação via @metaspec-gate-keeper.

### 4. Recursos Principais

- ✅ Workflow `/onion` — ponto de entrada inteligente.
- ✅ Persona @onion — orquestrador master (`.agents/AGENTS.md`).
- ✅ Task Manager Abstraction (Jira, ClickUp, Asana, Linear) — regra
  `task-manager-routing`.
- ✅ Framework EXTRACT para reuniões.

## 🔍 Contexto a Manter

### Documentação Essencial

- `README.md` — visão geral · `docs/INDEX.md` — hub de navegação
- `docs/onion/commands-guide.md` — workflows · `docs/onion/agents-reference.md` —
  personas · `docs/meta-specs/index.md` — meta especificações

### Estrutura do Sistema

- Workflows em `.agents/workflows/` organizados por categoria (product, git, engineer,
  docs, meta, validate, test, development, quick).
- Personas/subagentes em `.agents/AGENTS.md`; skills em `.agents/skills/`; regras em
  `.agents/rules/`; sessões versionadas em `docs/sessions/`.
- Knowledge Bases estruturadas para consumo por IA.

## 💡 Quando Usar

- Primeira vez no projeto · retorno após ausência · mudança de contexto · necessidade
  de visão geral completa.

## 🔗 Próximos Passos

Após este warm-up geral, use warm-ups específicos:
- `/product-warm-up` — trabalho de produto
- `/engineer-warm-up` — trabalho de engenharia

## ⚠️ Notas

- Este warm-up fornece contexto geral, não técnico profundo. Para trabalho
  específico, use os warm-ups de categoria. Mantenha a lista de arquivos `docs/` no
  contexto para referência rápida.
