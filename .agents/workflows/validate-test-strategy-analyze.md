---
description: Analisa estratégias de teste existentes contra o Framework de Testes, identifica gaps de conformidade/cobertura e sugere melhorias priorizadas.
---

# 🔍 Análise de Estratégia de Teste

Audita estratégias de teste existentes e sugere melhorias baseadas no Framework de
Testes, identificando gaps de conformidade e oportunidades de otimização. Este
workflow é um **orquestrador** — não duplica o framework nem as matrizes de scoring,
carrega ambos por referência:

- **Framework canônico:** `docs/knowledge-base/frameworks/framework_testes.md`
- **Matrizes de scoring, thresholds, gaps e impacto:** `docs/knowledge-base/frameworks/test-strategy-scoring.md`

## Entrada do usuário

O texto após o comando informa: `feature-id` (ID da feature/epic, obrigatório),
`--task-manager` (`jira|clickup|asana`, inferido do `.env`/formato do ID),
`--deep-scan` (analisa código e cobertura), `--auto-fix` (correções automáticas
seguras) e `--export-report` (relatório detalhado em arquivo).

## ⚡ Fluxo de Execução

### Passo 1: Carregar Framework e Matrizes de Scoring (OBRIGATÓRIO)

Ler `framework_testes.md` (QA Story Points, perspectivas White/Grey/Black-box,
técnicas, métricas, template universal de caso de teste, padrões de colaboração) e
`test-strategy-scoring.md` (matriz de discrepância, score multi-perspectiva, matriz
de gaps, thresholds, fórmulas de impacto, limites de auto-fix). Se algum não for
encontrado: ❌ ERRO indicando os caminhos.

### Passo 2: Detectar e Configurar Task Manager

Detectar o provedor pela regra `task-manager-routing` (prioridade: `--task-manager`
explícito → `TASK_MANAGER_PROVIDER` do `.env` → formato do `feature-id`:
`CU-`→clickup, `PROJ-`/numérico→jira, `ASANA-`→asana). Nada detectado → ❌ ERRO
sugerindo configurar o `.env` ou passar `--task-manager`. Validar que `feature-id`
não está vazio.

### Passo 3: Validar e Normalizar Parâmetros

`feature-id` obrigatório; flags `--deep-scan`/`--auto-fix`/`--export-report` default
`false`.

### Passo 4: Coletar Dados do Task Manager

Para o provedor ativo, buscar: epic/feature principal (nome, descrição, status,
labels, QA Story Points), todas as subtasks (White/Grey/Black-box), acceptance
criteria e histórico (comentários, mudanças de status, time tracking quando
disponível).

### Passo 5: Coletar Dados do Código (se `--deep-scan`)

Buscar arquivos de teste (`**/*test*`, `**/__tests__/**`, `**/spec/**`), configs
(jest/pytest/nyc), coverage e workflows de CI. Analisar tipos de teste presentes e
contagem, métricas de cobertura, quality gates e histórico de falhas/flaky/tempo.

### Passo 6: Analisar Conformidade com o Framework

Aplicar as matrizes de `test-strategy-scoring.md`: QA Story Points (atribuído vs.
calculado, discrepância >20% — §1); cobertura multi-perspectiva (§2); conformidade
com templates (AC, classificação, métricas — §3); padrões de colaboração (Three
Amigos, Pair Testing, Handoff — §4).

### Passo 7: Detectar Gaps e Problemas

Classificar gaps pela matriz §5 (cobertura/perspectivas faltantes, estimativas
incorretas, métricas fora do threshold, técnicas inadequadas, falta de automação,
debt técnico). Severidade CRITICAL/HIGH/MEDIUM/LOW conforme a KB.

### Passo 8: Calcular Impacto dos Gaps

Usar as fórmulas §6 (risco de bugs em produção, eficiência perdida em horas, quality
score base 100 com penalidades, impacto na velocity).

### Passo 9: Gerar Sugestões de Melhoria

Categorizar por severidade/tipo e priorizar por `Prioridade = (Impacto × Severidade)
/ Esforço` (§7). Cada sugestão: problema, ação específica, impacto esperado e esforço
estimado.

### Passo 10: Aplicar Auto-Fixes (se `--auto-fix`)

Apenas correções seguras e não-destrutivas (§8): recalcular QA points quando
discrepância >20%, adicionar labels/tags faltantes, criar subtasks para perspectivas
faltantes, completar AC com o template universal. Sempre: backup, log, comentário na
task e rollback possível. Nunca deletar tasks, modificar código de testes ou alterar
histórico.

### Passo 11: Gerar Relatório Detalhado

Estruturar: resumo executivo (score, gaps por severidade, risco, horas), dados
coletados, conformidade (4 dimensões do Passo 6), gap analysis, impact assessment,
recomendações priorizadas, tabela Current vs Target (§9), auto-fixes e action items.
Salvar em `reports/test-strategy-analysis/analysis-<feature-id>-<YYYYMMDD-HHMM>.md`.
Se `--export-report`: gerar também JSON e resumo executivo de 1 página.

### Passo 12: Apresentar Resultado Final

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 ANÁLISE DE ESTRATÉGIA DE TESTE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 Feature: <feature-id> - [nome]   📊 Task Manager: <provedor>
✅ Compliance: QA Points [X]% | Multi-Perspective [Y]% | Métricas [Z] gaps
🔍 Gaps: 🔴 CRITICAL | 🟡 HIGH | 🟢 MEDIUM | 🔵 LOW
📊 Quality Score: [X]/100 | Debt: [X]h
💡 [M] melhorias priorizadas por impacto/esforço
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 📋 Exemplos de Uso

```bash
/validate-test-strategy-analyze PROJ-123
/validate-test-strategy-analyze CU-456 --task-manager clickup
/validate-test-strategy-analyze TICKET-101 --deep-scan --auto-fix
/validate-test-strategy-analyze FEATURE-789 --deep-scan --export-report --auto-fix
```

## ⚠️ Validações e Regras

- Framework e KB de scoring devem existir (Passo 1) — senão abortar.
- `feature-id` não vazio; task manager acessível (senão erro sugerindo
  `/meta-setup-integration`); epic/feature deve ser encontrado.
- **Sempre baseado em referência** (citar seção da KB); **auto-fix apenas seguro**;
  **priorização por impacto/esforço**; **relatórios acionáveis**; **preservar histórico**.

## 🔗 Referências

- `docs/knowledge-base/frameworks/framework_testes.md` · `test-strategy-scoring.md`
- Relacionado: `/validate-test-strategy-create` · Regra `task-manager-routing`
- Personas: @test-engineer, @test-planner (`.agents/AGENTS.md`)
