---
description: Workflow de emergência end-to-end — cria task no Task Manager, branch de hotfix e prepara o desenvolvimento. Use para correções urgentes em produção.
---

# 🔥 Engineer Hotfix

Workflow de hotfix completo em um único comando: Task + Branch + Desenvolvimento.

Entrada do usuário (após o comando): a descrição do hotfix (obrigatória) e,
opcionalmente, IDs de tasks relacionadas e tags adicionais.

## 🎯 Objetivo

Executar o workflow de hotfix end-to-end de forma rápida e segura.

## ⚡ Fluxo de Execução

### Passo 1: Validar Input
- Confirmar que a descrição do hotfix foi fornecida; caso contrário, parar e pedir.
- Verificar a branch atual: o recomendado é iniciar de `main`/`master`/`develop`.
  Se estiver em outra branch, avisar antes de prosseguir.

### Passo 2: Criar Task Emergencial
Detectar o provider ativo (regra `task-manager-routing`) e criar a task no provider
configurado. Se `none`, registrar apenas localmente. Conteúdo sugerido:

- **Nome**: `🔥 HOTFIX: <descrição>`
- **Prioridade**: urgente
- **Tags**: `hotfix`, `urgent` + tags adicionais informadas
- **Status**: In Progress
- **Descrição** com checklist: Diagnóstico → Implementação → Testes → Deploy

### Passo 3: Criar Branch Hotfix
1. Garantir `main` atualizada: `git checkout main` e `git pull origin main`.
2. Calcular a próxima versão de patch a partir da versão atual (`pyproject.toml`/`package.json`).
3. Criar a branch: `hotfix/<patch>-<slug-da-descrição>` (slug em minúsculas, hífens, máx. ~30 chars).

### Passo 4: Setup da Sessão
- Criar `docs/sessions/hotfix-<YYYYMMDD>/` (contexto versionado, complementar aos Artifacts do Antigravity).
- Criar `context.md` registrando: ID/URL da task, nome e base da branch, descrição.

### Passo 5: Iniciar Desenvolvimento
Exibir resumo do setup e os próximos passos:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔥 HOTFIX INICIADO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Task: [URL da task no provider]
🌿 Branch: hotfix/X.X.X-description

⚡ Próximos Passos:
1. Implementar correção
2. Testar localmente
3. /engineer-pre-pr
4. /git-hotfix-finish
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 📤 Output Esperado (Sucesso)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ HOTFIX SETUP COMPLETO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Task Manager:
∟ Task: 🔥 HOTFIX: <descrição>
∟ ID: <id>
∟ Status: In Progress
∟ Priority: Urgent

🌿 Git:
∟ Branch: hotfix/1.2.3-fix-description
∟ Base: main
∟ Remote: origin

📁 Session:
∟ Path: docs/sessions/hotfix-20251124/

🚀 Comandos:
∟ Desenvolver: /engineer-work
∟ Pre-PR: /engineer-pre-pr
∟ Finalizar: /git-hotfix-finish
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências
- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md` (ex.: `@gitflow-specialist`)
- Padrões Git: skill `git-workflow-patterns` (`.agents/skills/`)
- Comandos relacionados: `/git-hotfix-start`, `/git-hotfix-finish`, `/product-task`

## ⚠️ Notas
- Sempre parte de `main` ou `master`.
- Task criada com prioridade máxima.
- Merge automático para `main` E `develop` no finish.
