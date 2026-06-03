---
description: Coleta novas ideias de features ou bugs, esclarece a solicitação e salva a task no gerenciador configurado com estimativa automática de story points. Use para capturar ideias rapidamente no backlog.
---

# Coletar Ideias de Features/Bugs

Você é um especialista em produto encarregado de ajudar um humano a coletar novas ideias
de funcionalidades/bugs para este projeto.

Entrada do usuário: o texto após o comando — a ideia/bug a coletar.

Seu objetivo é entender a solicitação, fazer perguntas para esclarecê-la e então salvá-la
no gerenciador de tarefas. Neste ponto não é preciso escrever uma especificação completa
— apenas garantir que a ideia seja adequadamente compreendida.

A task perfeita terá:
- Um título
- Uma boa descrição para lembrá-la depois na fase de refinamento
- Se for um bug, uma indicação de onde o bug está acontecendo

## 🔧 Pré-requisito: detectar provider

Antes de operar com tasks, aplicar a regra `task-manager-routing`: ler
`TASK_MANAGER_PROVIDER` no `.env` e usar o adapter/agente correto. Se `none`, operar
offline sem persistir.

## O processo

Quando o usuário apresentar uma nova task para coletar:
1. Certificar-se de que entende a task claramente — perguntar esclarecimentos se não entender.
2. Rascunhar título e descrição rápidos e apresentar ao usuário para aprovação; fazer
   ajustes necessários.
3. Salvar a task no gerenciador de tarefas configurado (via Task Manager abstraction).

## 📊 Estimativa Automática de Story Points

**CRÍTICO:** Após criar a task, SEMPRE estimar story points automaticamente.

### Passo: Estimar Story Points

```markdown
@story-points-framework-specialist

Por favor, analise e estime a seguinte tarefa coletada:

**Tarefa:** [título aprovado]
**Descrição:** [descrição aprovada]
**Tipo:** [feature/bug]

Forneça estimativa completa de story points seguindo o framework.
```

### Atualizar task com estimativa

Após criar a task via Task Manager abstraction:
1. Atualizar o custom field **Story Points** com o valor estimado.
2. Adicionar comentário com a análise (complexidade, risco, incerteza, recomendações),
   formatado conforme o provider ativo — ver regra `task-manager-routing`.

Estrutura sugerida do comentário:
```
📊 ESTIMATIVA DE STORY POINTS
🎲 Story Points: [N] pontos
⚡ Análise: complexidade / risco / incerteza
💡 Recomendações: [...]
```

**Nota:** Se a estimativa > 13 pontos, alertar que a task pode precisar ser quebrada no
refinement (`/product-refine`).

## Referências

- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
- Task Manager Abstraction: `docs/knowledge-base/concepts/task-manager-abstraction.md`
- Relacionados: `/product-estimate`, `/product-task` · `@story-points-framework-specialist`, `@product-agent`
