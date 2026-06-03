---
description: Sincronização pós-merge de branches com GitFlow e proteção automática de branches críticas (main/master/develop).
---

# 🔄 Git Sync - Sincronização com GitFlow

Sincronização pós-merge de branches com proteção automática. Entrada do usuário:
o texto após o comando (branch alvo opcional; default `develop`).

## 🎯 Objetivo

Automatizar sincronização após merge de PRs seguindo GitFlow.

## 🛡️ Branches Protegidas

| Branch | Push Direto | Fast-Forward |
|--------|-------------|--------------|
| `main` | ❌ Bloqueado | ✅ Permitido |
| `master` | ❌ Bloqueado | ✅ Permitido |
| `develop` | ❌ Bloqueado | ✅ Permitido |

## ⚡ Fluxo de Execução

### Passo 1: Detectar Contexto
- Identificar branch atual (`CURRENT`).
- Definir branch alvo (`TARGET`, default `develop` ou o valor informado).
- Marcar como protegida se `TARGET` for `main`, `master` ou `develop`.

### Passo 2: Validar Estado
- Se houver alterações não commitadas → avisar e interromper (commit ou stash antes).
- Executar `git fetch origin --prune`.

### Passo 3: Análise GitFlow

Consultar @gitflow-specialist para estratégia:

| Branch Atual | Target | Estratégia |
|--------------|--------|------------|
| `feature/*` | `develop` | `feature-cleanup` |
| `release/*` | `main` | `release-sync` |
| `hotfix/*` | `main` | `hotfix-sync` |
| `develop` | `main` | `protected-sync` |

Referência: `common/prompts/git-workflow-patterns.md`

### Passo 4: Executar Sync

**Para branches normais:**
```bash
git checkout $TARGET
git pull origin $TARGET
git checkout $CURRENT
git merge $TARGET --no-edit
```

**Para branches protegidas** — apenas fast-forward permitido:
```bash
git checkout $TARGET
git merge origin/$TARGET --ff-only
```
Se o fast-forward falhar, instruir o workflow de PR: `/engineer-pr`.

### Passo 5: Cleanup (se feature finalizada)
- Se a feature ainda existe no remote, apenas informar.
- Se já foi deletada no remote, perguntar se deseja deletar a branch local.

### Passo 6: Atualizar Task Manager

Se há sessão ativa com task_id: comentar o sync realizado e atualizar status se necessário.

## 📤 Output Esperado

### Sync Sucesso
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ SYNC CONCLUÍDO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Resumo:
∟ Branch atual: feature/user-auth
∟ Sincronizado com: develop
∟ Commits atualizados: 5
∟ Conflitos: 0

🚀 Próximo: /engineer-work
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Branch Protegida - Bloqueio
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🛡️ BRANCH PROTEGIDA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
❌ Push direto em 'develop' bloqueado

📋 Workflow Correto:
1. git checkout -b feature/my-changes
2. [fazer alterações]
3. /engineer-pr
4. [merge via GitHub/GitLab]
5. /git-sync develop
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Conflito Detectado
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠️ CONFLITOS DETECTADOS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📁 Arquivos em conflito:
∟ src/components/Button.tsx
∟ src/utils/helpers.ts

💡 Resolução:
1. Editar arquivos manualmente
2. git add [arquivos]
3. git commit
4. /git-sync (novamente)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Padrões: `common/prompts/git-workflow-patterns.md`
- Agente: @gitflow-specialist

## ⚠️ Notas

- Sempre fazer `git fetch` antes
- Branches protegidas só aceitam fast-forward
- Em caso de conflito, resolver manualmente
