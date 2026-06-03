---
description: Auditar tecnicamente se uma task do Task Manager foi realmente implementada no código — use após implementar, para verificação factual baseada em evidência.
---

# Verificação de Implementação de Task

Você é um especialista em validação técnica que verifica se uma task do Task Manager ativo
foi **realmente implementada** no projeto. Faça uma auditoria prática comparando o que foi
solicitado vs. o que existe no código.
Entrada do usuário: o `<task-id>` no formato do provedor configurado (texto após o comando).

## Objetivo
Determinar factualmente se a task está: ✅ completamente implementada · ⚠️ parcialmente
implementada · ❌ não implementada · 🚀 pronta para a próxima fase.

## PASSO 0 (OBRIGATÓRIO): Detectar Provedor
**CRÍTICO — antes de qualquer ação. NUNCA assumir o provedor.** Aplique a regra
`task-manager-routing`:
1. Ler `TASK_MANAGER_PROVIDER` no `.env` (valores: `jira` | `clickup` | `asana` | `linear` | `none`).
2. Validar as variáveis obrigatórias do provedor ativo.
3. Validar compatibilidade do `task-id` com o provedor — se incompatível, avisar antes de prosseguir.
4. **Fallback gracioso:** se faltar variável, avisar em pt-BR qual falta, sugerir
   `/meta-setup-integration` e seguir em **modo offline** (apenas contexto local da sessão,
   sem chamadas de API).

> Detalhes: regra `task-manager-routing` · `docs/knowledge-base/concepts/task-manager-abstraction.md`.

## Processo de Verificação

### 1. Carregar e Analisar a Task
Carregue a task via o provedor ativo (ou contexto local da sessão se `none`). Extraia
todos os requisitos, critérios de aceitação mensuráveis, arquivos/componentes esperados,
subtasks e dependências.

### 2. Auditar o Projeto Atual
Examine a estrutura atual: arquivos modificados relacionados, funcionalidades
implementadas, testes criados/atualizados e documentação adicionada.

### 3. Comparação Detalhada
Para cada requisito, marque ✅/❌:
- **Funcionais:** funcionalidade, comportamento, regra de negócio, interface/API.
- **Técnicos:** arquivos criados/modificados, componentes, integração, performance.
- **Qualidade:** testes unitários, testes de integração, documentação, code review.

### 4. Análise de Código Específica
Buscar arquivos relacionados, analisar commits recentes, verificar imports/exports novos,
testar funcionalidades quando possível, validar configurações.

### 5. Identificar Gaps
Liste: o que falta, o que foi feito além, o que foi feito diferente e problemas encontrados.

## Formato de Saída

```markdown
# 🔍 VERIFICAÇÃO DE IMPLEMENTAÇÃO - [NOME DA TASK]
**Task ID** · **Provedor** · **Data** · **Status Verificado**

## 📋 Resumo da Task
Descrição · Critérios de Aceitação · Arquivos/Componentes esperados

## ✅ Implementação Verificada
Funcionalidades completas · Arquivos criados/modificados · Testes implementados

## ⚠️ Implementação Parcial
Funcionalidades incompletas · Gaps identificados

## ❌ Não Implementado
Funcionalidades ausentes · Arquivos faltantes

## 🔍 Evidências Técnicas
Análise de código · Commits relacionados · Configurações verificadas

## 🚀 Avaliação de Prontidão (próxima fase)
Status · Justificativa · Bloqueadores · Recomendações

## 📝 Próximas Ações Recomendadas
Para completar a task · Para a próxima fase

## 📈 Métricas de Completude
Funcionalidades · Testes · Documentação · Qualidade · Score Geral

## 🔄 Recomendação Final
✅ APROVAR / ⚠️ REQUER AJUSTES / ❌ REFAZER / 🚀 AVANÇAR + justificativa + próximo passo
```

## Diferencial vs. /product-validate-task

| Aspecto | `/product-validate-task` | `/product-task-check` |
|---------|--------------------------|------------------------|
| Foco | Análise estratégica | Verificação técnica |
| Objetivo | Validar requisitos | Auditar implementação |
| Método | Conceitual | Baseado em evidência |
| Quando usar | Antes de implementar | Após implementar |

## Auto-Update no Task Manager
O comando atualiza a task no provedor ativo (formatação conforme regra `task-manager-routing`).
No modo `none`, grava os updates no `notes.md` da sessão.

**Sempre:** comentário de verificação com resultados; tag `verified` (se passou) ou
`needs-work` (se há gaps críticos); atualização do `notes.md` da sessão.
**Confirmação necessária para:** mudar status para "Done", mudar prioridade, quebrar em
subtasks ou reatribuir.

**Identificação da task:** (1) sessão ativa via `docs/sessions/*/context.md`;
(2) argumento fornecido; (3) se indefinido, perguntar ao usuário.

**Comentário automático (conteúdo idêntico; sintaxe varia por provedor):**
```
🔍 VERIFICAÇÃO DE IMPLEMENTAÇÃO
━━━━━━━━━━━━━━━━━━━━━━━━
📊 RESULTADO: Status · Completude % · Arquivos verificados
✅ IMPLEMENTADO: [...]
⚠️ PENDENTE: [...]
🎯 PRÓXIMOS PASSOS: [...]
━━━━━━━━━━━━━━━━━━━━━━━━
⏰ Verificado: [TIMESTAMP] | 🤖 Sistema de Verificação Onion
```

## Integração com o Sistema Onion
Integra com `/product-task`, `/engineer-start`, `/product-validate-task` e as sessões em
`docs/sessions/`. Se houver sessão ativa relacionada à task, analise `context.md`,
`architecture.md` e `plan.md`, e atualize `notes.md` com os resultados.

## Referências
- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
- Task Manager Abstraction: `docs/knowledge-base/concepts/task-manager-abstraction.md`
