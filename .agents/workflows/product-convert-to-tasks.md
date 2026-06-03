---
description: Converte documentos consolidados (reuniões/documentos) em tasks hierárquicas, delegando a /product-task e /product-collect. Use após consolidar conhecimento para transformá-lo em backlog acionável.
---

# 🔄 Converter para Tasks

Converte documentos consolidados em tasks usando workflows existentes do Sistema Onion.

Entrada do usuário: o texto após o comando — um arquivo consolidado ou uma pasta contendo
consolidações (`source`).

## 🎯 Objetivo

Transformar conhecimento consolidado em tasks acionáveis: analisa o documento consolidado,
extrai elementos acionáveis e cria tasks usando `/product-task` ou `/product-collect`.

## ⚡ Fluxo de Execução

### Passo 1: Identificar arquivos relevantes (análise inteligente)

- **Se `source` é arquivo:** usar diretamente.
- **Se `source` é pasta:** listar todos os markdown (recursivo) e delegar a
  `@product-agent` para identificar os relevantes:

```markdown
@product-agent

Analise os arquivos abaixo e identifique quais são relevantes para converter em tasks:
**Arquivos na pasta {{source}}:** {{lista_de_markdown_com_paths}}

Critérios:
1. Documentos consolidados — seções como "Tarefas", "Decisões", "Gaps", "Insights";
   estrutura de consolidação; metadados (data, origem, participantes).
2. Arquivos relacionados — análises/estudos, evolução/refinamento, especificações,
   notas que complementam, contexto de negócio/técnico relacionado.
3. Evitar duplicatas — descartar versões antigas e rascunhos já incorporados; priorizar
   documentos mais recentes e completos.

Para cada arquivo: ✅/❌ relevante? · tipo (consolidado/análise/especificação/contexto/
outro) · relação com outros · prioridade (alta/média/baixa) · justificativa breve.
Output: lista priorizada, sem duplicatas, com tipo e relação.
```

Processar: ordenar por prioridade, agrupar por tipo/relação e carregar conteúdo dos
arquivos relevantes.

### Passo 2: Analisar arquivos relevantes com @product-agent

```markdown
@product-agent

Analise os arquivos (consolidados e relacionados) e extraia elementos acionáveis
organizados hierarquicamente:
**Arquivos:** {{lista_com_tipos_e_relações}}
**Conteúdo:** {{conteudo_dos_arquivos_relevantes}}

Extraia: tarefas (título, descrição, owner, deadline, prioridade); decisões que requerem
implementação; gaps críticos bloqueantes; insights acionáveis de alto valor; hierarquia
sugerida (Task → Subtasks); contexto adicional para enriquecer as tasks.
Output: lista estruturada de tasks prontas para criação.
```

### Passo 3: Criar tasks usando workflows existentes

- **Tasks estruturadas (com subtasks):** `/product-task "{{descricao_completa}}"`
- **Tasks simples ou ideias:** `/product-collect "{{titulo}}"`

Esses workflows já detectam o Task Manager, criam tasks/subtasks hierarquicamente, estimam
story points e preservam contexto nos comentários.

## 📤 Output Esperado

Lista de tasks criadas com IDs e URLs:
```
✅ CONVERSÃO CONCLUÍDA
📄 Documento: {{arquivo}}
📊 Tasks criadas: {{total}}
{{lista_de_tasks_com_urls}}
```

## 🎯 Casos de Uso

- **Após consolidar reuniões:** `/product-consolidate-meetings "docs/meet/sprint-planning/"`
  → `/product-convert-to-tasks "docs/meet/consolidation-*.md"`
- **Após consolidar documentos:** `/docs-consolidate-documents "docs/business-context/"`
  → `/product-convert-to-tasks "docs/consolidated/"`

## 🔗 Integração

Delega para: `@product-agent` (análise/extração), `/product-task` (tasks estruturadas) e
`/product-collect` (tasks simples). Todos lidam com detecção automática de Task Manager,
criação hierárquica, estimativas de story points e preservação de contexto.

## Referências

- Personas/subagentes: `.agents/AGENTS.md`
- Relacionados: `/product-task`, `/product-collect`, `/product-consolidate-meetings`,
  `/docs-consolidate-documents`
