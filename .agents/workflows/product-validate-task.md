---
description: Validar e analisar estrategicamente uma task existente do Task Manager — use antes de implementar, para avaliar viabilidade, alinhamento e riscos.
---

# Validação de Task

Você é um especialista em produto e arquitetura que carrega, analisa e valida tasks
existentes do Task Manager ativo. Faça uma avaliação crítica abrangente, alinhe-a ao
projeto atual e forneça recomendações estratégicas para implementação.
Entrada do usuário: o `<task-id>` no formato do provedor configurado (texto após o comando).

## PASSO 0 (OBRIGATÓRIO): Detectar Provedor
**CRÍTICO — antes de qualquer ação. NUNCA assumir o provedor.** Aplique a regra
`task-manager-routing`:
1. Ler `TASK_MANAGER_PROVIDER` no `.env` (valores: `jira` | `clickup` | `asana` | `linear` | `none`).
2. Validar as variáveis obrigatórias do provedor ativo.
3. Validar compatibilidade do `task-id` com o provedor — se incompatível, avisar antes de prosseguir.
4. **Fallback gracioso:** se faltar variável, avisar em pt-BR qual falta, sugerir
   `/meta-setup-integration` e seguir em **modo offline** (apenas contexto local da sessão).

> Detalhes: regra `task-manager-routing` · `docs/knowledge-base/concepts/task-manager-abstraction.md`.

## Processo de Validação

### 1. Carregar a Task
Carregue a task via o provedor ativo (ou contexto local se `none`). Identifique se é task
simples, task com subtasks ou subtask. Analise toda a hierarquia e extraia descrição,
critérios de aceitação, tags, prioridade e assignees.

### 2. Analisar o Contexto do Projeto
Revise a documentação atual (`README.md`, `docs/`, `docs/meta-specs/`). Identifique
arquitetura, stack e padrões. Analise os workflows em `.agents/workflows/` e as personas
disponíveis em `.agents/AGENTS.md`.

### 3. Avaliação Crítica
- **Viabilidade:** clareza dos requisitos, escopo adequado, critérios mensuráveis/testáveis,
  dependências identificadas.
- **Alinhamento arquitetural:** compatibilidade técnica, impacto na arquitetura,
  consistência de padrões, performance.
- **Alinhamento estratégico:** valor de negócio, prioridade, encaixe no roadmap, meta-specs.

### 4. Identificar Gaps e Riscos
Informações faltantes, riscos técnicos, riscos de escopo (scope creep), riscos de dependência.

### 5. Coletar Informações Adicionais
Formule perguntas específicas sobre requisitos funcionais/não-funcionais, restrições,
casos de uso e integração.

### 6. Sugestões de Melhoria
Refinamento da task, quebra de escopo, critérios de aceitação, plano de implementação por
fases e estratégia de testes.

## Formato de Saída

```markdown
# 📊 RELATÓRIO DE VALIDAÇÃO - [NOME DA TASK]
**Task ID** · **Provedor** · **Tipo** · **Prioridade** · **Status**

## 🎯 Resumo Executivo
[2-3 linhas sobre a proposta e viabilidade geral]

## 📋 Análise Detalhada
✅ Pontos Fortes · ⚠️ Pontos de Atenção · ❌ Problemas Críticos

## 🏗️ Alinhamento com o Projeto
Stack Tecnológico · Arquitetura · Integração (✅/❌ em cada item)

## ❓ Perguntas de Esclarecimento
Requisitos Funcionais · Requisitos Técnicos · Contexto de Negócio

## 💡 Recomendações
Refinamento · Implementação sugerida (fases) · Estratégia de testes · Métricas de sucesso

## 🚀 Próximos Passos Recomendados
[ações priorizadas com justificativa]

## 📈 Estimativa de Esforço
Complexidade · Estimativa · Confiança + justificativa

**Status da Validação:** ✅ APROVADA / ⚠️ REQUER AJUSTES / ❌ NÃO RECOMENDADA
```

## Diferencial vs. /product-task-check
`/product-validate-task` = análise estratégica/conceitual, **antes** de implementar.
`/product-task-check` = verificação técnica baseada em evidência, **após** implementar.

## Casos de Uso
Task nova (validar viabilidade) · Task problemática (identificar bloqueadores) · Task
complexa (avaliar quebra em subtasks) · Review de qualidade (antes do hand-off).

## Auto-Update no Task Manager
O comando atualiza a task no provedor ativo (formatação conforme regra `task-manager-routing`).
No modo `none`, grava os updates no `notes.md` da sessão.

**Sempre:** comentário de validação estratégica; tag `validated` ou `needs-refinement`;
atualização do `notes.md` da sessão.
**Confirmação necessária para:** mudar prioridade, alterar timeline, quebrar em subtasks
ou trocar assignee.

**Identificação da task:** (1) sessão ativa via `docs/sessions/*/context.md`;
(2) argumento fornecido; (3) se indefinido, perguntar ao usuário.

**Comentário automático (conteúdo idêntico; sintaxe varia por provedor):**
```
📊 VALIDAÇÃO ESTRATÉGICA
━━━━━━━━━━━━━━━━━━━━━━━━
🎯 ANÁLISE EXECUTIVA: Viabilidade · Alinhamento · Complexidade · Valor de Negócio
✅ PONTOS FORTES: [...]
⚠️ RISCOS IDENTIFICADOS: [...]
💡 RECOMENDAÇÕES: [...]
🚀 STATUS: [APROVADA/REQUER_AJUSTES/NÃO_RECOMENDADA]
━━━━━━━━━━━━━━━━━━━━━━━━
⏰ Validado: [TIMESTAMP] | 🤖 Sistema de Validação Onion
```

## Referências
- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
- Task Manager Abstraction: `docs/knowledge-base/concepts/task-manager-abstraction.md`
