---
description: Facilita sessão Three Amigos (PO + Developer + QA) para refinement de stories, gerando agenda, ata e checklist de outputs.
---

# 🤝 Three Amigos - Sessão de Colaboração

Facilita sessões Three Amigos (Product Owner + Developer + QA) para refinement de
user stories, estimativa de pontos e definição de estratégia de testes.

## Entrada do usuário

O texto após o comando informa: `story_id` (ID da story no task manager,
obrigatório), `task_manager` (`clickup|jira|linear|asana`, default =
`TASK_MANAGER_PROVIDER` do `.env`) e `--generate-agenda` (gerar agenda
automaticamente antes da sessão).

## 🎯 Objetivo

Estruturar sessões Three Amigos que resultem em: user story refinada com critérios
de aceitação claros, Dev/QA/Cross-testing story points estimados, test strategy
definida, riscos mapeados por todas as perspectivas e Definition of Done acordada.

## 📚 Knowledge Base

Os protocolos, agendas, templates e checklists vivem na KB de testing colaborativo.
Este workflow é um **orquestrador** — carregue a KB e a seção 8 (Three Amigos) ao
montar agenda, ata, checklist ou integração. **Não duplique** o conteúdo; referencie
e instancie os templates com `<story_id>` / `<task_manager>`.

- **KB:** [`docs/knowledge-base/frameworks/collaborative-testing-patterns.md`](../../docs/knowledge-base/frameworks/collaborative-testing-patterns.md)
  - §8.1 — Agenda Three Amigos · §8.2 — Template de Ata · §8.3 — Checklist de
    Outputs · §8.4 — Comentário de conclusão + regras de integração · §6 — Calendar
- **Framework canônico:** [`docs/knowledge-base/frameworks/framework_testes.md`](../../docs/knowledge-base/frameworks/framework_testes.md)

## ⚡ Fluxo de Execução

### Passo 1: Preparação da Sessão

Se `--generate-agenda` informado **ou** a agenda não existe: buscar a story
(Passo 2), gerar agenda pelo template §8.1 (substituindo `<story_id>`) e salvar
como `three-amigos-agenda-<story_id>.md`. Senão, usar agenda existente.

### Passo 2: Buscar Contexto da Story

Buscar detalhes/descrição, critérios de aceitação, subtasks e comentários pelo
provider ativo (ver regra `task-manager-routing`). Se indisponível, solicitar as
informações manualmente.

### Passo 3: Gerar Template de Ata

Instanciar o template §8.2 com `<story_id>`; salvar como `three-amigos-ata-<story_id>.md`.

### Passo 4: Criar Checklist de Outputs

Instanciar o checklist §8.3; salvar como `three-amigos-checklist-<story_id>.md`.

### Passo 5: Integração com Task Manager

Seguir §8.4 (e §5 para o provider ativo): atualizar a descrição com a story
refinada; criar subtasks (Desenvolvimento/Dev points, Testes/QA points,
Cross-testing/Cross points); adicionar comentário de conclusão, tags `three-amigos`
+ `refined` e custom fields (Dev/QA/Cross Points). Sem provider (`none`): documentar
manualmente, sem chamadas de API.

### Passo 6: Integração com Calendar (Opcional)

Conforme §6 da KB: criar evento `Three Amigos: <story_id>` (60-90min, PO/Dev/QA,
agenda + link da story + anexo da ata), enviar convite e reminder 15min antes. Sem
integração: gerar `.ics` ou instruir criação manual.

## 📤 Output Esperado

Arquivos: `three-amigos-agenda-<story_id>.md` (se `--generate-agenda`),
`three-amigos-ata-<story_id>.md`, `three-amigos-checklist-<story_id>.md` e evento de
calendário (se disponível). No task manager: descrição atualizada, comentário de
resumo, subtasks (Dev/QA/Cross), tags e custom fields.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ THREE AMIGOS SESSION PREPARED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Story: <story_id>
📁 Arquivos: agenda · ata · checklist (three-amigos-*-<story_id>.md)
🔗 Task Manager: <provider> — story atualizada ✓ · comentário ✓
📅 Calendar: [Status]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## ⚠️ Notas

- **Duração:** 60-90min/story · **Timing ideal:** Sprint Planning ou Refinement.
- **Participantes obrigatórios:** PO + Developer + QA.
- **Outputs críticos:** estimativas (Dev + QA + Cross) e test strategy. Sempre
  registrar a ata completa. Calendar é opcional.

## 💡 Exemplos de Uso

```bash
/validate-collab-three-amigos STORY-123 clickup --generate-agenda
/validate-collab-three-amigos TASK-456 jira
/validate-collab-three-amigos FEATURE-789 clickup --generate-agenda --calendar
```

## 🔗 Referências

- KB: `docs/knowledge-base/frameworks/collaborative-testing-patterns.md`
- Relacionados: `/validate-test-strategy-create` · `/product-refine` · `/product-task`
- Personas: @test-engineer, @product-agent (`.agents/AGENTS.md`)
