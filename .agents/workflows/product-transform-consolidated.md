---
description: Transformar documentos consolidados (reuniões ou documentos) em contexto estruturado e tasks acionáveis — use entre a consolidação e a criação de tasks.
---

# Transformar Documentos Consolidados

Transforma conhecimento consolidado (de reuniões ou documentos) em contexto estruturado e
tasks acionáveis, preenchendo o gap entre consolidação e criação de tasks.
Entrada do usuário: o `source` (arquivo consolidado ou pasta) e, opcionalmente, `mode`
(`interactive` | `auto` | `tasks-only` | `context-only`) e `output_format`
(`tasks` | `context` | `both`). Tudo após o comando.

> **Convenções de comunicação, nomenclatura e formatação:** seguir a skill `.agents/skills/onion-patterns`.
> **Frameworks de transformação (prompt de extração, template de validação, templates de output, modos, persistência):** ver KB `docs/knowledge-base/concepts/consolidated-to-tasks-patterns.md`.

## Objetivo
1. **Ler** documentos consolidados (output de `/product-consolidate-meetings` ou `/docs-consolidate-documents`).
2. **Interagir** com o usuário para refinar e priorizar.
3. **Transformar** o conhecimento em contexto estruturado.
4. **Gerar** contexto pronto para `/product-collect` ou `/product-task`.

## Fluxo de Execução

### Fase 1 — Detecção e Carregamento
- Se `source` é arquivo → usar o arquivo único.
- Se `source` é pasta → localizar `*consolidation*.md` / `*consolidated*.md`; se múltiplos,
  perguntar qual usar.
- Caso contrário → avisar que `source` deve ser arquivo ou pasta.

Validar estrutura: confirmar seções esperadas (Tarefas, Decisões, Gaps, Insights),
identificar o tipo (reuniões vs. documentos) e extrair metadados (data, participantes, temas).

### Fase 2 — Análise Automática (sempre executada)
Independente do modo, invocar @product-agent com o **Framework de Extração Acionável** do
KB para extrair tarefas, decisões, gaps, insights e dependências em YAML. Salvar em
`docs/sessions/consolidated-transform/analysis-<timestamp>.yaml`.

### Fase 3 — Validação Interativa (modo `interactive`, obrigatória)
Apresentar a análise usando o **Template de Validação Interativa** do KB e coletar
aprovações/ajustes do usuário (IDs aprovados, prioridades, owners, deadlines,
dependências, quebra em subtasks) por bloco, encerrando com a confirmação final.

### Fase 4 — Modos de Processamento
Aplicar o modo solicitado (`interactive` padrão, `auto`, `tasks-only`, `context-only`)
conforme a tabela de modos e heurísticas do KB.

### Fase 5 — Gerar Output Final
Com base nos elementos validados (ou na análise automática no modo `auto`) e no
`output_format`, gerar os artefatos usando os **Templates de Output** do KB, em
`docs/sessions/consolidated-transform/`:
- **Contexto estruturado** (`context`/`both`) → `context-<ts>.md`
- **Lista de tasks + comandos prontos** (`tasks`/`both`) → `tasks-<ts>.yaml`, `commands-<ts>.sh`
- Para cada tarefa aprovada, montar invocação de `/product-collect` ou `/product-task` com
  contexto completo e referência ao documento de origem.

## Outputs Esperados
- **Contexto estruturado:** tarefas priorizadas, decisões acionáveis, gaps por impacto,
  insights, dependências e matriz de priorização.
- **Lista de tasks** (opcional): YAML pronto para criação, com metadados e referência ao original.
- **Comandos prontos** (opcional): chamadas `/product-collect` e `/product-task` prontas.

## Parâmetros
- **`source`**: arquivo consolidado `.md`, pasta com consolidações ou múltiplos arquivos.
- **`mode`**: `interactive` (padrão) · `auto` · `tasks-only` · `context-only`.
- **`output_format`**: `tasks` · `context` · `both` (padrão).

## Relacionamentos
**Antes:** `/product-consolidate-meetings`, `/docs-consolidate-documents`
**Depois:** `/product-collect`, `/product-task`, `/product-estimate`, `/product-refine`

Tasks criadas são sincronizadas automaticamente com o Task Manager ativo (regra
`task-manager-routing`), preservando o contexto e a referência ao documento de origem.

## Referências
- KB: `docs/knowledge-base/concepts/consolidated-to-tasks-patterns.md`
- Skill `.agents/skills/onion-patterns` · Personas em `.agents/AGENTS.md`
