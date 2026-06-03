---
description: Continua o trabalho em uma feature ativa — lê a sessão, identifica a próxima fase do plan.md e atualiza o progresso no Task Manager. Use para retomar/avançar o desenvolvimento.
---

# Engineer Work

Estamos trabalhando em uma funcionalidade especificada em uma pasta de sessão.

Entrada do usuário: o texto após o comando (tipicamente o `<feature-slug>` ou caminho da
sessão em `docs/sessions/<feature-slug>/`).

Para trabalhar nisso, você deve:
- Ler todos os arquivos markdown na pasta da sessão.
- Revisar o `plan.md` e identificar qual Fase está atualmente em progresso.
- Apresentar ao usuário um plano para abordar a próxima fase.

## 🔄 Auto-Update do Task Manager

Este comando **atualiza automaticamente** a task durante o desenvolvimento. Aplique a
regra `task-manager-routing`: carregue o `.env` e leia `TASK_MANAGER_PROVIDER`. Se o
provider não estiver configurado (`none`), opere em **modo offline** — o progresso não
será sincronizado remotamente, apenas no `plan.md` local.

### Updates Automáticos A CADA FASE
- **Comentário de progresso** quando a fase é completada.
- **Atualização de status da subtask** correspondente para "done".
- **Atualização do plan.md** com status e decisões.
- **Progresso % estimado** baseado nas fases concluídas.
- **Timestamp de atividade** para tracking temporal.

### CRITICAL: Phase→Subtask Mapping
**Obrigatório**: quando uma fase é completada, o sistema deve:
1. Identificar a subtask correspondente via mapeamento estabelecido no `context.md`.
2. Atualizar o status da subtask para "done" automaticamente.
3. Documentar a conclusão com timestamp e métricas da fase.

### Estratégia DUAL de Comentários
Ao completar uma fase, o sistema automaticamente:
1. Cria um comentário **DETALHADO** na **SUBTASK**.
2. Cria um comentário **RESUMIDO** na **TASK PRINCIPAL**.

### Identificação da Task
1. **context.md**: lê o task ID do arquivo de contexto da sessão.
2. **Sessão ativa**: detecta automaticamente a sessão em `docs/sessions/`.
3. **Phase-Subtask Mapping**: lê o mapeamento do `context.md` para correlacionar fases→subtasks.

Estrutura do mapeamento (`context.md`):
```markdown
## 📋 Phase-Subtask Mapping
- **Phase 1**: "Nome da Fase" → Subtask ID: [subtask-id-1]
- **Phase 2**: "Nome da Fase" → Subtask ID: [subtask-id-2]
```

### Execução automática (via Task Manager Abstraction)
Quando uma fase é marcada como "Completada ✅" no `plan.md`, execute **nesta ordem** (apenas
se o provider estiver configurado):

1. **Comentário DETALHADO na SUBTASK** contendo: nome da fase concluída, arquivos
   modificados, implementações realizadas, decisões técnicas, próximos passos e
   timestamp/status. Use a formatação do provider ativo (ex.: Unicode no ClickUp — skill
   `clickup-patterns`; ADF no Jira; Markdown no Linear).
2. **Atualizar o STATUS da SUBTASK** para "done".
3. **Comentário RESUMIDO na TASK PRINCIPAL** indicando: fase X/total concluída, referência
   à subtask, próxima fase e timestamp.

## Importante

Ao desenvolver o código da fase atual, use as personas/subagents de desenvolvimento,
code-review e teste quando apropriado para preservar o máximo possível do seu contexto
(ver `.agents/AGENTS.md`).

Toda vez que completar uma fase do plano:
- **Auto-update**: adicione comentário de progresso via o Task Manager configurado.
- **Rastreamento**: marque os checkboxes na description correspondentes aos critérios completados.
- Pause e peça ao usuário para validar seu código.
- Faça as mudanças necessárias até ser aprovado.
- Atualize a fase correspondente no `plan.md`, marcando o que foi feito e adicionando
  comentários úteis para quem abordará as próximas fases (questões, decisões, etc.).
- Apenas inicie a próxima fase após o usuário concordar.

## 🔗 Referências
- Regra `task-manager-routing` (detecção de provider e roteamento)
- Task Manager Abstraction: `docs/knowledge-base/concepts/task-manager-abstraction.md`
- Padrões de comentários: skill `clickup-patterns` (`.agents/skills/`)

Agora, veja a fase atual de desenvolvimento e forneça um plano ao usuário sobre como abordá-la.
