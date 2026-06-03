---
description: Criar tasks no Task Manager ativo com decomposição hierárquica (task → subtask → action item) e story points automáticos — suporta ClickUp, Asana e Linear.
---

# Criação de Task com Decomposição

Criar tasks estruturadas no Task Manager ativo, com decomposição hierárquica
**Task → Subtask → Action Item** e story points automáticos.
Entrada do usuário: a `description` da task e, opcionalmente, o `project_name`
(nome do projeto/lista). Tudo após o comando.

## Objetivo
Estabelecer base sólida para desenvolvimento, registrando a intenção no Task Manager
antes de qualquer execução, com plano confirmado pelo usuário.

## PASSO 0 (OBRIGATÓRIO): Detectar Provedor
**CRÍTICO — antes de qualquer ação. NUNCA assumir o provedor.** Aplique a regra
`task-manager-routing`:
1. Ler `TASK_MANAGER_PROVIDER` no `.env` (valores: `clickup` | `asana` | `linear` | `none`).
2. Validar a variável obrigatória do provedor ativo (`CLICKUP_API_TOKEN` /
   `ASANA_ACCESS_TOKEN` / `LINEAR_API_KEY`).
3. **Fallback gracioso:** se faltar variável, avisar em pt-BR qual falta, sugerir
   `/meta-setup-integration` e seguir em **modo offline** (tasks não sincronizadas).
   Não inventar valores nem assumir outro provedor.

## Fluxo de Execução

### Passo 1: Resolver Projeto/Lista
- Se `project_name` fornecido → buscar no provedor pelo nome; se não encontrado, perguntar.
- Se não fornecido → usar o default do `.env` (`CLICKUP_DEFAULT_LIST_ID` /
  `ASANA_DEFAULT_PROJECT_ID` / `LINEAR_TEAM_ID`); se não configurado, listar opções e perguntar.

### Passo 2: Análise de Contexto e Compreensão
Antes de decompor:
1. Revisar a documentação do projeto (`README.md`, `docs/`) para identificar padrões,
   tecnologias e estrutura.
2. Ler com cuidado a `description`.
3. Resolver ambiguidades e entender como a tarefa se encaixa na estrutura existente.
4. **Classificar complexidade** (define a granularidade):

   | Tipo | Duração | Subtasks | Action Items/Subtask |
   |------|---------|----------|----------------------|
   | Simples | 1-3d | 2-3 | 2-3 |
   | Média | 4-7d | 3-4 | 3-4 |
   | Complexa | 1-2sem | 4-6 | 3-5 |
   | Épico | >2sem | Quebrar em múltiplas tasks | — |

### Passo 3: Decompor Hierarquicamente
Consultar @task-specialist. Cada action item deve caber em **1-4h**:

```
📋 TASK (objetivo de alto nível)
├── 🔧 Subtask 1 (componente funcional)
│   ├── ✅ Action Item 1.1 (1-4h)
│   └── ✅ Action Item 1.2 (1-4h)
└── 🔧 Subtask 2 (componente funcional)
    └── ✅ Action Item 2.1 (1-4h)
```

### Passo 4: Estimar Story Points (Automático)
Via @story-points-framework-specialist:
1. **Task principal** — passar descrição, subtasks e complexidade; obter story points,
   análise de complexidade/risco/incerteza e recomendações.
2. **Cada subtask** — obter story points; armazenar o total (soma das subtasks).
3. **Validar consistência:** se `soma(subtasks) > task_principal`, ajustar a principal
   para a soma; se `task_principal > 13 pts`, alertar **ÉPICO** e propor quebra.

> Framework: `docs/knowledge-base/frameworks/framework_story_points.md`.

### Passo 5: Apresentar Plano e Obter Confirmação (OBRIGATÓRIO ANTES DE CRIAR)
**CRÍTICO: NUNCA criar task sem apresentar o plano e obter confirmação explícita.**
Apresentar: task principal (nome, tipo, complexidade, estimativa, story points),
descrição funcional, arquitetura técnica, bibliotecas/dependências sugeridas, componentes
afetados, decomposição com pontos por subtask, critérios de aceitação e pontos de atenção
para teste. Encerrar com:

> ❓ **Este plano está correto? Posso criar a task no Task Manager?** [Y/n]

Aguardar confirmação explícita. Se o usuário pedir ajustes, revisar e reapresentar.

### Passo 6: Criar no Gerenciador (APÓS CONFIRMAÇÃO)
**ORDEM CRÍTICA:** (1) criar a task no Task Manager; (2) executar o trabalho se houver
ação imediata (Passo 7); (3) atualizar com o resultado (Passo 8). **NUNCA** executar
trabalho antes de criar a task.

6.1. **Preparar dados normalizados** seguindo a interface `ITaskManager`
(priority `urgent|high|normal|low`), mesmo usando MCP diretamente.

6.2. **Criar task principal, subtasks e comentário** via as ferramentas MCP do provedor
ativo. Os mapeamentos exatos de campos, nomes de ferramentas, conversão de markdown e
construção de URL estão na regra `task-manager-routing` —  **não duplicar aqui**.
Sequência: (1) criar task principal → extrair `id`/`gid` e `url`; (2) criar cada subtask
com `parent` = id da principal; (3) adicionar comentário inicial:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 TASK CRIADA VIA /product-task
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 COMPLEXIDADE: [...]
🎲 STORY POINTS: Principal · Subtasks · Total
⚡ FATORES: [...]  💡 RECOMENDAÇÕES: [...]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
> Convenções de formatação de comentários ClickUp (Unicode, timestamp, status): regra `task-manager-routing`.

**Modo offline (`none`):** gerar `id` local (`local-{timestamp}`), criar documento em
`docs/sessions/tasks/{id}.md` (subtasks em `.../{parent-id}/subtasks/`), anexar o
comentário e avisar que não será sincronizado.

6.3. **Normalizar saída** em `TaskOutput`: `id`, `provider`, `name`, `url`,
`status: 'todo'`, `createdAt` (ISO), `projectId`, `storyPoints`, `subtasks[]`.

### Passo 7: Executar Trabalho (Se Aplicável)
APENAS se a descrição indica ação imediata (ex.: "Remover arquivos X"): após criar a
task, executar o trabalho e documentar. Se for só planejamento/futuro, pular (fica "To Do").

### Passo 8: Atualizar Task com Resultado
Se houve execução: adicionar comentário com o que foi feito, arquivos
modificados/criados/deletados, resultado e próximos passos. Atualizar status:
completo → "Done"; parcial → "In Progress"; só planejamento → "To Do".

### Passo 9: Apresentar Resultado
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ TASK CRIADA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Task · 🔗 URL · 📊 Provedor
🎲 STORY POINTS: Principal · Subtasks · Total
📊 ANÁLISE: Complexidade · Risco · Incerteza
🔧 ESTRUTURA: [árvore de subtasks/action items]
💡 RECOMENDAÇÕES: [...]
🚀 Próximo: /engineer-start [feature-slug]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Referências
- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
- Task Manager Abstraction: `docs/knowledge-base/concepts/task-manager-abstraction.md`
- Decomposição: @task-specialist · Estimativas: @story-points-framework-specialist,
  `/product-estimate`, `docs/knowledge-base/frameworks/framework_story_points.md`

## Notas
- **OBRIGATÓRIO:** detectar provedor antes de tudo; apresentar plano e pedir confirmação
  antes de criar; criar task PRIMEIRO, depois executar (se aplicável).
- Action items: máximo 4h cada. Épico (>13 pts): alertar e propor quebra.
- Se `soma(subtasks) > task principal`, ajustar a principal. Sem provedor: modo offline.
