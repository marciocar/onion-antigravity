---
description: Organiza sessão de pair testing multi-perspectiva (White/Grey/Black-box) para validação colaborativa de features, com agenda, template e checklist.
---

# 🤝 Pair Testing - Sessão de Teste em Par

Organiza sessões de pair testing multi-perspectiva para validação colaborativa de
features. Este workflow é um **orquestrador**: os protocolos detalhados (agendas
por perspectiva, template de documentação, checklist de execução, formatos de
comentário) vivem na KB [`docs/knowledge-base/frameworks/collaborative-testing-patterns.md`](../../docs/knowledge-base/frameworks/collaborative-testing-patterns.md). O framework canônico de testes (perspectivas, QA Story Points, técnicas) vive em [`docs/knowledge-base/frameworks/framework_testes.md`](../../docs/knowledge-base/frameworks/framework_testes.md).

## Entrada do usuário

O texto após o comando informa os parâmetros: `feature` (nome da feature,
obrigatório), `perspective` (`white-box|grey-box|black-box`, obrigatório),
`--schedule` (criar evento de calendário), `--task-manager` (`clickup|jira|linear|asana`,
default = `TASK_MANAGER_PROVIDER` do `.env`), `--feature-id` (ID no task manager
para buscar contexto) e `--participants` (ex.: `"dev1,qa1"`; se omitido, inferir da
perspectiva).

## 🎯 Objetivo

Estruturar sessões de pair testing que resultem em validação colaborativa sob
múltiplas perspectivas, descoberta de edge cases, transferência de conhecimento,
documentação em tempo real de findings/bugs, estimativa colaborativa de QA Story
Points e test strategy refinada.

## ⚡ Fluxo de Execução

### Passo 1: Carregar Referências (OBRIGATÓRIO)

Ler antes de organizar a sessão:
1. `collaborative-testing-patterns.md` — protocolo, agendas, templates, checklists.
2. `framework_testes.md` — framework canônico (perspectivas e QA Story Points).

Se alguma KB não for encontrada: ❌ ERRO indicando o caminho ausente.

### Passo 2: Validar e Normalizar Parâmetros

`feature` obrigatório (abortar se vazio). `perspective` obrigatório, em
minúsculas, validado em `[white-box|grey-box|black-box]` (abortar se inválida).
`task-manager` default `clickup`. `feature-id`/`participants` opcionais.

### Passo 3: Determinar Participantes e Combinação

Se `participants` informado, usar diretamente (validar formato). Senão, inferir da
perspectiva pela tabela §1 da KB.

### Passo 4: Buscar Contexto da Feature (Opcional)

Se `feature-id` informado, buscar descrição, critérios de aceitação, test strategy,
bugs conhecidos e comentários pelo provider ativo (ver regra `task-manager-routing`).
Senão, buscar arquivos relacionados no código (testes, docs, specs).

### Passo 5: Gerar Agenda Estruturada

Selecionar a agenda da perspectiva (§2 da KB): `grey-box` → Dev+Dev (§2.1);
`white-box`/`black-box` com Dev+QA → Dev+QA (§2.2); `black-box` com QA+QA → QA+QA
(§2.3). Preencher cabeçalho (feature, participantes, perspectiva, duração 1-2h,
data) e salvar como `pair-testing-agenda-<feature>.md`.

### Passo 6: Criar Template de Documentação

Instanciar o template §3 da KB preenchido com a feature/feature-id. Salvar como
`pair-testing-session-<feature>.md`.

### Passo 7: Criar Checklist de Execução

Instanciar o checklist §4 da KB. Salvar como `pair-testing-checklist-<feature>.md`.

### Passo 8: Integração com Task Manager (Opcional)

Se `feature-id` informado, seguir §5 da KB: comentar resumo da sessão, criar
subtasks (preparação, execução, follow-up de bugs), aplicar tags `pair-testing` e a
perspectiva. Respeitar o provider ativo e sua formatação (regra `task-manager-routing`).

### Passo 9: Integração com Calendar (Opcional)

Se `--schedule`, criar evento conforme §6 da KB (título, duração, participantes,
reminder 15min antes). Senão, gerar `.ics` ou instruir criação manual.

## 📤 Output Esperado

Arquivos gerados: `pair-testing-agenda-<feature>.md`, `pair-testing-session-<feature>.md`,
`pair-testing-checklist-<feature>.md` e (se `--schedule`) evento de calendário.
Atualizações no task manager (se `feature-id`): comentário com resumo, subtasks,
tags `pair-testing`/perspectiva, custom fields.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ PAIR TESTING SESSION ORGANIZED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Feature: <feature>   🎯 Perspectiva: <perspective>
👥 Participantes: [P1] + [P2]
📁 Arquivos: agenda / session / checklist
🔗 Task Manager: <provider> (Feature ID: <feature-id>)
📅 Calendar: [criado se --schedule]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## ⚠️ Notas

- **Duração:** 1-2h/sessão. **Rotação Driver/Navigator:** a cada 20-30min.
- **Combinações:** grey-box → Dev+Dev · white/black-box com Dev+QA → Dev+QA ·
  black-box com QA+QA → QA+QA. Documentação sempre em tempo real.

## 💡 Exemplos de Uso

```bash
/validate-collab-pair-testing "checkout" grey-box --schedule --feature-id CU-123
/validate-collab-pair-testing "login" black-box --participants "qa1,qa2"
/validate-collab-pair-testing "user-profile" white-box --feature-id TASK-456 --task-manager jira
```

## 🔗 Referências

- KB de testing colaborativo: `docs/knowledge-base/frameworks/collaborative-testing-patterns.md`
- Framework de testes: `docs/knowledge-base/frameworks/framework_testes.md`
- Relacionados: `/validate-collab-three-amigos` · `/validate-test-strategy-create`
- Personas: @test-engineer, @test-planner (`.agents/AGENTS.md`)
