---
description: Cria personas/subagents do Antigravity com análise de contexto, para integrar novos especialistas ao ecossistema Onion.
---

# 🤖 Criar Persona/Subagent Inteligente

Arquiteto de personas para criar especialistas contextualizados no Sistema Onion. No Antigravity, personas/subagents são declarados em `.agents/AGENTS.md`.

Entrada do usuário: o texto após o comando — `agent_name` (kebab-case,
obrigatório) e, opcionalmente, `category` e áreas de `expertise`.

## 🎯 Objetivo

Criar personas que se integram ao ecossistema existente seguindo padrões v3.0.

## ⚡ Fluxo de Execução

### Passo 1: Análise de Contexto

- Listar personas/subagents já existentes em `.agents/AGENTS.md`.
- Verificar se a categoria já existe.
- Verificar duplicação do nome.

### Passo 2: Determinar Categoria

SE a categoria foi fornecida → usar diretamente.
SENÃO → inferir da expertise:

| Expertise | Categoria |
|-----------|-----------|
| react, node, typescript | `development` |
| tasks, specs, features | `product` |
| git, branch, pr | `git` |
| iso, compliance, security | `compliance` |
| docs, writing | `review` |
| test, coverage | `testing` |
| commands, agents | `meta` |

### Passo 3: Gerar Estrutura

Definir a persona em `.agents/AGENTS.md` com a seguinte estrutura conceitual
(papel, modelo, ferramentas do Antigravity disponíveis, expertise, relacionamentos):

```markdown
## @{{agent_name}}

**Categoria**: {{category}} · **Prioridade**: [alta/média/baixa]

**Descrição**: [2 linhas]. Use para [caso de uso principal].

**Expertise**: [area-1], [area-2], [area-3]

**Ferramentas do Antigravity**: leitura/escrita/edição de arquivo, busca no
codebase, grep, listagem de diretórios, web search, gestão de tarefas — conforme
necessário ao papel.

**Relacionados**: @[persona-relacionada] · /[workflow-relacionado]

# Você é o [Nome da Persona]

## 🎯 Filosofia Core
[Descrição da filosofia e propósito]

## 🔧 Áreas de Especialização
### 1. [Área 1]
### 2. [Área 2]

## 📋 Processo de Trabalho
[Workflow da persona]

## ⚠️ Regras
- [Regra 1]
- [Regra 2]
```

### Passo 4: Validações Obrigatórias

**CRÍTICO**: Executar TODAS as validações antes de criar:

1. **Duplicação** — o nome não pode existir em `.agents/AGENTS.md`. Se existir → abortar.
2. **Categoria** — deve ser válida: `development | product | compliance | meta | review | testing | research | git`. Caso contrário → abortar.
3. **Expertise** — recomendado 3-5 áreas; avisar se fora do intervalo.

**Checklist de Validação:**
- [ ] Nome único (não existe em `.agents/AGENTS.md`)
- [ ] Categoria válida
- [ ] Expertise definida (3-5 áreas)
- [ ] Definição completa (papel, modelo, ferramentas)
- [ ] < 300 linhas

### Passo 5: Criar/Atualizar a Persona

Adicionar/atualizar a entrada da persona em `.agents/AGENTS.md`.

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ PERSONA CRIADA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📁 Local: .agents/AGENTS.md → @{{agent_name}}

📋 Detalhes:
∟ Nome: {{agent_name}}
∟ Categoria: {{category}}
∟ Expertise: [áreas]

🔗 Relacionamentos:
∟ Personas: [lista]
∟ Workflows: [lista]

🚀 Para usar: @{{agent_name}}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Personas/subagents: `.agents/AGENTS.md`
- Padrões: `docs/knowledge-base/concepts/ai-agent-design-patterns.md`
- Persona: `@agent-creator-specialist`

## ⚠️ Notas

- Sempre validar duplicação antes de criar
- Não adicionar MCPs em personas genéricas
