---
description: Refinar requisitos via perguntas de esclarecimento e recalcular story points — use para amadurecer um requisito inicial até um documento pronto para desenvolvimento.
---

# Refinamento de Requisitos

Você é um especialista em produto que ajuda o humano a refinar requisitos. O objetivo é
pegar um requisito inicial, fazer perguntas de esclarecimento, resumir o entendimento e
salvar os requisitos refinados.
Entrada do usuário: o requisito a analisar (texto após o comando).

## 1. Fase de Esclarecimento
Leia o requisito inicial. Faça perguntas de esclarecimento até ter clareza abrangente
sobre o objetivo da funcionalidade e seus detalhes.

## 2. Fase de Resumo e Aprovação
Quando tiver informação suficiente, apresente um resumo do entendimento:

> Com base na nossa discussão, aqui está meu entendimento dos requisitos:
> [resumo conciso da funcionalidade, objetivos e requisitos principais]
> Este entendimento está correto? Quer fazer mudanças ou adições?

Incorpore feedback e reapresente o resumo até aprovação. Pesquise no codebase ou na
internet se necessário antes de se comprometer com uma saída.

## 3. Salvar os Requisitos
Após a aprovação, salve os requisitos. A localização depende da origem:
- Se o refinamento foi feito sobre um arquivo → edite o arquivo.
- Se foi sobre uma task do Task Manager ativo → atualize a task (regra `task-manager-routing`).

## 4. Recalcular Story Points (Automático)
**CRÍTICO:** após refinamento, SEMPRE recalcular story points e manter histórico.

1. **Obter estimativa anterior (se existir):** ler o campo "Story Points" atual e
   comentários anteriores com estimativas; identificar a última registrada.
2. **Recalcular** via @story-points-framework-specialist, passando título refinado,
   descrição completa após refinamento, estimativa anterior e as mudanças que afetam esforço.
3. **Comparar e documentar histórico:** registrar data, estimativa anterior, nova
   estimativa, delta, motivo (refinamento) e mudanças identificadas.
4. **Atualizar a task no Task Manager ativo (se aplicável):** atualizar o campo
   "Story Points" e adicionar comentário com o histórico, conforme a formatação do
   provedor (regra `task-manager-routing`). Exemplo de comentário:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔄 ESTIMATIVA ATUALIZADA APÓS REFINAMENTO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📅 Data: [data]
📊 HISTÓRICO: anterior X pts → nova Y pts (Δ ±Z)
⚡ ANÁLISE: Complexidade / Risco / Incerteza
📝 MUDANÇAS QUE AFETARAM A ESTIMATIVA: [lista]
💡 RECOMENDAÇÕES: [...]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Template de Saída dos Requisitos

```markdown
# [NOME DA FUNCIONALIDADE]

## POR QUE
[razões para construir esta funcionalidade]

## O QUE
[o que precisa ser construído/modificado — inclua funcionalidades afetadas]

## COMO
[detalhes úteis para um Desenvolvedor IA]

## 📊 ESTIMATIVA DE STORY POINTS
**Atual:** [X] pontos

**Histórico de Mudanças:**
| Data | Estimativa | Mudança | Motivo |
|------|------------|---------|--------|
| [data inicial] | [X] pts | - | Criação inicial |
| [data refinamento] | [Y] pts | [±Z] | Refinamento de requisitos |

**Análise Atual:** Complexidade / Risco / Incerteza
**Fatores que influenciaram:** [lista]
```

A audiência deste documento é um Desenvolvedor IA com capacidades e contexto similares
ao seu. Seja conciso, mas forneça informação suficiente para a IA entender e prosseguir.

## Referências
- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
- Comandos relacionados: `/product-estimate`, `/product-task`
- Framework: `docs/knowledge-base/frameworks/framework_story_points.md`
