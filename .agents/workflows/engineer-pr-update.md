---
description: Atualiza um PR existente com mudanças adicionais — commit inteligente, push e documentação no Task Manager. Use após /engineer-pr quando fizer mudanças subsequentes.
---

# 🔄 Engineer PR Update

Atualiza um Pull Request existente com mudanças adicionais, automatizando commit, push e
documentação. Use quando você já executou `/engineer-pr` mas fez mudanças subsequentes.

Entrada do usuário (opcional, como prose após o comando): `--type <fix|feat|docs|refactor|...>`
para forçar o tipo de commit, `--message "..."` para mensagem personalizada, ou `--dry-run`
para preview sem executar.

## 🎯 Funcionalidades

### Detecção Automática de Contexto
- Identifica a branch de feature ativa.
- Detecta mudanças pendentes (staged/unstaged/untracked).
- Valida se existe PR aberto para a branch atual.
- Verifica se está na sessão de desenvolvimento correta.

### Commit Inteligente e Descritivo
- Analisa arquivos modificados para categorizar mudanças.
- Gera mensagem de commit contextual e descritiva.
- Suporta tipos (fix, feat, docs, refactor) e mantém commits atômicos.

### Sincronização Automática
- Push automático para a branch do PR existente.
- Atualização do Task Manager configurado com comentário detalhado (conforme `TASK_MANAGER_PROVIDER`).
- Validação de que o PR foi atualizado e timestamp/métricas das mudanças.

## 🤝 Integração com o Task Manager

Antes de operar com a task, aplique a regra `task-manager-routing`: carregue o `.env` e
leia `TASK_MANAGER_PROVIDER` (`jira` | `clickup` | `asana` | `linear` | `none`). Se `none`,
pule a atualização remota (apenas commit + push).

### Detecção de Task Ativa
- Lê o task ID de `docs/sessions/[slug]/context.md`.
- Identifica o PR existente através da task ou branch.
- Valida se a task está "in progress" com tag "under-review".

### Comentário Automático Padronizado
O comentário deve documentar: tipo do commit (fix | feat | refactor | docs | chore), hash
do commit, arquivos modificados, linhas adicionadas/removidas e descrição das mudanças.

Roteamento por provider (carregar `.env` → ler `TASK_MANAGER_PROVIDER` → seguir o adapter):
- **`clickup`** → comentário em formatação Unicode via `@clickup-specialist` (skill `clickup-patterns`).
- **`jira`** → comentário em ADF via `@jira-specialist`.
- **`asana`** → comentário (story) via `@task-specialist`.
- **`linear`** → comentário em Markdown via `@task-specialist`.
- **`none`** → não persistir comentário remoto.

Detalhes: regra `task-manager-routing` e `docs/knowledge-base/concepts/task-manager-abstraction.md`.

## ⚙️ Processo Automático

1. **Validação de Contexto**: confirma branch de feature + sessão ativa.
2. **Análise de Mudanças**: categoriza arquivos modificados por tipo.
3. **Geração de Commit**: cria mensagem contextual e descritiva.
4. **Staging Inteligente**: adiciona apenas arquivos relevantes.
5. **Commit & Push**: executa commit + push para a branch do PR.
6. **Atualização do Task Manager**: documenta mudanças com comentário formatado no provider configurado.
7. **Validação Final**: confirma que o PR foi atualizado com sucesso.

## 🧠 Detecção Inteligente de Tipos

Tipos de commit auto-detectados: **fix** (correções/patches), **feat** (novas
funcionalidades), **docs** (apenas documentação), **refactor** (sem mudança de
funcionalidade), **style** (formatação/lint), **test** (testes), **chore** (manutenção).

Categorização automática por arquivos modificados:
- `.agents/workflows/` → "feat/fix: workflow updates"
- `docs/` → "docs: documentation updates"
- `tests/` → "test: test updates"
- `*.md` (session files) → "chore: session documentation"
- Múltiplos tipos → "chore: multiple updates"

## ⚠️ Resolução de Problemas

- **"Não há PR ativo para esta branch"** → execute `/engineer-pr` primeiro para criar o PR inicial.
- **"Nenhuma mudança detectada"** → verifique `git status` para confirmar mudanças pendentes.
- **"Branch não está sincronizada"** → `git pull origin [branch-name]` antes de atualizar.
- **"Task do Task Manager não encontrada"** → confirme o task ID em `docs/sessions/[slug]/context.md` e valide se a task existe/está acessível.

## 💡 Casos de Uso Comuns
- **Correções pós-review**: após feedback, faça as correções e use `--type fix`.
- **Melhorias adicionais**: após implementar enhancements, use `--type feat`.
- **Documentação esquecida**: após atualizar docs, use `--type docs`.
- **Correções arquiteturais**: use `--type fix --message "Correção arquitetural - Phase→Subtask sync"`.

## 🔗 Integração com Workflow

Fluxo padrão completo:
1. `/product-task` — criar task no Task Manager configurado
2. `/engineer-start` — iniciar desenvolvimento
3. `/engineer-work` — desenvolver features
4. `/engineer-pre-pr` — validações finais
5. `/engineer-pr` — criar Pull Request
6. **`/engineer-pr-update`** — atualizar PR com mudanças adicionais (quantas vezes necessário)
7. Merge do PR → auto-sync `/git-sync`

Compatibilidade: funciona após `/engineer-pr`, integra com o progress tracking do
`/engineer-work`, é compatível com o `/git-sync` automático pós-merge e respeita o
mapeamento Phase→Subtask do `context.md`.

---

**🎯 Valor agregado**: elimina o processo manual de atualização de PRs, automatizando
commit inteligente, push e documentação no Task Manager configurado em uma única operação.
