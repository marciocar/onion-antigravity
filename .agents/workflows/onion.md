---
description: Ponto de entrada inteligente para o Sistema Onion no Antigravity — navegação, recomendações e orquestração de workflows.
---

# 🧅 Onion

Atalho inteligente para o orquestrador master @onion. Entrada do usuário: sua
pergunta ou necessidade após o comando (ex.: `/onion "como criar task"`).

## 🎯 Objetivo

Acessar o Sistema Onion (rodando sobre o Antigravity) para navegação, recomendações
e orquestração de workflows.

## 🔄 Fluxo de Execução

### Passo 1: Detectar Tipo de Solicitação

| Tipo | Indicadores | Ação |
|------|-------------|------|
| 🆘 Ajuda | "como", "o que" | Explicar o sistema |
| 🎯 Tarefa | "criar", "fazer" | Recomendar workflow |
| 🔍 Busca | "qual", "onde" | Encontrar recurso |
| 🔧 Problema | "erro", "não funciona" | Diagnosticar |
| 🔄 Workflow | "do zero", "completo" | Orquestrar fluxo |

### Passo 2: Preparar Contexto

Detectar sessões ativas (`docs/sessions/*/context.md`) e o estado do Git (branch
atual e `git status --short`).

### Passo 3: Invocar @onion

Delegar à persona @onion (`.agents/AGENTS.md`) com o contexto coletado.

## 📤 Respostas Comuns

### Ajuda Geral

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧅 SISTEMA ONION (sobre Antigravity)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Estrutura:
∟ Workflows em .agents/workflows/ (categorias: product, git, engineer,
  docs, meta, validate, test, development, quick)
∟ Personas/subagentes em .agents/AGENTS.md
∟ Skills em .agents/skills/ · Regras em .agents/rules/
∟ Task Manager Abstraction (Jira/ClickUp/Asana/Linear) via regra task-manager-routing

🚀 Workflows Principais:
∟ /product-task      — Criar tasks
∟ /engineer-start    — Iniciar feature
∟ /engineer-work     — Continuar trabalho
∟ /git-feature-start — Criar branch

💡 Use: /onion "sua pergunta"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Recomendação de Workflow

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💡 RECOMENDAÇÃO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Para: "criar task no ClickUp"
✅ Use: /product-task [descrição]
📋 Exemplo: /product-task Implementar autenticação OAuth2
🔗 Relacionados: /product-spec · /product-feature
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Workflow Completo

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔄 WORKFLOW: Feature Completa
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. /product-task [nome]
2. /engineer-start [feature-slug]
3. /git-feature-start
4. /engineer-work
5. /engineer-pre-pr
6. /engineer-pr
7. /git-sync
💡 Dica: cada workflow sincroniza com o task manager ativo
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## ⚠️ Notas

- Sempre começa pelo contexto do workspace e detecta sessões ativas
  (`docs/sessions/`) automaticamente. Para ajuda específica de persona: @nome.

## 🔴 REGRA CRÍTICA: Criação de Tasks

**SEMPRE criar tasks no task manager configurado.** Ao usar workflows que criam
tasks (`/product-task`, `/product-feature`):
1. ✅ Detectar o provedor via regra `task-manager-routing` (lê `TASK_MANAGER_PROVIDER`
   no `.env`).
2. ✅ Criar task principal + subtasks no provedor configurado.
3. ✅ Adicionar comentários e atualizar status.
4. ❌ NUNCA criar apenas documentos locais sem sincronizar.
5. ❌ NUNCA ignorar o provedor do `.env`.

**Provedores:** ClickUp · Asana · Linear · Jira · `none` (modo offline). Esta regra é
OBRIGATÓRIA.

## 🔗 Referências

- Persona master: @onion (`.agents/AGENTS.md`) · Regra `task-manager-routing`
- Relacionados: `/product-task` · `/engineer-start` · `/git-feature-start`
