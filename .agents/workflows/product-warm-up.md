---
description: Preparar contexto de produto e negócio — use ao iniciar trabalho com features, specs, estimativas, reuniões ou análise de requisitos, antes de tarefas técnicas.
---

# Warm-up de Produto

Preparação específica para trabalho de produto, negócio e gestão de features.

## Objetivo
Estabelecer contexto focado em: documentação de produto e negócio, especificações de
features e domínios, knowledge bases de produto, frameworks de produto (Story Points etc.)
e workflows/personas de produto.

## Checklist de Preparação

### 1. Contexto Geral (Base)
- Revisar `README.md` para visão geral do Sistema Onion.
- Entender a estrutura de documentação em `docs/`.

### 2. Meta Especificações
- Revisar `docs/meta-specs/index.md`; memorizar a hierarquia de especificações.
- Entender quando consultar meta specs para decisões de produto.

### 3. Documentação de Produto
- Revisar o guia de comandos (seção "Comandos de Produto").
- Mapear os workflows de produto disponíveis:

**Gestão de Tasks:** `/product-task` · `/product-feature` · `/product-collect` ·
`/product-task-check` · `/product-validate-task` · `/product-checklist-sync`
**Especificação e Refinamento:** `/product-spec` · `/product-refine` · `/product-estimate` · `/product-light-arch`
**Processamento de Reuniões:** `/product-extract-meeting` · `/product-consolidate-meetings`
**Análise e Validação:** `/product-check` · `/product-analyze-pain-price`
**Comunicação:** `/product-branding` · `/product-presentation`
**Documentação Relacionada:** `/docs-consolidate-documents`

### 4. Knowledge Bases de Produto
- `docs/knowledge-base/frameworks/framework_story_points.md`
- `docs/knowledge-base/concepts/task-manager-abstraction.md`
- `docs/knowledge-base/concepts/meeting-transcription-to-knowledge-base.md`
- `docs/knowledge-base/concepts/identificar-precificar-dor-cliente.md`
- `docs/knowledge-base/concepts/branding-posicionamento-marca.md`

### 5. Personas de Produto
Conhecer as personas/subagents especializados (em `.agents/AGENTS.md`):
- @product-agent — orquestração estratégica
- @story-points-framework-specialist — estimativas ágeis
- @extract-meeting-specialist — extração de reuniões
- @meeting-consolidator — consolidação de reuniões
- @storytelling-business-specialist — narrativas de negócio
- @branding-positioning-specialist — branding e posicionamento

### 6. Task Manager Integration
- Verificar `TASK_MANAGER_PROVIDER` no `.env` (regra `task-manager-routing`).
- Entender a abstração de Task Manager (ClickUp, Asana, Linear).
- Revisar `docs/knowledge-base/concepts/task-manager-abstraction.md`.

### 7. Especificações de Features
- Mapear a estrutura: Domain Specs (L1 — regras de negócio) e Feature Specs (L2).
- Entender o formato de especificações do projeto.

## Workflows de Produto

**Feature (completo):** `/product-collect` → `/product-task` → `/product-feature` →
`/product-check` → `/product-spec` → `/product-estimate` → `/product-refine` → `/product-light-arch`
**Reuniões:** `/product-extract-meeting` → `/product-consolidate-meetings` → `/docs-consolidate-documents`
**Validação:** `/product-validate-task` → `/product-task-check` → `/product-checklist-sync`
**Análise:** `/product-analyze-pain-price` → `/product-branding` → `/product-presentation`

## Quando Usar Este Warm-up
- Trabalho em especificações de features; criação/refinamento de tasks; estimativas de
  story points; processamento e consolidação de reuniões; análise de requisitos de negócio;
  análise de dor do cliente; branding/posicionamento; criação de apresentações; trabalho
  com Product Owners.

## Integração com Engenharia
Após preparar o contexto de produto: tasks criadas são validadas por `/engineer-start`,
story points são verificados antes de iniciar desenvolvimento e especificações alimentam
as sessões de engenharia.

## Notas
- Foco em contexto de negócio e produto, não técnico.
- Mantenha os frameworks de produto no contexto e use as personas especializadas.
- Sempre sincronize tasks com o Task Manager ativo (regra `task-manager-routing`).
