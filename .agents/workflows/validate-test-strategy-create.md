---
description: Cria estratégias completas de teste multi-perspectiva (White/Grey/Black-box) com cálculo automático de QA Story Points, baseadas no Framework de Testes.
---

# 🧪 Criação de Estratégia de Teste

Cria estratégias completas de teste baseadas no Framework de Testes
(`docs/knowledge-base/frameworks/framework_testes.md`), gerando automaticamente
estratégias multi-perspectiva com cálculo de QA Story Points.

## Entrada do usuário

O texto após o comando informa: `feature-name` (obrigatório), `risk-level`
(`baixo|médio|alto|crítico`, default `médio`), `complexity`
(`simples|médio|complexo|épico`, default `médio`), `--task-manager`
(`clickup|asana|linear`, usa `TASK_MANAGER_PROVIDER` se omitido), `--project-id` e
`--dry-run` (executa sem criar tasks reais).

## ⚡ Fluxo de Execução

### Passo 1: Carregar Framework de Testes (OBRIGATÓRIO)

Ler `framework_testes.md` antes de processar e extrair: QA Story Points, diferenças
White/Black/Grey-box, técnicas por tipo, métricas de qualidade e template universal
de caso de teste. Se não encontrado: ❌ ERRO indicando o caminho.

### Passo 2: Validar e Normalizar Parâmetros

`feature-name` obrigatório. Normalizar `risk-level` e `complexity` para minúsculas;
se inválidos, usar default `médio` e avisar.

### Passo 3: Calcular QA Story Points

Fórmula `QA Points = Complexidade Base + Risco + Tipo de Teste`:

- **Complexidade base:** simples 1.5 · médio 4 · complexo 6.5 · épico 10.5
- **Ajuste de risco:** baixo +0.5 · médio +1.5 · alto +2.5 · crítico +4
- **Tipo de teste (por complexidade):** simples +1 · médio +2.5 · complexo/épico +5

Somar e arredondar para inteiro. Ex.: complexo (6.5) + alto (2.5) + extensivo (5) =
**14 pontos**.

### Passo 4: Distribuir QA Points por Perspectiva

Features simples/médias: White 30% · Grey 30% · Black 40%. Features
complexas/épicas: White 25% · Grey 35% · Black 40%. Arredondar cada perspectiva e
garantir que a soma = total. Ex.: 14 → White 4 · Grey 5 · Black 5.

### Passo 5: Gerar Estratégia Multi-Perspectiva

- **White-box:** Code Coverage (>80%), Mutation Testing (>70%), TDD/BDD, Jest/PyTest/JUnit
- **Grey-box:** API Contracts (100%), Integration (>95% pass rate), Fuzzing, Postman
- **Black-box:** Equivalence Partitioning, Boundary Analysis, User Journeys (100%), Cypress/Selenium

### Passo 6: Detectar e Configurar Task Manager

Resolver o provedor pela regra `task-manager-routing`: `--task-manager` explícito,
senão `TASK_MANAGER_PROVIDER` do `.env`; se ausente, modo offline (dry-run implícito).
Se `--dry-run`, não criar tasks reais — gerar apenas estrutura e relatório. Resolver
`project-id`: `--project-id`, senão `CLICKUP_DEFAULT_LIST_ID`/`ASANA_DEFAULT_PROJECT_ID`,
senão perguntar.

### Passo 7: Criar Estrutura de Tasks (SE não dry-run)

```
📋 Epic: [Feature] - Test Strategy ([X] QA points)
├── 🔬 White-box ([X]) — Unit Setup · Coverage · Code Review Criteria
├── 🔗 Grey-box ([Y]) — API Contract · Integration Setup · Cross-team Validation
└── 📱 Black-box ([Z]) — User Journey · Acceptance Criteria · Exploratory
```

Criar epic + subtasks por perspectiva no provider ativo (regra
`task-manager-routing`). Em modo none/dry-run: salvar a estrutura local em
`docs/sessions/test-strategies/<feature-name>.md`.

### Passo 8: Gerar Relatório Detalhado

Markdown com resumo (QA Points, effort, risk, complexity), estratégias por
perspectiva (técnicas, métricas, ferramentas), task breakdown (IDs/links se criadas),
success metrics e referências ao framework. Salvar em
`reports/test-strategies/test-strategy-<feature-name>-<YYYYMMDD>.md` (mesmo em dry-run).

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ ESTRATÉGIA DE TESTE CRIADA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 Feature: <feature-name>
🧮 QA Story Points: Base [X] + Risk [Y] + Coverage [Z] = [TOTAL]
🎭 Multi-Perspective:
  🔬 White-box [X] — Coverage >80% · Mutation >70% · Jest/PyTest/JUnit
  🔗 Grey-box  [Y] — Integration >95% · API Contract 100% · Postman
  📱 Black-box [Z] — User Story 100% · Bug Detection >85% · Cypress/Selenium
🔗 Task Manager: <provedor> · Epic [ID/URL] · Subtasks [N]
📄 Report: reports/test-strategies/test-strategy-<feature>-<date>.md
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 Próximos: revisar → ajustar pontos → /engineer-start <feature-slug>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 📋 Exemplos de Uso

```bash
/validate-test-strategy-create "checkout-flow" alto complexo --task-manager clickup --project-id 123456
/validate-test-strategy-create "user-profile" médio simples --dry-run
/validate-test-strategy-create "payment-integration" critico complexo --task-manager asana
```

## ⚠️ Validações e Regras

- **Framework deve existir** (senão ❌ ERRO); **feature-name não vazio**; se
  `total > 20`, ⚠️ ALERTA sugerindo quebrar a feature.
- Sempre citar seções do framework; a distribuição deve somar o total; tasks criadas
  incluem story points como custom field; relatório salvo mesmo em dry-run.
- Funciona sem task manager (modo offline).

## 🔗 Referências

- `docs/knowledge-base/frameworks/framework_testes.md` · Regra `task-manager-routing`
- Relacionados: `/product-task` · `/product-estimate` · `/engineer-start`
- Personas: @test-engineer, @test-planner (`.agents/AGENTS.md`)
