---
description: Cria task de feature no gerenciador configurado com tag backlog e estimativa automática de story points, para planejamento e priorização sem iniciar desenvolvimento. Use para organizar o product backlog.
---

# 🎯 Criar Feature — Task para Planejamento

Workflow para criar tasks de feature no gerenciador configurado (via Task Manager
abstraction) para planejamento e backlog, seguindo o padrão do Sistema Onion. Cria tasks
de backlog para organização e priorização **sem iniciar desenvolvimento**.

Entrada do usuário: o texto após o comando — nome ou descrição da feature.

## 🎯 Funcionalidades

- **Criar task de backlog** — task com tag "backlog", status "Backlog" (aguardando
  planejamento/priorização), no projeto da sessão atual ou no projeto padrão.
- **Integração inteligente** — auto-detecção de projeto/lista, herança de contexto da
  sessão ativa, links com tasks relacionadas, tags apropriadas, suporte a múltiplos
  provedores (ClickUp, Asana, Linear).

## ⚙️ Workflow Automático

### 1. Validação de parâmetros
Se nenhum nome de feature for fornecido, instruir o uso correto e parar. Sanitizar o nome
em um `FEATURE_SLUG` (kebab-case, sem caracteres especiais).

### 2. Detecção de contexto via Task Manager
Aplicar a regra `task-manager-routing` para detectar o provider ativo. Detectar o
projeto/lista alvo: tentar obter da sessão ativa (`docs/sessions/<feature>/context.md`);
se não houver, usar o projeto padrão configurado.

### 3. Criação da task via Task Manager
Criar a task com título `🚀 <FEATURE_NAME>`, status `backlog`, prioridade `medium` e tags
`feature, backlog, planning`. A descrição deve incluir:

- **Feature para planejamento** (tipo, status, criada via `/product-feature`)
- **Descrição** da feature
- **Workflow de desenvolvimento** — para iniciar depois:
  `/git-feature-start "<FEATURE_SLUG>"` ou `/engineer-start <FEATURE_SLUG>`;
  sequência recomendada: Planejamento → `/git-feature-start` → `/engineer-work` →
  `/git-sync` → `/engineer-pr`.
- **Critérios de aceitação** (planejamento + desenvolvimento)
- **Tags e categorização** (type/status/priority/phase)

### 4. Estimar story points (automático)

**CRÍTICO:** após criar a task, SEMPRE estimar story points.

```markdown
@story-points-framework-specialist

Analise e estime esta feature de backlog:
**Feature:** <FEATURE_NAME>
**Descrição:** [descrição da feature]
**Status:** Backlog (planejamento inicial)

Forneça estimativa inicial de story points para planejamento.
```

Atualizar a task: setar o custom field "Story Points" e adicionar comentário com a análise
(formato conforme o provider — ver regra `task-manager-routing`), indicando que é uma
estimativa inicial refinável no refinement.

### 5. Resultado e links
Apresentar: título, ID, status (Backlog), URL, tags, story points (estimativa inicial) e
próximos passos. Adicionar comentário inicial na task (setup, planejamento, caminho para
desenvolvimento, timestamp). Tratar falhas com graceful degradation (comentário pode
falhar sem invalidar a criação da task).

## 🔗 Integração com Sistema Onion

Separação de responsabilidades:
- **`/product-feature`** — cria task de backlog para **planejamento**.
- **`/git-feature-start`** — inicia desenvolvimento **GitFlow** (branch + session).
- **`/git-sync`** — finaliza desenvolvimento (pós-merge + cleanup).

Sequência: `/product-feature "nova-funcionalidade"` (planejamento) →
`/git-feature-start "nova-funcionalidade"` (desenvolvimento) → `/git-sync` (finalização).

### Quando usar
- ✅ Criar features para backlog e roadmap planning
- ✅ Organizar product backlog e priorização
- ✅ Capturar ideias de features rapidamente
- ✅ Setup inicial de projetos com múltiplas features

### Quando NÃO usar
- ❌ Desenvolvimento imediato → use `/git-feature-start`
- ❌ Hotfixes urgentes → use `/engineer-hotfix`
- ❌ Tasks já existentes → use `/engineer-start <feature-slug>`

## ⚠️ Tratamento de Erros

- **Nome não fornecido** → instruir uso correto.
- **Task Manager falhou** → verificar `TASK_MANAGER_PROVIDER` e permissões; oferecer
  criação manual como fallback (título, tags `feature/backlog/planning`, status Backlog).
- **Lista não encontrada** → rodar a partir de sessão ativa ou configurar lista padrão.
- Para reconfigurar integrações, sugerir `/meta-setup-integration`.

## 💡 Dicas de Uso

- ✅ Nomes descritivos e específicos: "implement-oauth2-authentication-flow".
- ✅ Features modulares e focadas: "add-user-profile-avatar-upload".
- ❌ Evitar genéricos ("melhorias"), técnicos internos ("refactor-class-x") ou bugs
  (use `/engineer-hotfix`).

## Referências

- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
- Task Manager Abstraction: `docs/knowledge-base/concepts/task-manager-abstraction.md`
- **Agentes:** `@story-points-framework-specialist`, `@product-agent`
- **Relacionados:** `/product-estimate`, `/product-task`
