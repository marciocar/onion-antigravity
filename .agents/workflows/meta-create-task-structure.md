---
description: Decompõe uma tarefa complexa em estrutura hierárquica de fases e subtarefas, com saída em markdown, JSON ou para o task manager ativo.
---

# 📋 Criar Estrutura de Tarefas

Decomposição de tarefas complexas em hierarquia gerenciável.

Entrada do usuário: o texto após o comando — a descrição da tarefa complexa
(obrigatória) e, opcionalmente, o formato de saída (`markdown` default | `json` | `clickup`).

## 🎯 Objetivo

Transformar tarefa complexa em estrutura organizada de subtarefas.

## ⚡ Fluxo de Execução

### Passo 1: Analisar Tarefa

| Aspecto | Pergunta |
|---------|----------|
| Objetivo | O que precisa estar pronto? |
| Escopo | Quais áreas são afetadas? |
| Complexidade | Quantas etapas? |
| Dependências | Ordem obrigatória? |
| Riscos | Pontos críticos? |

### Passo 2: Identificar Subtarefas

#### Por Camada Técnica
- 📊 **Data**: schemas, migrations, modelos
- 🔌 **API**: endpoints, validação, business logic
- 🎨 **UI**: componentes, páginas, styling
- 🧪 **Testing**: unit, integration, e2e
- 📚 **Docs**: documentação, comentários

#### Por Tipo de Atividade
- 🔍 Análise e pesquisa
- 🏗️ Implementação
- 🧪 Testes
- 📝 Documentação
- 🔄 Integração

### Passo 3: Estruturar Hierarquia

Consultar `@task-specialist` para estrutura:

```
📋 TASK PRINCIPAL
├── 🔧 Fase 1: [Nome]
│   ├── ✅ Subtask 1.1
│   └── ✅ Subtask 1.2
├── 🔧 Fase 2: [Nome]
│   ├── ✅ Subtask 2.1
│   └── ✅ Subtask 2.2
└── 🔧 Fase 3: [Nome]
    └── ✅ Subtask 3.1
```

### Passo 4: Gerar Output

- `markdown` → documento com resumo (fases, subtasks, estimativa) + hierarquia.
- `json` → objeto com `task`, `phases`, `estimatedDays`.
- `clickup` → delegar para `@clickup-specialist` (ou ao especialista do provider ativo — ver regra `task-manager-routing`).

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ ESTRUTURA CRIADA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Task: [descrição]

📊 Decomposição:
∟ Fases: 4
∟ Subtasks: 12
∟ Estimativa: 5 dias

🔧 Estrutura:
├── Fase 1: Setup (1d)
├── Fase 2: Implementação (2d)
├── Fase 3: Testes (1d)
└── Fase 4: Deploy (1d)

🚀 Próximo: /product-task
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Decomposição: `@task-specialist`
- Criação no task manager ativo: `/product-task` (ver regra `task-manager-routing`)

## ⚠️ Notas

- Máximo 6 fases por task
- Subtasks de 1-4h cada
- Se muito grande: quebrar em múltiplas tasks
