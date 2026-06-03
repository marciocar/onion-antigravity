---
description: Calcula QA Story Points com a fórmula exata do Framework de Testes, com breakdown por perspectiva, sugestões de técnicas e integração opcional com task managers.
---

# 🧮 Estimativa de QA Story Points

Calcula QA Story Points usando a fórmula exata do Framework de Testes, com análise
contextual inteligente, breakdown por perspectiva e sugestões de técnicas.

## Entrada do usuário

O texto após o comando informa: `task-description` (entre aspas, obrigatório),
`complexity` (`simple|medium|complex|epic`, auto-detect), `risk`
(`low|medium|high|critical`, auto-detect), `type` (`unit|integration|ui|api|e2e|
performance|security|manual`, auto-detect), `--task-id` (ID para atualizar),
`--task-manager` (`jira|clickup|asana`, inferido do `.env`/formato do ID), `--update`
(grava story points no task manager), `--breakdown` (detalha por perspectiva) e
`--suggest-techniques`.

> **Base de conhecimento (carregue antes de calcular):**
> - **Conceito + fórmula + conversão p/ horas:** `docs/knowledge-base/frameworks/framework_testes.md` (seção "QA Story Points").
> - **Tabelas determinísticas, keywords, distribuição por tipo, técnicas:** `docs/knowledge-base/frameworks/qa-story-points.md`.
> - **Operacionalização p/ auditoria:** `docs/knowledge-base/frameworks/test-strategy-scoring.md`.

## ⚡ Fluxo de Execução

### Passo 1: Carregar referências (OBRIGATÓRIO)

Ler `framework_testes.md` e `qa-story-points.md` antes de qualquer cálculo. Se
`framework_testes.md` não encontrado: ❌ ERRO pedindo para verificar o arquivo.

### Passo 2: Análise contextual da descrição

Detectar keywords de complexidade, risco e tipo (Mapa de Detecção, §2 de
`qa-story-points.md`). Inferir parâmetros não fornecidos; aplicar defaults da §2
quando nenhuma keyword for detectada.

### Passo 3: Cálculo de QA Story Points

Aplicar `QA Points = Complexidade Base + Ajuste de Risco + Ajuste de Tipo` usando os
valores determinísticos da §1 de `qa-story-points.md` (sem desvios). Somar ajustes
contextuais (ex.: `third-party integration` +1 complexity; `legacy system` +1
complexity +1 risk). Converter o total para horas pela escala do framework.

**Exemplo:** medium (4) + context (+1) + high risk (+3) + integration (+2) = **10
QA Story Points** ≈ 14-18h.

### Passo 4: Breakdown multi-perspectiva (SE `--breakdown`)

Distribuir o total pelos percentuais White/Grey/Black-box do tipo de teste (§3).
Reportar pontos, horas e percentual por perspectiva.

### Passo 5: Sugestões de técnicas (SE `--suggest-techniques`)

Listar técnicas do tipo de teste a partir da §4 de `qa-story-points.md`. Não
inventar técnicas fora do framework.

### Passo 6: Integração com Task Manager (SE `--task-id`)

1. **Detectar provedor:** `--task-manager` explícito; senão inferir do formato
   (`CU-` → clickup, `PROJ-` → jira) ou de `TASK_MANAGER_PROVIDER`. Nada detectado →
   só output local. Ver regra `task-manager-routing`.
2. **Buscar a task** pelo adapter apropriado; ler story points atuais.
3. **Atualizar (SE `--update`):** gravar o custom field "QA Story Points" via o
   provider ativo e comentar a análise. Se o custom field não existir: comentar a
   estimativa e sugerir criá-lo.

**Template de comentário (Unicode):**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧮 QA STORY POINTS ESTIMATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📅 Timestamp: [data/hora]
📋 TASK ANALYSIS: "<task-description>" · Keywords · Context Factors
🧮 FORMULA: Base + Context + Risk + Type = <total> QA Story Points
⏱️ ESTIMATED EFFORT: <hours_range> hours
🎭 MULTI-PERSPECTIVE: White / Grey / Black (pts · h · %)
💡 SUGGESTED TECHNIQUES: <lista>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ <total> QA Story Points estimated.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 📤 Output Esperado

**Resumido (sem flags):** `🧮 QA STORY POINTS: <total> pontos · ⏱️ <hours_range>h ·
📊 Type/Complexity/Risk`.

**Completo (`--breakdown` / `--suggest-techniques`):** seguir a estrutura do template
acima (Task Analysis → Formula → Effort → Multi-Perspective → Techniques).

## 📋 Exemplos de Uso

```bash
/validate-qa-points-estimate "API integration tests for payment gateway"
/validate-qa-points-estimate "API tests with third-party service" medium high integration --breakdown --suggest-techniques
/validate-qa-points-estimate "Login form validation" simple low ui --task-id CU-456 --update
/validate-qa-points-estimate "E2E testing for checkout flow with payment integration"
```

## ⚠️ Regras e Validações

- **Descrição obrigatória:** vazia → ❌ ERRO pedindo detalhes.
- **Valores válidos e anti-patterns:** ver §5 de `qa-story-points.md` (alerta de
  épico >13 pts; inconsistências como `unit+critical`, `manual+epic`).
- **Task-id inválido:** ⚠️ AVISO, continuar sem atualização.
- **Fórmula exata:** sempre usar os valores das KBs; técnicas sempre do framework.
- **`--update`:** apenas quando confiante; requer integração configurada via
  `/meta-setup-integration`.

## 🔗 Integração com Outros Workflows

- `/validate-test-strategy-create "<task-description>" --qa-points=<pts>`
- `/product-task "<task-description>" --qa-points=<pts>`
- `/product-estimate` — comparar estimativa dev vs QA.

Personas: @test-engineer, @test-planner (`.agents/AGENTS.md`).
