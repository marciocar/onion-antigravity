---
description: Desenvolvimento completo com Runflow SDK — cria projetos, agentes, workflows, RAG e integrações, delegando para a persona @runflow-specialist.
---

# Desenvolvimento Runflow

Workflow especializado para desenvolvimento com Runflow SDK usando a persona
`@runflow-specialist` (`.agents/AGENTS.md`). Facilita criação de projetos, agentes,
workflows, RAG e integrações, sempre orientando próximos passos e fechamento de
tarefas.

Entrada do usuário: o texto após o comando é a solicitação detalhada repassada à
persona.

```
<requirements>
[Entrada do usuário]
</requirements>
```

## Processo

### 1. Invocar a Persona Especialista

**SEMPRE** invoque `@runflow-specialist` para todas as operações Runflow. A persona
conhece a base em `docs/knowledge-base/platforms/runflow.md` e os padrões do projeto.

### 2. Operações Disponíveis

- **Criar novo projeto Runflow** — estrutura de diretórios, `package.json`,
  `tsconfig.json`, `main.ts` com agente básico, `.runflow/rf.json` e `README.md`.
- **Criar novo agente** — analisar requisitos, seguir padrões de `main.ts`,
  implementar tools customizadas, configurar memory/RAG, validar e documentar.
- **Conectar agentes (multi-agent)** — agentes especializados + supervisor com
  roteamento inteligente, comunicação entre agentes e exemplo de uso.
- **Configurar RAG** — verificar se a base existe na plataforma, configurar
  threshold/k/searchPrompt e integrar ao agente.
- **Criar workflow completo** — schemas Zod de entrada/saída, agentes por passo,
  conectores (HubSpot, Twilio, etc.), passos sequenciais/condicionais e exemplo.
- **Orientar próximos passos** — comandos de teste, melhorias, integrações e TODOs.
- **Finalizar desenvolvimento** — revisar código, validar com linter, verificar
  testes/docs e criar resumo executivo.

Em cada caso, formule a solicitação à persona de forma específica:
`@runflow-specialist <solicitação detalhada com nome, funcionalidades, tools, RAG,
memory e integrações>`.

### 3. Fluxos Recomendados

- **Novo projeto:** criar projeto → criar agente inicial → configurar RAG → orientar
  próximos passos → finalizar.
- **Adicionar feature:** criar agente/feature → integrar (multi-agent/workflow) →
  orientar → finalizar.

## Guidelines

**Boas práticas:** seja específico (nome, funcionalidades, tools); mencione
requisitos técnicos (RAG, memory, integrações); teste incrementalmente; valide antes
de continuar; siga os padrões de `main.ts`; peça próximos passos e finalize
explicitamente.

**Atenções:** verifique `.runflow/rf.json`/variáveis de ambiente; confirme a versão
do SDK no `package.json`; use `observability: 'minimal'`; a base de conhecimento RAG
deve existir antes de configurar; defina critérios de roteamento claros em
multi-agent; defina schemas e tratamento de erros em workflows.

**Evitar:** solicitações vagas; pular etapas (RAG antes de usar); ignorar validação;
acessar Prisma diretamente (use o SDK); `observability: 'full'`; conectores sem
credenciais; RAG sem base criada; conectar agentes sem roteamento.

## Troubleshooting

- **Base de conhecimento não encontrada:** crie a base na plataforma antes do RAG.
- **Erro no trace collector:** use `observability: 'minimal'`.
- **Agente não segue padrões:** mencione "seguir padrões de main.ts".
- **Roteamento multi-agent falha:** revise critérios no supervisor e teste com
  inputs variados.
- **Workflow falha em um passo:** revise schemas e tratamento de erros.

## Exemplos de Uso

```bash
/development-runflow-dev Criar projeto "assistente-juridico" com agente que analisa processos usando RAG na base "processos-juridicos"
/development-runflow-dev Criar sistema multi-agente com supervisor que roteia para Sales, Support e Billing, cada um com tools e RAG próprios
/development-runflow-dev Finalizar desenvolvimento do sistema de suporte completo e criar resumo
```

## Referências

- Persona: @runflow-specialist · KB: `docs/knowledge-base/platforms/runflow.md`
- Relacionados: `/meta-create-agent` · `/meta-create-knowledge-base`
