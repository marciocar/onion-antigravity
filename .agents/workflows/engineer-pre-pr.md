---
description: Executa validações finais de padrões e qualidade antes de abrir o PR e atualiza a task no Task Manager. Use ao finalizar o trabalho de uma branch.
---

# Pre-PR — Validação Completa Antes do Pull Request

Estamos nos aproximando de finalizar o trabalho nesta branch e preparar o pull request.
É hora de fazer verificações finais e limpezas para garantir alinhamento com nossos
padrões e objetivos.

## 🔄 Auto-Update do Task Manager

Este comando **atualiza automaticamente** a task no Task Manager configurado durante a
preparação para PR. Antes de operar, aplique a regra `task-manager-routing`: carregue o
`.env` e leia `TASK_MANAGER_PROVIDER` (`jira` | `clickup` | `asana` | `linear` | `none`).
Se `none`, gere o relatório de validação localmente sem persistir.

### Updates Automáticos SEMPRE
- **Validação de critérios de aceitação** — verifica todos os checkboxes.
- **Comentário de preparação** com checklist completo.
- **Tag `ready-for-pr`** quando todas as verificações passam.
- **Tag `needs-fixes`** se alguma verificação falha.
- **Progresso estimado** para 90% (quase pronto).

### Formato do Comentário de Pre-PR
O comentário deve conter: resultado da validação de critérios de aceitação (completo?
cobertura? critérios pendentes?), checks técnicos (meta specs, code review, testes) e
indicador `readyForPR`.

Roteamento por provider (carregar `.env` → ler `TASK_MANAGER_PROVIDER` → seguir o adapter):
- **`clickup`** → comentário em formatação Unicode via `@clickup-specialist` (skill `clickup-patterns`).
- **`jira`** → comentário em ADF via `@jira-specialist`.
- **`asana`** → comentário (story) via `@task-specialist`.
- **`linear`** → comentário em Markdown via `@task-specialist`.
- **`none`** → gerar relatório localmente, sem persistir.

Detalhes: regra `task-manager-routing` e `docs/knowledge-base/concepts/task-manager-abstraction.md`.

### Identificação da Task
1. **context.md**: lê o task ID da sessão ativa em `docs/sessions/[slug]/`.
2. **Branch atual**: detecta automaticamente pela branch git.

## Checklist de Preparação

### ✅ Validação de Critérios de Aceitação
1. **Extrair critérios** — ler checkboxes da description da task/subtask.
2. **Validar cobertura** — confirmar que TODOS os checkboxes estão marcados `[x]`.
3. **Gerar relatório** — criar lista de critérios validados.
4. **Bloquear se incompleto** — se algum critério não estiver marcado, indicar no comentário.

### 🔧 Validações Técnicas
1. Acione `@branch-metaspec-checker` para verificar alinhamento da branch com as meta specs do projeto.
2. Acione `@branch-code-reviewer` para revisar o código e garantir que está pronto para lançar.
3. Acione `@branch-documentation-writer` para atualizar a documentação do projeto.
4. Acione `@branch-test-planner` para finalizar a escrita de testes para a branch.

(Personas/subagents do Antigravity — ver `.agents/AGENTS.md`.)

### 📋 Auto-Update
5. **Validar critérios de aceitação** — verificar todos os checkboxes.
6. **Adicionar comentário de preparação** no Task Manager configurado automaticamente (conforme `TASK_MANAGER_PROVIDER`).
7. **Aplicar tags** (`ready-for-pr` ou `needs-fixes`).
8. **Atualizar progresso** para 90%.

Você também precisará lidar com todo o feedback que essas personas fornecerem e fazer
mudanças e correções conforme necessário.

Uma vez terminado E todos os critérios de aceitação validados, avise-me e peça permissão
para abrir o Pull Request (`/engineer-pr`).
