---
description: Sincroniza e monitora checklists nativos do ClickUp, analisando estrutura híbrida (texto + checklists) e gerando insights de progresso. Use para acompanhar completion e detectar divergências em tasks.
---

# 📋 ClickUp Checklist Sync — Análise e Monitoramento

Assistente especializado em sincronizar e monitorar checklists nativos do ClickUp com o
sistema de desenvolvimento. Analisa estruturas híbridas (texto + checklists nativos) e
fornece insights de progresso.

Entrada do usuário: o texto após o comando — o `<task-id>` e, opcionalmente, um modificador:
`--subtasks` (foco nas subtasks), `--progress` (relatório detalhado) ou `--auto-sync`
(atualiza comentários automaticamente).

> Requer provider ClickUp ativo. Antes de operar, aplicar a regra `task-manager-routing`.

## 🎯 Funcionalidades

- **Leitura de estrutura híbrida** — descrição markdown + metadata da task, checklists
  nativos por subtask, mapeamento texto ↔ checklists interativos, status resolved/unresolved.
- **Sincronização inteligente** — detecção de divergências entre texto e checklists,
  consolidação de status, cálculo preciso de progresso, detecção de itens faltantes.
- **Reporting avançado** — resumo de progresso por subtask, detecção de bloqueios,
  velocity tracking, sugestões de próximas ações.

## 🔧 Processo de Análise

### 1. Leitura completa da estrutura
Buscar a task com subtasks via provider ativo. Para cada subtask: ler checklists nativos,
analisar action items na descrição markdown, detectar divergências e calcular progresso.

### 2. Consolidação de status
- **Resolved** — marca como completados.
- **Unresolved** — identifica pendentes.
- **Missing checklists** — sinaliza onde faltam checklists nativos.
- **Orphaned text** — identifica action items apenas em texto.

### 3. Geração de insights
Completion rate por subtask · bloqueios prováveis · recomendações de estrutura ·
próximos itens prioritários.

## 📋 Output

### Análise básica
```markdown
# 📊 CHECKLIST SYNC ANALYSIS
## 🎯 Task: [NOME] (ID: [ID]) — Status: [X] | Progresso: [XX]%
### Structure Overview
- Subtasks: [N] ([N] com checklists, [N] apenas texto)
- Action items: [N] nativos + [N] apenas texto — Completion: [XX]%
### Subtask Breakdown
**[SUBTASK]** — [XX]% ([N]/[N]) · ✅ [N] completos · ⏳ [N] pendentes · ⚠️ [issues]
### Sync Issues
- Missing checklists / Orphaned text / Status divergence
### Recommendations + Next Actions (imediato / curto prazo)
```

### Progress report
```markdown
# 📊 PROGRESS REPORT - [DATA]
- Task completion: [XX]% · Action items: [XX]% · Velocity: [N]/dia · ETA: [data]
- Checklist health: coverage, sync status, missing items
- Progress trends (24h) + Focus areas (alta/média/baixa prioridade)
```

## 🤝 Integração com Sistema Onion

- `/product-task` — cria estrutura inicial (texto)
- `/engineer-start` — lê e analisa checklists no início
- `/engineer-work` — monitora progresso durante desenvolvimento
- `/product-checklist-sync` — especialista em sincronização (este workflow)

Workflow recomendado: criar task com `/product-task` → criar checklists nativos no ClickUp
(manual) → `/product-checklist-sync <task-id>` → `/engineer-start` → monitorar com
`/product-checklist-sync <task-id> --progress`.

## ⚠️ Limitações

- **Não pode:** criar checklists nativos (limitação da API ClickUp MCP), modificar itens
  existentes, automatizar criação durante `/product-task`.
- **Pode:** ler todos os checklists existentes, analisar estrutura híbrida, reportar
  divergências, calcular métricas precisas, sugerir melhorias, monitorar completion.

## 📚 Casos de Uso

- **Nova task criada** (`/product-checklist-sync <id>`) → detecta ausência de checklists
  nativos e sugere onde criar.
- **Desenvolvimento em progresso** (`--progress`) → progresso real baseado em checklists,
  próximos itens, ETA por velocity.
- **Review de estrutura** (`--auto-sync`) → identifica divergências, sugere correções e
  atualiza comentários na task.

## Referências

- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
- Task Manager Abstraction: `docs/knowledge-base/concepts/task-manager-abstraction.md`
