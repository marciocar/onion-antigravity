# 📚 Índice - Sistema Onion

> **Última atualização**: 2026-05-15 | **Gerado por**: `/docs-build-index onion` | **Revisado**: auditoria manual

Bem-vindo ao índice da documentação do **Sistema Onion**. Este documento organiza os 26 arquivos de documentação operacional do sistema em `docs/onion/`.

---

## 🎯 Visão Geral

O **Sistema Onion** é um framework instalável em `.agents/` para o **Google Antigravity**, com:
- 🤖 **78 workflows** em `.agents/workflows/` (flat + prefixo de categoria)
- 🎯 **Personas e subagents** em `.agents/AGENTS.md` (consolidados de 49 agentes especializados)
- 🧩 **2 skills** em `.agents/skills/` (`onion`, `onion-validation`) + **4 rules** always-on em `.agents/rules/`
- 📚 **Knowledge Bases estruturadas** para consumo por IA
- 🧅 **Skill + workflow `/onion`** — ponto de entrada inteligente
- 🔗 **Task Manager Abstraction** plugável (Jira, ClickUp, Asana, Linear)
- 🏗️ **Spec as Code Multi-Context** — business, technical e meta-specs

---

## 📊 Estatísticas

- **26 arquivos** em `docs/onion/`
- **78 workflows** em `.agents/workflows/`
- **Personas e subagents** em `.agents/AGENTS.md`
- **2 skills** (`onion`, `onion-validation`) + **4 rules** em `.agents/`

---

## 📖 Documentação Principal

### 🌟 Guias Essenciais

**Comece aqui se você é novo no Sistema Onion:**

1. **[Guia de Workflows](commands-guide.md)** - Documentação dos workflows disponíveis
   - 78 workflows (flat + prefixo de categoria)
   - Exemplos de uso e fluxos
   - Integrações com Task Managers

2. **[Referência de Personas](agents-reference.md)** - Lista e descrição das personas/subagents
   - Personas e subagents (consolidados de 49 agentes especializados)
   - Quando usar cada persona
   - Capacidades e especializações

3. **[Fluxos de Engenharia](engineering-flows.md)** - Workflows detalhados para desenvolvimento
   - Feature Development completo
   - Correção de bugs
   - Refatoração e hotfixes
   - Integração ClickUp por fluxo

4. **[Sistema de Testes e Validação](testing-validation-system.md)** - Framework completo de testes e validação
   - 4 camadas integradas (Knowledge Base, Agentes, Comandos de Teste, Comandos de Validação)
   - White-box, Grey-box, Black-box
   - QA Story Points

5. **[Sistema de Níveis](levels-system.md)** - Descoberta progressiva de comandos (v4.0) ✨ NOVO
   - Organização por níveis (starter, intermediate, advanced)
   - Redução de 90% no tempo de onboarding
   - Filosofia 80/15/5

### ⚙️ Integrações e Configuração

**Setup e configuração do sistema:**

1. **[Configuração Inicial](getting-started.md)** - Setup completo do sistema
   - Primeiros passos
   - Configuração de integrações
   - Workflows básicos

2. **[Instalação](INSTALLATION.md)** - Guia de instalação do Sistema Onion
   - Requisitos do sistema
   - Instalação passo a passo
   - Verificação de instalação

3. **Task Manager Abstraction** — configure provider (Jira/ClickUp/Asana/Linear) via `.env` e `/meta-setup-integration`. Adapters técnicos em [`docs/reference/task-manager/adapters/`](../reference/task-manager/adapters/).

### 🔧 Referências Técnicas

**Documentação técnica e arquitetural:**

1. **[Exemplos Práticos](practical-examples.md)** - Casos de uso reais com exemplos
   - Workflows completos
   - Cenários de uso
   - Melhores práticas

2. **[Referência de Ferramentas](tools-reference.md)** - Todas as ferramentas disponíveis
   - Ferramentas integradas
   - Configuração
   - Uso e exemplos

3. **[Arquitetura de Comandos](claude-code-commands-architecture.md)** - Estrutura interna dos comandos
   - Padrões de design
   - Estrutura de arquivos
   - Best practices

### 📚 Documentação Avançada

**Para usuários avançados e desenvolvedores do sistema:**

1. **[Testes de Validação E2E](end-to-end-validation-tests.md)** - Testes end-to-end do sistema
   - Suíte de testes completa
   - Validação de funcionalidades
   - Cobertura de testes

2. **[Guia de Engenharia Reversa](sistema-engenharia-reversa-guia-uso.md)** - Engenharia reversa de projetos
   - Metodologia completa
   - Templates adaptativos
   - Casos de uso

3. **[Phase Subtask Sync Fix](phase-subtask-sync-fix.md)** - Documentação técnica de correção
   - Problema identificado
   - Solução implementada
   - Validação

---

## 🚀 Release Notes e Anúncios

**Histórico de versões e novidades:**

### Release Notes v4.0

1. **[Release Notes v4.0 Beta](RELEASE-NOTES-v4.0-beta.md)** - Novidades da versão 4.0 ✨ NOVO
   - Arquitetura Multi-Context
   - Melhorias significativas em onboarding
   - Suporte multi-IDE

2. **[Release Notes v4.0 Fase 2](RELEASE-NOTES-v4.0-fase2.md)** - Migração de comandos e agentes
   - Estrutura v4.0 implementada
   - Migração completa
   - Novos recursos

3. **[Release Notes v4.0 Fase 3](RELEASE-NOTES-v4.0-fase3.md)** - Sistema de níveis
   - Descoberta progressiva
   - Organização por níveis
   - Redução de onboarding

### Anúncios e Pacotes

1. **[Anúncio v4.0 Beta.1](ANNOUNCEMENT-v4.0-beta.1-docs.md)** - Anúncio da versão beta
   - Novidades principais
   - Como começar
   - Próximos passos

2. **[Release Complete v4.0 Beta.1](RELEASE-COMPLETE-v4.0-beta.1-docs.md)** - Conclusão da release
   - Status completo
   - Funcionalidades entregues
   - Métricas

3. **[Release Package v4.0 Beta.1](RELEASE-PACKAGE-v4.0-beta.1.md)** - Pacote completo da release
   - Arquivos incluídos
   - Estrutura completa
   - Instruções de uso

---

## 🛠️ Documentação de Implementação

**Documentação técnica de implementação e milestones:**

### CLI (Command Line Interface)

1. **[CLI Implementation Complete](cli-implementation-complete.md)** - Implementação completa do CLI
   - Funcionalidades implementadas
   - Comandos disponíveis
   - Uso e exemplos

2. **[CLI Wizard Fix](cli-wizard-fix.md)** - Correção do wizard CLI
   - Problema identificado
   - Solução implementada
   - Validação

3. **[Implementation Plan CLI Wizard](implementation-plan-cli-wizard.md)** - Plano de implementação do wizard
   - Arquitetura proposta
   - Implementação passo a passo
   - Validação

### Milestones e Fixes

1. **[Milestone First Successful Init](MILESTONE-first-successful-init.md)** - Primeiro init bem-sucedido
   - Conquista importante
   - Detalhes técnicos
   - Próximos passos

2. **[Fix Multiple Starter Commands](fix-multiple-starter-commands.md)** - Correção de múltiplos comandos starter
   - Problema identificado
   - Solução implementada
   - Validação

---

## 🗺️ Navegação Rápida

### Por Tipo de Documento

| Tipo | Arquivos | Descrição |
|------|----------|-----------|
| **Guias** | 5 | Guias essenciais de uso |
| **Configuração** | 3 | Setup e instalação |
| **Referências** | 4 | Documentação técnica |
| **Avançado** | 3 | Para usuários avançados |
| **Release Notes** | 6 | Histórico de versões |
| **Implementação** | 4 | Documentação técnica |

### Por Perfil de Usuário

#### 👨‍💻 Desenvolvedores
- Comece com: [Getting Started](getting-started.md)
- Aprenda: [Commands Guide](commands-guide.md)
- Explore: [Engineering Flows](engineering-flows.md)
- Teste: [Testing Validation System](testing-validation-system.md)

#### 📋 Product Owners
- Comece com: [Commands Guide](commands-guide.md) - Seção Produto
- Aprenda: [Practical Examples](practical-examples.md)
- Explore: [Agents Reference](agents-reference.md)

#### 🧪 QA/Test Engineers
- Comece com: [Testing Validation System](testing-validation-system.md)
- Aprenda: [End-to-End Validation Tests](end-to-end-validation-tests.md)
- Explore: [Agents Reference](agents-reference.md) - Seção Testing

#### 🏗️ Arquitetos
- Comece com: [Arquitetura de Workflows do Antigravity](claude-code-commands-architecture.md)
- Explore: [Engineering Flows](engineering-flows.md)

#### 🔧 Administradores
- Comece com: [Installation](INSTALLATION.md)
- Aprenda: [Getting Started](getting-started.md)
- Configure Task Manager: `/meta-setup-integration` (adapters em `docs/reference/task-manager/adapters/`)

---

## 🔗 Links Rápidos

### Documentação Essencial
- [README Principal](../../README.md) - Visão geral do Sistema Onion v4.0 Beta
- [Índice Central](../../INDEX.md) - Hub de navegação completo
- [Guia de Comandos](commands-guide.md) - Todos os comandos
- [Referência de Agentes](agents-reference.md) - Todos os agentes

### Knowledge Bases Relacionadas
- [Task Manager Abstraction](../../knowledge-base/concepts/task-manager-abstraction.md)
- [Spec-Driven Development](../../knowledge-base/concepts/spec-driven-development.md)
- [AI Agent Design Patterns](../../knowledge-base/concepts/ai-agent-design-patterns.md)
- [Framework de Story Points](../../knowledge-base/frameworks/framework_story_points.md)

### Configuração
- [Installation](INSTALLATION.md)
- [Getting Started](getting-started.md)
- [Adapters de Task Manager](../reference/task-manager/adapters/)

---

## 📅 Histórico de Atualizações

| Data | Mudança |
|------|---------|
| 2026-05-15 | Auditoria manual: 94 comandos / 49 agentes / 1 skill `onion`; agente @onion corrigido (agentes fantasmas removidos, 18 novos adicionados) |
| 2026-05-15 | Índice revisado: estatísticas reais (91→94 comandos / 49 agentes / 26 arquivos), refs quebradas removidas |
| 2025-12-20 | Índice reconstruído |
| 2025-12-02 | Adicionado Spec-Driven Development |

---

## 🆕 Novidades Recentes

### ✨ Adicionado Recentemente

- **[Sistema de Níveis](levels-system.md)** (v4.0)
  - Descoberta progressiva de comandos
  - Organização por níveis (starter, intermediate, advanced)
  - Redução de 90% no tempo de onboarding

- **[Release Notes v4.0 Beta](RELEASE-NOTES-v4.0-beta.md)**
  - Arquitetura Multi-Context
  - Melhorias significativas em onboarding e organização
  - Suporte multi-IDE

- **[Installation Guide](INSTALLATION.md)**
  - Guia completo de instalação
  - Requisitos do sistema
  - Verificação de instalação

---

## 🔄 Manutenção

Este índice é gerado automaticamente pelo comando `/docs-build-index onion`.

**Para atualizar:**
```bash
/docs-build-index onion        # Reconstruir este índice
/docs-build-index              # Reconstruir índice principal
```

---

**Última atualização:** 2026-05-15
**Mantido por:** Sistema Onion

---

**Sistema Onion** - Multi-Context Development Orchestrator 🧅
