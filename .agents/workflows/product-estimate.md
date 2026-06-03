---
description: Orquestra estimativas de story points via @story-points-framework-specialist, com análise de complexidade, quebra de épicos e calibração contextual. Use para estimar tarefas e calibrar velocity do time.
---

# 🎯 Estimativa de Story Points

Workflow para orquestrar estimativas ágeis usando o Framework de Story Points, integrando
análise de complexidade, decomposição de tarefas e calibração contextual.

Entrada do usuário: o texto após o comando — a descrição da tarefa/feature
(`task_description`) e, opcionalmente: nível do responsável (`assignee_level`:
junior/pleno/senior), metodologia (`methodology`: planning-poker/t-shirt/decomposition,
padrão auto-detect) e `create_task` (se true, cria a task no gerenciador com a estimativa).

## 🎯 Objetivo

Estimativas precisas e acionáveis considerando complexidade técnica, incerteza/riscos,
esforço, senioridade do responsável e métricas históricas do time.

## ⚡ Fluxo de Execução

### Passo 1: Carregar base de conhecimento
Ler o framework completo em `docs/knowledge-base/frameworks/framework_story_points.md` e
buscar métricas históricas disponíveis (velocity, accuracy rate, reference stories).

### Passo 2: Análise preliminar da tarefa
Coletar tarefa, responsável e metodologia. Classificar a natureza (técnico/negócio/
infra/integração) e detectar red flags (requisitos nebulosos, tecnologias desconhecidas,
dependências não confirmadas, impacto crítico sem rollback). Se houver red flags,
solicitar clarificações antes de estimar.

### Passo 3: Invocar agente especialista

```markdown
@story-points-framework-specialist

Analise a tarefa e forneça estimativa completa:
**Tarefa:** {{task_description}}
**Responsável:** {{assignee_level}}
**Metodologia sugerida:** {{methodology}}

Processo: 1) Análise de Domínio · 2) Seleção Metodológica (se não especificada) ·
3) Checklist apropriado · 4) Contextualização por senioridade · 5) Validação final.
Saída estruturada conforme template do agente.
```

O agente retorna: classificação do domínio, metodologia selecionada (com justificativa),
story points atribuídos (checklist 3/5/8/13), fatores de complexidade, ajustes por
contexto e recomendações.

### Passo 4: Validação e ajustes

- **Épico (>13 pontos):** alertar, propor quebra (por camadas técnicas / funcionalidades /
  complexidade), estimar cada história resultante e validar a quebra.
- **Alta incerteza (range >50%):** identificar fontes, propor spike/POC, sugerir
  estimativa conservadora (maior valor), documentar riscos/dependências.
- **Sem critérios de aceite:** solicitar definição antes de estimar e explicar o impacto
  na precisão.

### Passo 5: Criar task (opcional, se `create_task` = true)

1. Aplicar a regra `task-manager-routing` para detectar o provider; se não configurado,
   avisar e seguir apenas com output local.
2. Estruturar a task (nome, descrição, story points, complexidade, risco, fatores,
   recomendações).
3. Criar via o adapter do provider ativo; adicionar custom field "Story Points" e tags
   (complexity, risk, etc.).
4. Se a tarefa foi quebrada de um épico, criar relação parent-child e referenciar o épico.

### Passo 6: Documentar métricas (opcional)
Se houver métricas históricas: atualizar velocity, calcular accuracy rate e atualizar
reference stories.

## 📤 Output Esperado

### Formato completo
```
✅ ESTIMATIVA COMPLETA
📋 TAREFA: {{task_description}}
🎯 Domínio: natureza · componentes · tecnologias · dependências
🔧 Metodologia: técnica + justificativa
🎲 Story Points: [X] ou [X-Y] · confiança · checklist · itens marcados
⚡ Complexidade: técnica / incerteza / esforço / risco (com justificativa)
👤 Contextualização: responsável · buffer · velocity histórico
💡 Recomendações: quebra? · riscos · dependências · sugestões (pair, spike, pesquisa)
📊 Métricas (se disponível): velocity · accuracy rate · comparação histórica
🚀 Próximos passos: validar com time · definir responsável · criar task · reference story · spike
```

### Formato resumido (quick estimate)
```
🎲 ESTIMATIVA RÁPIDA: [X pontos] · Confiança: [alta/média/baixa] · Nota: [observação]
```

## 🔗 Integração com Outros Workflows

- **`/product-task`** — após estimar, criar task completa com a estimativa.
- **`/product-feature`** — estimar feature antes de especificar.
- **`/product-spec`** — incluir estimativa na especificação.

## 📋 Exemplos de Uso

- Simples: `/product-estimate "Criar API REST de usuários com autenticação JWT"` → ~8 pts.
- Com contexto: `/product-estimate "Dashboard com múltiplas visualizações" --assignee_level=junior`
  → base 5 + buffer júnior = 6 pts, pair programming sugerido.
- Épico: `/product-estimate "Sistema completo de notificações (email, SMS, push)"` →
  detectado como épico, proposta de quebra em ~6 histórias.
- Com criação: `/product-estimate "Refatorar módulo de autenticação" --create_task=true --assignee_level=senior`
  → 8 pts, task criada com custom field e tags.

## ⚠️ Regras e Validações

- **Descrição vazia** → erro: descrição é obrigatória.
- **`assignee_level` inválido** (fora de junior/pleno/senior) → aviso, usa estimativa padrão.
- **`methodology` inválida** (fora de planning-poker/t-shirt/decomposition) → aviso, usa auto-detect.
- **Anti-patterns:** tarefas >13 pts sem justificativa → propor quebra; estimativas sem
  critérios de aceite → alertar; alta incerteza (>50%) → propor spike/POC e estimativa conservadora.

## ⚠️ Notas

- Story points são esforço relativo, não horas.
- Contexto importa: considerar quem executa e o histórico do time.
- Épicos (>13 pts) precisam de justificativa forte ou quebra.
- Use métricas históricas para calibrar; valide em planning poker quando possível.

## 🔗 Referências

- **Agente:** `@story-points-framework-specialist` · Relacionados: `@product-agent`, `@task-specialist`
- **Framework:** `docs/knowledge-base/frameworks/framework_story_points.md` (escala Fibonacci,
  checklists por nível, regras de quebra de épicos, ajustes por senioridade, métricas de
  calibração, Planning Poker)
- **Relacionados:** `/product-task`, `/product-feature`, `/product-spec`
- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
