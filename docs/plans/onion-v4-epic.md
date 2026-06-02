---
status: historical
abandoned-on: 2026-05-18
replaced-by: docs/plans/onion-saneamento-plan-2026-05.md
---

# 🧅 ÉPICO: Migração Sistema Onion v3 → v4

> **AVISO**: Este plano foi formalmente abandonado em 2026-05-18. As FASES 5-9 (CLI standalone, multi-IDE, aprendizado contínuo, Story Points como plugin, polimento GA) não serão executadas. Mantido como registro histórico. Plano vigente: [Saneamento Onion 2026-05](onion-saneamento-plan-2026-05.md).

> **Status**: Abandonado | **Criado**: 2025-12-20 | **ID**: ONION-V4-EPIC-001

---

## 📋 Informações do Épico

| Campo | Valor |
|-------|-------|
| **Nome** | Migração Sistema Onion v3 → v4: Multi-Context Development Orchestrator |
| **Tipo** | Epic / Refactoring |
| **Prioridade** | Alta |
| **Complexidade** | Alta |
| **Story Points** | 55.5 SP (Épico) |
| **Estimativa** | 4-6 semanas |
| **Assignee** | Onion Team |
| **Status** | 🚧 Em Progresso (FASE 1-4 concluídas, 71%) |

---

## 🎯 Objetivo

Transformar o Sistema Onion de "ferramenta SDD com 90 comandos" em **metodologia completa de desenvolvimento multi-contexto**, criando categoria única no mercado sem concorrentes diretos.

---

## 📝 Descrição Completa

### Transformação Core

O Onion v4 reimagina o sistema como **Multi-Context Development Orchestrator** com:

1. ✅ **CLI Standalone** (`@onion/cli`) - CONCLUÍDO
2. 🚧 **Arquitetura `.onion/`** agnóstica de IDE
3. 🚧 **6 Contextos** independentes (business, technical, CS, compliance, sales, marketing)
4. 🚧 **Sistema de Níveis** (starter → intermediate → advanced)
5. 🚧 **IDE Loaders** (Claude Code, Windsurf, Claude Code)
6. 🚧 **Ciclo Completo** de 5 camadas (KB → Agentes → Comandos → Regras → Sessions)
7. 🚧 **Descoberta Progressiva** com help inteligente

### Diferencial Único

- **Onboarding**: 4-8 horas → 15-30 minutos (90% redução)
- **Foco**: 90 comandos → 5 starter (80% dos casos)
- **Portabilidade**: Claude Code-only → 3+ IDEs
- **Organização**: Contextos misturados → Separação clara
- **Categoria**: Única no mercado (Multi-Context Orchestrator)

---

## 🏗️ Arquitetura Técnica

### Estrutura Proposta

```
.onion/                              # ✅ Universal (agnóstica de IDE)
├── core/                            # Núcleo universal (5-10 comandos)
│   ├── knowledge-base/                    # CAMADA 1: Knowledge Base
│   ├── agents/                      # CAMADA 2: Agentes IA
│   ├── commands/                    # CAMADA 3: Comandos
│   ├── rules/                       # CAMADA 4: Regras
│   └── utils/                       # Abstractions pluggáveis
│
├── contexts/                        # 6 contextos especializados
│   ├── business/
│   │   ├── commands/starter/        # 5 comandos (80% casos)
│   │   ├── commands/intermediate/   # 10 comandos (15% casos)
│   │   ├── commands/advanced/       # Todos (5% casos)
│   │   ├── agents/                  # Agentes especializados
│   │   ├── knowledge-base/                # KBs específicas
│   │   └── sessions/                # CAMADA 5: Rastreabilidade
│   ├── technical/
│   ├── customer-success/
│   ├── compliance/
│   ├── sales/
│   └── marketing/
│
├── ide/                             # Loaders por IDE
│   ├── claude/onion-loader.js
│   ├── windsurf/onion-bridge.ts
│   └── claude/onion-adapter.py
│
└── .onion-config.yml                # Configuração central
```

---

## 📊 Decomposição em Fases (Subtasks)

### ✅ FASE 1: CLI e Estrutura Base - **CONCLUÍDO** ✅
**Story Points**: 8 SP | **Status**: ✅ Concluído | **Duração**: 1 semana

**Entregáveis**:
- [x] CLI `onion init` funcional com wizard interativo
- [x] Geração de estrutura `.onion/` completa
- [x] Criação de `.onion-config.yml`
- [x] Geração de comandos starter por contexto
- [x] Agente `@onion` core
- [x] `.claude/` para compatibilidade
- [x] Documentação em `docs/onion/MILESTONE-first-successful-init.md`

**Arquivo da Task**: [`tasks/fase-01-cli-structure.md`](tasks/fase-01-cli-structure.md) ✅

---

### ✅ FASE 2: Migração de Comandos e Agentes - **CONCLUÍDO** ✅
**Story Points**: 21 SP | **Status**: ✅ Concluído | **Duração**: 2 horas (scripts automatizados)

**Objetivo**: Migrar 90 comandos e 46 agentes de `.claude/` para `.onion/contexts/`

**Entregáveis**:
- [x] Migrar comandos business context (20 comandos)
- [x] Migrar comandos technical context (32 comandos)
- [x] Migrar 12 agentes business + 23 agentes technical
- [x] Classificar todos por nível (starter/intermediate/advanced)
- [x] Criar `.context-config.yml` para cada contexto
- [x] Atualizar headers YAML com novos campos (context, level)
- [x] Criar 91 symlinks para backward compatibility
- [x] Testar comandos migrados no Claude Code

**Arquivo da Task**: [`tasks/fase-02-migration-commands-agents.md`](tasks/fase-02-migration-commands-agents.md) ✅

**Destaques**:
- ✅ 91/91 arquivos migrados (52 comandos + 35 agentes)
- ✅ Scripts Node.js automatizados para migração
- ✅ 100% backward compatibility via symlinks
- ✅ Validação automatizada com script dedicado
- ✅ Documentação: `.claude/sessions/fase-02-migration-commands/SUMMARY.md`

---

### ✅ FASE 3: Sistema de Níveis e Help Inteligente - **CONCLUÍDO** ✅
**Story Points**: 8 SP | **Status**: ✅ Concluído | **Duração**: 4 horas

**Objetivo**: Implementar sistema de níveis e descoberta progressiva

**Entregáveis**:
- [x] Estrutura starter/intermediate/advanced em cada contexto
- [x] Comando `/help` global inteligente
- [x] Comando `/business/help` com estatísticas
- [x] Comando `/technical/help` com estatísticas
- [x] Descoberta progressiva em 13 comandos starter
- [x] Pré-requisitos e próximos passos em 15 comandos inter/adv
- [x] Documentação completa em `docs/onion/levels-system.md` (400+ linhas)

**Arquivo da Task**: [`tasks/fase-03-levels-help.md`](tasks/fase-03-levels-help.md) ✅

**Destaques**:
- ✅ 3 comandos help (global, business, technical)
- ✅ 28 comandos com navegação progressiva
- ✅ Scripts automatizados para adicionar seções
- ✅ 91 symlinks para backward compatibility
- ✅ Documentação: `.claude/sessions/fase-03-level-system/SUMMARY.md`

---

### ✅ FASE 4: CLI Enhancements - **CONCLUÍDO** ✅
**Story Points**: 2.5 SP | **Status**: ✅ Concluído | **Duração**: 13.5 horas

**Objetivo**: Melhorar CLI com comandos `add` e `migrate`, criando módulos reutilizáveis

**Entregáveis**:
- [x] Core modules: validator, detector, config (45+ testes)
- [x] Generator refatorado para métodos atômicos (15 funções)
- [x] Comando `onion add` (adicionar contextos/IDEs)
- [x] Migrator modules: detector, mapper, transformer
- [x] Comando `onion migrate` (migração v3→v4 completa)
- [x] Arquitetura de baixo acoplamento validada

**Arquivo da Task**: `.claude/sessions/fase-04-cli-enhancements/` ✅

**Destaques**:
- ✅ 16 arquivos criados (~3700 LOC)
- ✅ 71+ funções implementadas
- ✅ 45+ testes unitários
- ✅ ZERO acoplamento desnecessário
- ✅ Máxima reutilização alcançada
- ✅ Documentação: `.claude/sessions/fase-04-cli-enhancements/SUMMARY.md`

**Nota**: Esta fase foi adicionada durante desenvolvimento para melhorar qualidade do CLI.

---

### 🚧 FASE 5: IDE Loaders e Compatibilidade
**Story Points**: 8 SP | **Status**: 📋 Próxima | **Duração**: 1 semana

**Objetivo**: Implementar loaders para Claude Code, Windsurf, Claude

**Entregáveis**:
- [ ] Claude Code Loader funcional (`.onion/ide/claude-code/onion-loader.js`)
- [ ] Testar descoberta de comandos no Claude Code
- [ ] Windsurf Loader (`.onion/ide/windsurf/onion-bridge.ts`)
- [ ] Claude Code Loader (`.onion/ide/claude/onion-adapter.py`)
- [ ] Testes de integração em todos os IDEs
- [ ] Documentação em `docs/onion/ide-integration-complete.md`

**Arquivo da Task**: [`tasks/fase-05-ide-loaders.md`](tasks/fase-05-ide-loaders.md) 📋

---

### 🚧 FASE 6: Abstractions e Extensibilidade
**Story Points**: 5 SP | **Status**: 📋 Planejada | **Duração**: 1 semana

**Objetivo**: Implementar abstractions pluggáveis

**Entregáveis**:
- [ ] `/meta/create-abstraction` completo
- [ ] Transcription Service abstraction
- [ ] Notification Manager abstraction
- [ ] Storage Manager abstraction
- [ ] Audit Logger abstraction
- [ ] Documentação SDAAL em `docs/onion/sdaal-abstractions.md`

**Arquivo da Task**: [`tasks/fase-05-abstractions.md`](tasks/fase-05-abstractions.md) 📋

---

### 🚧 FASE 7: Gestão de Contexto e Learnings
**Story Points**: 5 SP | **Status**: 📋 Planejada | **Duração**: 1 semana

**Objetivo**: Implementar ciclo fechado de aprendizado

**Entregáveis**:
- [ ] `.onion/contexts/{context}/sessions/` para rastreabilidade
- [ ] `.onion/core/knowledge-base/learnings/` para aprendizado contínuo
- [ ] `/meta/extract-learning` para criar KBs automáticas
- [ ] Ciclo fechado (sessions → learnings → KBs)
- [ ] Documentação em `docs/onion/complete-cycle.md`

**Arquivo da Task**: [`tasks/fase-06-context-management.md`](tasks/fase-06-context-management.md) 📋

---

### 🚧 FASE 8: Story Points Cross-Context
**Story Points**: 3 SP | **Status**: 📋 Planejada | **Duração**: 3 dias

**Objetivo**: Implementar ciclo cross-context de estimativas

**Entregáveis**:
- [ ] Ciclo: business estima → technical valida → reavalia → afera
- [ ] Histórico de estimativas em banco de dados
- [ ] Machine learning para sugestões de SP
- [ ] Dashboard em `docs/onion/story-points-dashboard.md`
- [ ] Documentação do ciclo em `docs/onion/story-points-cycle.md`

**Arquivo da Task**: [`tasks/fase-07-story-points-cycle.md`](tasks/fase-07-story-points-cycle.md) 📋

---

### 🚧 FASE 9: Documentação e Onboarding
**Story Points**: 3 SP | **Status**: 📋 Planejada | **Duração**: 4 dias

**Objetivo**: Reescrever documentação completa para v4

**Entregáveis**:
- [ ] Reescrever `README.md` com visão Multi-Context
- [ ] `docs/onion/multi-context-architecture.md`
- [ ] `docs/onion/getting-started-by-context.md` (15-30 min)
- [ ] Vídeos/demos por contexto
- [ ] Templates e exemplos reais
- [ ] `CHANGELOG.md` com Onion v4.0.0

**Arquivo da Task**: [`tasks/fase-08-documentation.md`](tasks/fase-08-documentation.md) 📋

---

## 📊 Métricas de Sucesso

| Métrica | v3 Atual | v4 Meta | Status |
|---------|----------|---------|--------|
| **Onboarding Time** | 4-8 horas | 15-30 min/contexto | 🎯 |
| **Comandos Conhecidos** | 90 (confuso) | 5 starter + descoberta | 🎯 |
| **IDE Support** | Claude Code only | Claude Code + Windsurf + Claude | 🎯 |
| **Separação Contextos** | ❌ Misturado | ✅ Clara | 🎯 |
| **Portabilidade** | `.claude/` apenas | `.onion/` universal | 🎯 |
| **Categoria** | SDD tool | Multi-Context Orchestrator | 🎯 |

---

## 🚨 Riscos e Mitigações

### Risco 1: Onboarding Ainda Complexo
**Mitigação**: Help inteligente + descoberta progressiva + vídeos/demos

### Risco 2: IDEs Não Suportam Loaders
**Mitigação**: Fallback `AGENTS.md` universal + RFC Onion Format

### Risco 3: Breaking Changes v3 → v4
**Mitigação**: Symlinks `.claude/` → `.onion/` + script `onion migrate`

### Risco 4: Manutenção de 6 Contextos
**Mitigação**: Meta-comandos + validações automáticas + CI/CD

---

## 📅 Timeline

```
✅ Semana 0     → FASE 1: CLI (CONCLUÍDO)
🚧 Semana 1-2   → FASE 2: Migração de comandos
📋 Semana 3     → FASE 3: Sistema de níveis  
📋 Semana 4     → FASE 4: IDE Loaders
📋 Semana 5     → FASE 5-6: Abstractions + Learnings
📋 Semana 6     → FASE 7-8: Story Points + Documentação
🎉 Release      → Onion v4.0.0
```

**Duração Total**: 6 semanas (4-6 semanas estimadas)

---

## 🔗 Documentos Relacionados

### Visão e Estratégia
- [Plano de Migração Completo](onion-v4-migration-plan.md)
- [Visão Multi-Context Orchestrator](../knowledge-base/frameworks/onion-multi-context-orchestrator-vision.md)
- [Entendimento do Ciclo Completo](../knowledge-base/frameworks/onion-complete-cycle-understanding.md)
- [Estratégia de Integração IDE](../knowledge-base/frameworks/onion-ide-integration-strategy.md)

### Implementação
- [CLI Implementation Complete](../onion/cli-implementation-complete.md)
- [Milestone First Init](../onion/MILESTONE-first-successful-init.md)
- [Spec-Driven Development](../knowledge-base/concepts/spec-driven-development.md)

---

## 📈 Progresso Geral

```
Total Story Points: 55.5 SP

✅ Concluído: 39.5 SP (FASE 1-4)   [███████░░░] 71%
🚧 Em Progresso: 0 SP               [░░░░░░░░░░] 0%
📋 Planejado: 16 SP (FASE 5-9)      [░░░░░░░░░░] 0%
```

### Breakdown de Progresso

| Fase | Story Points | Status | Data |
|------|--------------|--------|------|
| FASE 1: CLI Base | 8.0 SP | ✅ | 2025-12-20 |
| FASE 2: Migração | 21.0 SP | ✅ | 2025-12-20 |
| FASE 3: Níveis | 8.0 SP | ✅ | 2025-12-20 |
| FASE 4: CLI Enhancements | 2.5 SP | ✅ | 2025-12-20 |
| **Subtotal** | **39.5 SP** | **✅ 71%** | - |
| FASE 5: IDE Loaders | 8.0 SP | 📋 | - |
| FASE 6-9: Restante | 8.0 SP | 📋 | - |
| **Total** | **55.5 SP** | - | - |
```

---

## 💬 Comentários e Atualizações

### 2025-12-20 - FASE 1-4 Concluídas (71% do Epic)
**Status**: ✅ 4 FASES concluídas em 1 dia  
**Próximo**: FASE 5 (IDE Loaders)

**Destaques Gerais**:
- ✅ 39.5 SP completados (71% do epic)
- ✅ 4 fases implementadas e testadas
- ✅ Arquitetura v4 100% funcional
- ✅ CLI robusto com 3 comandos principais
- ✅ 91 arquivos migrados automaticamente
- ✅ Sistema de níveis com descoberta progressiva

**Recursos gerados**:
- `packages/onion-cli/` - CLI completo: init, add, migrate
- `.onion/contexts/` - Estrutura multi-contexto
- `docs/onion/levels-system.md` - Documentação do sistema de níveis
- 3 SUMMARY.md detalhados (fases 2, 3, 4)

**Aprendizados**:
- Scripts Node.js automatizam migração perfeitamente
- Baixo acoplamento permite máxima reutilização
- Sistema de níveis reduz drasticamente onboarding
- ES6 modules + métodos atômicos = arquitetura limpa

---

### 2025-12-20 - Épico Criado
**Status**: ✅ FASE 1 concluída  
**Próximo**: Iniciar FASE 2 (Migração de Comandos)

**Destaques FASE 1**:
- ✅ CLI wizard funcional em 100%
- ✅ Estrutura `.onion/` gerada automaticamente
- ✅ Comandos starter criados para cada contexto
- ✅ Compatibilidade com Claude Code garantida
- ✅ Documentação completa do milestone

**Recursos gerados**:
- `packages/onion-cli/` - CLI completo com 5 comandos
- `docs/onion/MILESTONE-first-successful-init.md` - Marco histórico
- `docs/onion/cli-implementation-complete.md` - Documentação técnica

**Aprendizados**:
- Wizard interativo funciona perfeitamente com `inquirer` v9+
- Necessário criar `.claude/` na raiz para Claude Code reconhecer comandos
- Template system com `handlebars` facilita geração de arquivos

---

**Última atualização**: 2025-12-20  
**Versão**: 2.0.0  
**Criado por**: Sistema Onion  
**Status**: 🚧 Em Progresso (71% concluído - 4/9 fases completas)

