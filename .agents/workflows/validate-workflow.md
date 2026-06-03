---
description: Validar a completude de um workflow do Sistema Onion — sincronização, working dir, sessões, branches, compliance e integração com task manager.
---

# Validação Completa de Workflow

Verifica que todos os passos de um workflow foram executados corretamente e
identifica pendências. Entrada do usuário: o tipo de validação após o comando
(`pr-merge`, `cleanup`, `development` ou nada para validação completa).

## 🎯 Objetivo

Validar que workflows do Sistema Onion foram concluídos:
- **Git workflows:** PR, sync, branch management
- **Session management:** arquivamento e organização
- **Repository state:** sincronização e limpeza
- **Compliance:** branch protection, GitFlow

## Sintaxe

- `/validate-workflow` — validação completa do estado atual
- `/validate-workflow pr-merge` — validação específica de PR merge
- `/validate-workflow cleanup` — validação de limpeza/housekeeping
- `/validate-workflow development` — validação de desenvolvimento

## 🔍 Validações Executadas

Execute as 6 verificações abaixo e contabilize aprovações, avisos e erros.

1. **Sincronização Local vs Remoto** — commits locais pushados, sem divergência
   entre local e `origin/<branch>`, remote acessível. *(erro se divergente)*
2. **Working Directory** — sem mudanças não commitadas (`git status --porcelain`
   vazio), sem arquivos órfãos. *(aviso se pendente)*
3. **Gestão de Sessões** — se um nome de sessão foi informado, confirmar que está
   arquivada (não existe mais em `docs/sessions/<nome>/` e aparece em
   `docs/sessions/archived/`); senão, garantir que não há sessões ativas
   desnecessárias. *(aviso)*
4. **Limpeza de Branches** — sem branches `feature/`, `hotfix/` ou `bugfix/`
   órfãs ou já mescladas. *(aviso)*
5. **Compliance e Segurança** — branch protection ativa, PR workflow respeitado,
   GitFlow seguido. *(erro se ausente)*
6. **Integração com Task Manager** — status da task sincronizado, comentários e
   tags aplicados, conclusão registrada no provider ativo (ver regra
   `task-manager-routing`). *(aviso)*

## 📊 Resultado

Calcule o score de qualidade (`aprovadas / total × 100`) e classifique:

- **Erros = 0 e avisos = 0** → 🎉 WORKFLOW PERFEITO. Se houver task associada,
  registrar a validação no provider ativo.
- **Erros = 0, avisos > 0** → 🔶 VÁLIDO COM RECOMENDAÇÕES. Listar avisos e sugerir
  ajustes; reexecutar após corrigir.
- **Erros > 0** → 🚨 INCOMPLETO. Correção obrigatória antes de prosseguir; não
  avançar até resolver.

Apresente: estatísticas (total/aprovadas/avisos/erros), score em %, resultados
detalhados por verificação e o status final com ações recomendadas/obrigatórias.

## 🎯 Quando Usar

- Após `/git-sync` — validar sincronização
- Após `/engineer-pr` — validar o workflow do PR
- Após housekeeping — validar limpeza
- Diagnóstico quando algo parecer errado, ou validação final antes de deploy

## 📤 Exemplo de Saída

```
🔍 VALIDAÇÃO DE WORKFLOW: complete
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔄 [1/6] Sincronização Local vs Remoto
🧹 [2/6] Working Directory
📁 [3/6] Gestão de Sessões
🌿 [4/6] Limpeza de Branches
🛡️ [5/6] Compliance e Segurança
🔗 [6/6] Integração com Task Manager

📈 ESTATÍSTICAS: 6 verificações · 6 aprovadas · 0 avisos · 0 erros
🎯 SCORE DE QUALIDADE: 100%
🎉 STATUS: WORKFLOW PERFEITO!
```

## 🔗 Referências

- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
