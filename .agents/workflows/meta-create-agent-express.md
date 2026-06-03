---
description: Cria uma persona/subagent do Antigravity de forma rápida e simplificada a partir dos requisitos do usuário.
---

# Criar Persona/Subagent (Express)

Você tem a tarefa de criar uma nova persona/subagent do Antigravity baseada nos requisitos do usuário. Siga esta abordagem sistemática para construir uma persona bem estruturada.

Entrada do usuário: o texto após o comando — os requisitos da persona.

## Processo

### 1. Entender o Propósito da Persona
Analise o que o usuário quer que esta persona faça:
- Qual é a responsabilidade principal?
- Que tarefas ela executará?
- O que a torna especializada?

### 2. Definir Configuração da Persona
Com base nos requisitos, determine:
- **Nome**: identificador em minúsculas, separado por hífens
- **Descrição**: descrição clara e concisa do propósito
- **Ferramentas**: selecione as ferramentas apropriadas do conjunto disponível

### 3. Seleção de Ferramentas
Liste as ferramentas do Antigravity disponíveis e pergunte ao usuário quais a persona deve acessar:

- **Operações de Arquivo**: leitura, escrita, edição, notebooks
- **Pesquisa e Navegação**: glob, grep, listagem de diretórios
- **Execução**: shell/bash, delegação de tarefas
- **Web**: web fetch, web search
- **Desenvolvimento**: gestão de tarefas/todos
- **Ferramentas MCP**: integrações específicas (Task Managers, etc.)

Apresente as ferramentas organizadas por categoria e peça ao usuário para selecionar as apropriadas. Por padrão, use acesso mínimo às ferramentas por segurança.

### 4. Projetar o Prompt do Sistema
Crie um prompt do sistema detalhado que:
- Define claramente o papel e expertise
- Fornece instruções passo a passo
- Inclui restrições e diretrizes
- Especifica requisitos de formato de saída
- Contém exemplos se úteis

### 5. Registrar a Persona
Adicione a definição da persona em `.agents/AGENTS.md` com:

```markdown
## @[nome-da-persona]

**Descrição**: [descrição clara do propósito]
**Ferramentas**: [lista das ferramentas selecionadas]

[Prompt do sistema detalhado com instruções claras]
```

### 6. Implementação
- Registre a persona em `.agents/AGENTS.md`
- Torne o prompt do sistema abrangente mas focado

### 7. Confirmar Criação
Após registrar a persona, confirme que a entrada foi criada com sucesso.

## Melhores Práticas
- Mantenha personas focadas em uma única responsabilidade
- Escreva prompts do sistema claros e acionáveis
- Limite o acesso às ferramentas ao que é necessário
- Inclua exemplos em prompts complexos
- Considere tratamento de erros e casos extremos
- Torne os formatos de saída explícitos

Agora, analise os requisitos e comece a criar a persona seguindo este processo.
