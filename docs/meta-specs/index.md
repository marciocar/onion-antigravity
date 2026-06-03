---
title: Meta Specs — Sistema Onion
date: 2026-06-03
version: 2.0.0
status: active
changelog: "v2.0.0 — migração de plataforma Claude Code → Google Antigravity; atualiza descrições das 5 meta-specs, paths (.claude/ → .agents/) e terminologia (comandos → workflows, agentes → personas)"
---

# Meta Specs - Sistema Onion

---

## 📋 Visão Geral

**Meta Specs** são especificações de nível mais alto que servem como "constituição" do Sistema Onion. Elas definem princípios, padrões e regras imutáveis que todos os componentes devem seguir.

### Hierarquia de Especificações

```
┌─────────────────────────────────────────────────────────┐
│                    META-SPECS (L0)                      │
│          "Constituição" - Regras Imutáveis              │
├─────────────────────────────────────────────────────────┤
│                    DOMAIN SPECS (L1)                    │
│          Regras de Negócio e Domínio                    │
├─────────────────────────────────────────────────────────┤
│                    FEATURE SPECS (L2)                   │
│          Especificações de Features                     │
├─────────────────────────────────────────────────────────┤
│                    TASK SPECS (L3)                      │
│       Artifacts do Antigravity / docs/sessions/         │
└─────────────────────────────────────────────────────────┘
```

---

## 📁 Estrutura

```
docs/meta-specs/
├── index.md              # Este arquivo
├── architecture.md       # Padrões arquiteturais
├── code-standards.md     # Padrões de código
├── agents.md             # Padrões para personas/subagents
├── commands.md           # Padrões para workflows
└── integrations.md       # Padrões para integrações
```

---

## 🎯 Propósito

### O que são Meta Specs?

Meta Specs definem:
- **Princípios arquiteturais** que o sistema deve seguir
- **Padrões de código** para consistência
- **Convenções de nomenclatura** para personas e workflows
- **Regras de integração** com sistemas externos
- **Critérios de qualidade** para validação

### Quando usar Meta Specs?

| Situação | Uso |
|----------|-----|
| Criar nova persona/subagent | Consultar `agents.md` |
| Criar novo workflow | Consultar `commands.md` |
| Tomar decisão arquitetural | Consultar `architecture.md` |
| Revisar código | Consultar `code-standards.md` |
| Integrar sistema externo | Consultar `integrations.md` |

### Quem mantém Meta Specs?

- **@metaspec-gate-keeper**: Valida conformidade (a constituição de validação)
- **`/meta-metaspec-validate`**: workflow que **aplica** a constituição executando as leituras e produzindo o veredito com evidência (ponto de entrada confiável)
- **@branch-metaspec-checker**: aplica o mesmo padrão ao diff do branch no pré-PR
- **@onion**: Orquestra aplicação
- **Administradores do projeto**: Atualizam specs

### Dualidade de contexto — L0 (framework) vs L1+ (projeto-alvo)

O gate-keeper opera em **dois modos**, escolhendo a régua conforme o artefato:

- **Modo Framework (L0)** — no `onion-antigravity`, valida artefatos `.agents/**`
  contra as **5 meta-specs L0** (agents/commands/architecture/code-standards/integrations).
- **Modo Projeto-alvo (L1+)** — quando o Onion está instalado num projeto, valida
  artefatos de **domínio/feature/ADR** contra as metaspecs **daquele projeto**.

Em ambos os modos as metaspecs são **descobertas dinamicamente** (`docs/meta-specs/`,
sem nomes cravados), para o mesmo gate-keeper funcionar em qualquer projeto-alvo.

---

## 📜 Meta Specs Disponíveis

> As 5 meta-specs foram criadas em 2026-05-18 (Plano de Saneamento Onion 2026-05, T2.1 a T2.5) e migradas para o Google Antigravity em 2026-06-03 (ver [ADR-001](../analysis/onion-antigravity-migration-adr-2026-06.md)).

### 🤖 [agents.md](./agents.md) — ATIVA (v2.0.0, 2026-06-03)
Padrões para personas/subagents:
- Personas declaradas de forma consolidada em `.agents/AGENTS.md` (não mais arquivos por categoria)
- Schema Role @handle · Goal · Traits · Constraint (sem frontmatter YAML por-agente)
- Expertise profunda lastreada em skills (`.agents/skills/`) e KBs (`docs/knowledge-base/`)
- Convenções de nomenclatura kebab-case + handle `@nome`
- Subagents do Antigravity e padrões de delegação/integração com MCPs

### 🔧 [commands.md](./commands.md) — ATIVA (v2.0.0, 2026-06-03)
Padrões para workflows:
- Workflows em `.agents/workflows/` (flat, prefixo de categoria), invocados `/<categoria>-<nome>`
- Frontmatter mínimo: apenas `description`
- **Workflows faseados como invariantes** (`engineer-plan→...→engineer-pr-update` e `product-collect→...→product-feature`)
- Política de duplicação de nomes resolvida pelo prefixo de categoria
- Limite de tamanho: workflow ≤ 400 linhas; hooks em `.agents/hooks.json`

### 🏗️ [architecture.md](./architecture.md) — ATIVA (v2.0.0, 2026-06-03)
Padrões arquiteturais:
- Estrutura obrigatória de `.agents/` e `docs/`
- Separação operacional vs documentação
- Princípio de framework instalável
- Dependências permitidas entre categorias (com diagrama)
- Plataforma única: Google Antigravity

### 📝 [code-standards.md](./code-standards.md) — ATIVA (v2.0.0, 2026-06-03)
Padrões de código e idioma:
- Separação pt-BR (docs/UX) vs inglês (código/commits/logs)
- Formatação Markdown
- Convenções de naming (filenames, slugs, branches, commits)
- Estilo de escrita
- Configuração e secrets

### 🔌 [integrations.md](./integrations.md) — ATIVA (v2.0.0, 2026-06-03)
Padrões para integrações:
- Task Manager Abstraction como referência canônica (docs em `docs/reference/task-manager/`)
- Estrutura obrigatória de adapter (factory + interface + types + detector + providers)
- Gestão de `.env` (obrigatórias vs opcionais, fallback gracioso)
- MCPs configurados em `~/.gemini/config/mcp_config.json` (`{"mcpServers":{...}}`, `serverUrl`)
- Formatação por provider (ADF Jira v3, Markdown ClickUp, Unicode comments, HTML Asana, Markdown Linear)

---

## 🔄 Workflow de Validação

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│    CHANGE       │────▶│  @metaspec-     │────▶│   APPROVED/     │
│    REQUEST      │     │  gate-keeper    │     │   REJECTED      │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

### Processo

1. **Proposta de mudança**: Desenvolvedor propõe alteração
2. **Validação**: `@metaspec-gate-keeper` verifica conformidade
3. **Decisão**: Aprovado se conforme, rejeitado com justificativa

---

## 📚 Referências

- **Knowledge Bases**: `docs/knowledge-base/`
- **Documentação Onion**: `docs/onion/`
- **Personas**: `.agents/AGENTS.md`
- **Workflows**: `.agents/workflows/`
- **Regras (always-on)**: `.agents/rules/`
- **Skills**: `.agents/skills/`

---

## 📅 Histórico

| Data | Versão | Mudança |
|------|--------|---------|
| 2025-11-24 | 1.0.0 | Criação inicial |
| 2026-06-03 | 2.0.0 | Migração de plataforma Claude Code → Google Antigravity (`.claude/` → `.agents/`, comandos → workflows, agentes → personas) |

---

**Responsável**: Sistema Onion
**Última Atualização**: 2026-06-03
