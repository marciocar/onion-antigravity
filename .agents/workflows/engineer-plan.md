---
description: Cria um plano de implementação faseado e retomável (plan.md) para uma feature. Use após definir contexto e arquitetura da sessão.
---

# Engineer Plan

Comando para iniciar o planejamento detalhado de uma funcionalidade.

Entrada do usuário: o texto após o comando (tipicamente o `<feature-slug>`).

## Análise

Leia os arquivos `context.md` e `architecture.md` em `docs/sessions/<feature-slug>/`
se ainda não tiver feito.

Sua tarefa agora é criar um plano de implementação detalhado (`plan.md`) para esta
funcionalidade. O objetivo é uma abordagem de implementação **faseada** que permita
construir a feature incrementalmente, testando cada fase conforme avança — e que torne
possível **retomar o trabalho** caso a sessão seja interrompida.

O `plan.md` deve dividir a implementação em fases, cada uma com um pedaço de trabalho
realizável por um humano em ~2 horas.

Template do `plan.md`:

```
# [NOME DA FUNCIONALIDADE]

Se você está trabalhando nesta funcionalidade, certifique-se de atualizar este
arquivo plan.md conforme progride.

## FASE 1 [Completada ✅]

Detalhes desta parte da funcionalidade

### Uma tarefa que foi feita [Completada ✅]

Detalhes sobre a tarefa

### Comentários:
- Algo que aconteceu e nos forçou a mudar de direção
- Algo que aprendemos durante o desenvolvimento
- Algo que discutimos e concordamos

## FASE 2 [Em Progresso ⏰]

### Uma tarefa que precisa ser feita [Em Progresso ⏰]

Detalhes sobre a tarefa

### Uma tarefa que precisa ser feita [Não Iniciada ⏳]

Detalhes sobre a tarefa

## FASE 3 [Não Iniciada ⏳]

### Uma tarefa que precisa ser feita [Não Iniciada ⏳]

Detalhes sobre a tarefa
```

## Dicas
- Use a busca de código do Antigravity para encontrar arquivos relevantes com base nas respostas de descoberta.
- Leia o código relevante em batch para entender detalhes específicos de implementação.
- Use WebSearch e/ou documentação de bibliotecas (MCP) para melhores práticas, se necessário.

## Decisões arquiteturais
Se a pesquisa levantar uma nova decisão arquitetural ou contradição com decisões
anteriores, inicie uma discussão com o humano, concorde sobre as mudanças e atualize
o `architecture.md` da funcionalidade se necessário.

O documento também deve anotar quais tarefas precisam ser feitas **sequencialmente** ou
em **paralelo**.

Uma vez que o `plan.md` esteja finalizado, informe ao humano que você está pronto para
prosseguir para o próximo passo (`/engineer-work`).
