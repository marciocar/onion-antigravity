---
description: Prepara o contexto técnico e de engenharia — arquitetura, padrões de código, estrutura do projeto, comandos de desenvolvimento e frameworks. Use antes de trabalho técnico.
---

# 🔥 Warm-up de Engenharia

Preparação específica para trabalho técnico e de desenvolvimento.

## 🎯 Objetivo

Estabelecer contexto focado em:
- Arquitetura técnica do projeto
- Padrões de código e convenções
- Estrutura de código e organização
- Comandos e workflows de desenvolvimento
- Frameworks técnicos e ferramentas
- Sistema de testes e validação

## 📋 Checklist de Preparação

### 1. Contexto Geral (Base)
- ✅ Revisar `README.md` para visão geral do Sistema Onion
- ✅ Entender a estrutura de documentação em `docs/`

### 2. Meta Especificações Técnicas
- ✅ Revisar `docs/meta-specs/index.md`
- ✅ Focar em (quando disponíveis): `architecture.md`, `code-standards.md`, `agents.md`, `commands.md`
- ✅ Entender a hierarquia de especificações para decisões técnicas

### 3. Documentação Técnica
- ✅ Revisar `docs/onion/commands-guide.md` — seção "Comandos de Engenharia"
- ✅ Revisar `docs/onion/engineering-flows.md` — fluxos de desenvolvimento
- ✅ Revisar `docs/onion/testing-validation-system.md` — sistema de testes
- ✅ Mapear comandos de engenharia:
  - `/engineer-start` — iniciar desenvolvimento (valida story points)
  - `/engineer-work` — trabalhar em feature
  - `/engineer-plan` — planejar implementação
  - `/engineer-pre-pr` — preparar Pull Request
  - `/engineer-pr` — criar Pull Request
  - `/engineer-docs` — documentar código
  - `/engineer-warm-up` — este comando

### 4. Estrutura do Projeto
- ✅ Mapear a estrutura de diretórios do código
- ✅ Entender a organização de módulos/pacotes
- ✅ Identificar tecnologias principais (linguagens, frameworks)
- ✅ Localizar arquivos de configuração importantes

### 5. Padrões de Código
- ✅ Revisar convenções de nomenclatura
- ✅ Entender o estilo de código esperado
- ✅ Conhecer ferramentas de linting/formatting
- ✅ Verificar arquivos de configuração (.eslintrc, .prettierrc, etc.)

### 6. Knowledge Bases Técnicas
- ✅ `docs/knowledge-base/concepts/spec-as-code-strategy.md`
- ✅ `docs/knowledge-base/concepts/ai-agent-design-patterns.md`
- ✅ `docs/knowledge-base/concepts/abstraction-patterns-catalog.md`
- ✅ `docs/knowledge-base/frameworks/framework_testes.md`
- ✅ `docs/knowledge-base/concepts/context-window-optimization.md`

### 7. Personas de Desenvolvimento
Conhecer as personas/subagents especializados (ver `.agents/AGENTS.md`):
- `@react-developer` — desenvolvimento React
- `@nodejs-specialist` — backend Node.js/TypeScript
- `@postgres-specialist` — PostgreSQL
- `@nx-monorepo-specialist` — monorepos NX
- `@docker-specialist` — Docker e containers
- `@c4-architecture-specialist` — arquitetura C4
- `@mermaid-specialist` — diagramas Mermaid
- `@test-engineer` — testes e QA
- `@code-reviewer` — code review

### 8. Sistema de Testes
- ✅ Revisar `docs/onion/testing-validation-system.md`
- ✅ Entender comandos de teste:
  - `/test-unit` — testes unitários (White-box)
  - `/test-integration` — testes de integração (Grey-box)
  - `/test-e2e` — testes end-to-end (Black-box)
- ✅ Conhecer os frameworks de teste utilizados

### 9. Git e Versionamento
- ✅ Revisar comandos Git disponíveis:
  - `/git-feature-start` — criar branch de feature
  - `/git-sync` — sincronizar após merge
- ✅ Entender o workflow Git do projeto e as convenções de branching

### 10. Validação de Story Points
- ✅ Entender que `/engineer-start` valida estimativas
- ✅ Conhecer o processo de validação antes de iniciar desenvolvimento
- ✅ Saber como lidar com tasks sem estimativas

## 🔍 Contexto Específico de Engenharia

### Documentação Essencial
- `docs/onion/engineering-flows.md` — fluxos de desenvolvimento
- `docs/onion/testing-validation-system.md` — sistema de testes
- `docs/onion/commands-guide.md` — comandos de engenharia
- `docs/knowledge-base/frameworks/framework_testes.md` — framework de testes

### Workflows de Desenvolvimento
1. **Iniciar**: `/engineer-start` → valida story points, cria branch, sessão
2. **Trabalhar**: `/engineer-work` → loop de desenvolvimento
3. **Planejar**: `/engineer-plan` → planejar implementação detalhada
4. **Pre-PR**: `/engineer-pre-pr` → preparar Pull Request
5. **PR**: `/engineer-pr` → criar Pull Request (testes, build, PR)
6. **Sync**: `/git-sync` → sincronizar após merge

### Estrutura de Sessões
- ✅ Entender `docs/sessions/<feature>/` para o contexto de trabalho (complementar aos Artifacts do Antigravity)
- ✅ Conhecer o formato dos arquivos de sessão

## 💡 Quando Usar Este Warm-up
- ✅ Início de desenvolvimento de feature
- ✅ Retorno a trabalho técnico após período ausente
- ✅ Mudança de contexto técnico (nova tecnologia/framework)
- ✅ Necessidade de entender a arquitetura do projeto
- ✅ Preparação para code review ou refatoração

## 🔗 Integração com Produto
- Tasks vêm de `/product-task` com story points
- Especificações vêm de `/product-spec`
- Validação de estimativas antes de iniciar
- Sincronização com o Task Manager durante o desenvolvimento

## ⚠️ Notas
- Foco em contexto técnico e de código.
- Mantenha conhecimento de padrões e convenções no contexto.
- Use personas/subagents especializados para tecnologias específicas.
- Sempre valide story points antes de iniciar (`/engineer-start`).
- Mantenha o código sincronizado com o Task Manager durante o desenvolvimento.
