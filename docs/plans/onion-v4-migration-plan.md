---
status: historical
abandoned-on: 2026-05-18
replaced-by: docs/plans/onion-saneamento-plan-2026-05.md
---

# 🧅 Onion v4 Migration Plan

> **AVISO**: Este plano foi formalmente abandonado em 2026-05-18. Propunha estrutura `.onion/`, CLI standalone e suporte multi-IDE — todos descartados. Mantido como registro histórico. Plano vigente: [Saneamento Onion 2026-05](onion-saneamento-plan-2026-05.md).

> **Status**: Abandonado | **Data**: 2025-12-20 | **Categoria**: Plano de Migração

---

## 📋 PLANO DE TASK PROPOSTO

### **📋 Task Principal**
**Nome**: Migração do Sistema Onion v3 → v4: Multi-Context Development Orchestrator
**Tipo**: Epic / Refactoring
**Complexidade**: Alta
**Estimativa**: 4-6 semanas (épico)

---

## 🎯 Descrição Funcional

Transformar o Sistema Onion de uma "ferramenta SDD com muitos comandos" em uma **metodologia completa de desenvolvimento multi-contexto**, implementando:

1. **Reestruturação `.claude/` → `.onion/`** (agnóstica de IDE)
2. **Separação clara de contextos** (business, technical, customer-success, compliance, sales, marketing)
3. **Sistema de níveis** (starter/intermediate/advanced) para reduzir onboarding
4. **CLI standalone** (`@onion/cli`) com wizard interativo
5. **Ciclo completo de 5 camadas** (KB → Agentes → Comandos → Regras → Gestão de Contexto)
6. **IDE Loaders** (Claude Code, Windsurf, Claude Code) para descoberta de recursos
7. **Descoberta progressiva** de comandos com help inteligente

---

## 🏗️ Arquitetura Técnica

### Nova Estrutura Proposta

```
.onion/                              # ✅ Agnóstico de IDE
├── core/                            # Núcleo universal
│   ├── knowledge-base/                    # 📚 CAMADA 1: Knowledge Base
│   │   ├── concepts/                # Conceitos fundamentais
│   │   ├── frameworks/              # Análises de frameworks
│   │   ├── tools/                   # Documentação de tools
│   │   └── learnings/               # 🆕 Learnings de sessions
│   │
│   ├── agents/                      # 🤖 CAMADA 2: Agentes Core
│   │   ├── onion.md                 # Orquestrador master
│   │   ├── kb-manager.md            # 🆕 Gerencia KBs
│   │   └── context-router.md        # Roteador de contextos
│   │
│   ├── commands/                    # ⚡ CAMADA 3: Comandos Core
│   │   ├── meta/                    # 🔧 Meta-comandos (autogeração)
│   │   │   ├── create-knowledge-base.md
│   │   │   ├── create-agent.md
│   │   │   ├── create-command.md
│   │   │   ├── create-abstraction.md
│   │   │   └── create-rule.md
│   │   ├── warm-up.md               # Preparar contexto
│   │   ├── sync.md                  # Sincronização
│   │   └── help.md                  # Ajuda universal
│   │
│   ├── rules/                       # ✅ CAMADA 4: Regras
│   │   ├── onion-patterns.mdc       # Padrões de nomenclatura
│   │   ├── validation-rules.mdc     # Validações automáticas
│   │   └── context-validation.mdc   # 🆕 Validação por contexto
│   │
│   └── utils/                       # Utilities e abstractions
│       ├── task-manager/            # TMA (abstraction reference)
│       ├── transcription-service/   # 🆕 Transcrição
│       ├── notification-manager/    # 🆕 Notificações
│       └── storage-manager/         # 🆕 Storage
│
├── contexts/                        # Contextos específicos
│   ├── business/
│   │   ├── .context-config.yml      # Configuração do contexto
│   │   ├── knowledge-base/                # KBs específicas business
│   │   ├── agents/                  # Agentes business
│   │   ├── commands/
│   │   │   ├── starter/             # 5 comandos essenciais
│   │   │   ├── intermediate/        # 10 comandos adicionais
│   │   │   └── advanced/            # Todos os comandos
│   │   └── sessions/                # 🆕 CAMADA 5: Gestão de Contexto
│   │
│   ├── technical/
│   │   ├── .context-config.yml
│   │   ├── knowledge-base/
│   │   ├── agents/
│   │   ├── commands/
│   │   │   ├── starter/
│   │   │   ├── intermediate/
│   │   │   └── advanced/
│   │   └── sessions/
│   │
│   ├── customer-success/
│   ├── compliance/
│   ├── sales/
│   └── marketing/
│
├── docs/                            # Documentação estruturada
│   ├── onion/                       # Docs do framework Onion
│   ├── business-context/            # Docs gerados por business
│   ├── technical-context/           # Docs gerados por technical
│   ├── customer-success-context/
│   └── compliance-context/
│
├── ide/                             # 🆕 IDE-specific configs
│   ├── claude/
│   │   ├── onion-loader.js
│   │   └── settings.json
│   ├── windsurf/
│   │   ├── onion-bridge.ts
│   │   └── settings.yml
│   └── claude/
│       ├── onion-adapter.py
│       └── config.json
│
└── .onion-config.yml                # 🆕 Config central

.claude/                             # Mantido para compatibilidade
├── commands/ → symlink .onion/     
├── agents/ → symlink .onion/
└── rules/ → symlink .onion/
```

---

## 🔧 Componentes Principais

### 1. **Onion CLI** (`packages/onion-cli/`)
CLI standalone com:
- **Wizard interativo** (`onion init`)
- **Geração de estrutura** multi-contexto
- **Geradores** (contexts, IDEs, integrations)
- **Validadores** (estrutura, config, consistência)
- **Migrador** (`.claude/` → `.onion/`)

**Status atual**: ✅ **IMPLEMENTADO** (FASE 1 concluída no milestone anterior)

### 2. **IDE Loaders**
Adapters para descoberta de recursos:
- **Claude Code**: `.onion/ide/claude-code/onion-loader.js`
- **Windsurf**: `.onion/ide/windsurf/onion-bridge.ts`
- **Claude Code**: `.onion/ide/claude/onion-adapter.py`

### 3. **Sistema de Níveis**
Separação de comandos por complexidade:
- **Starter** (5 comandos) → 80% dos casos → 15-30 min onboarding
- **Intermediate** (10-15 comandos) → 15% dos casos
- **Advanced** (todos) → 5% dos casos

### 4. **Help Inteligente**
Comando `/help` com:
- Estatísticas de uso pessoal
- Sugestões de próximos comandos
- Filtro por nível (starter/intermediate/advanced)
- Descoberta progressiva

### 5. **Abstractions Pluggáveis**
Sistema `/meta/create-abstraction` para criar:
- **Transcription Service** (whisper-local, assemblyai, deepgram, none)
- **Notification Manager** (slack, discord, email, none)
- **Storage Manager** (s3, gcs, local, none)
- **Audit Logger** (cloudwatch, splunk, local, none)

---

## 📚 Bibliotecas/Dependências

### CLI (packages/onion-cli/)
- ✅ **chalk** `^4.1.2` - Terminal styling
- ✅ **inquirer** `^9.2.12` - Interactive prompts
- ✅ **commander** `^11.1.0` - CLI framework
- ✅ **fs-extra** `^11.2.0` - File system operations
- ✅ **yaml** `^2.3.4` - YAML parsing
- ✅ **ora** `^5.4.1` - Terminal spinners
- ✅ **execa** `^5.1.1` - Execute commands
- ✅ **glob** `^10.3.10` - File pattern matching
- ✅ **handlebars** `^4.7.8` - Template engine

**Status**: ✅ Já instaladas e testadas

### IDE Loaders
- **Claude Code**: JavaScript (Node.js) - sem deps externas
- **Windsurf**: TypeScript (`typescript`, `ts-node`)
- **Claude**: Python (`pyyaml`, `pathlib`)

---

## ✅ Critérios de Aceitação

### 🎯 FASE 1: CLI e Estrutura Base ✅ **CONCLUÍDO**
- [x] CLI `onion init` funcional com wizard interativo
- [x] Geração de estrutura `.onion/` completa
- [x] Criação de `.onion-config.yml`
- [x] Geração de comandos starter por contexto
- [x] Geração de agente `@onion` core
- [x] Criação de `.claude/` para compatibilidade Claude Code
- [x] Documentação completa do CLI em `docs/onion/`

**Status**: ✅ **100% implementado** conforme milestone `docs/onion/MILESTONE-first-successful-init.md`

---

### 🎯 FASE 2: Migração de Comandos e Agentes 🚧 **PRÓXIMA**
- [ ] Migrar comandos de `.claude/commands/` → `.onion/contexts/{context}/commands/`
- [ ] Classificar comandos por nível (starter/intermediate/advanced)
- [ ] Migrar agentes de `.claude/agents/` → `.onion/contexts/{context}/agents/`
- [ ] Criar `.context-config.yml` para cada contexto
- [ ] Atualizar headers YAML de comandos e agentes com novos campos
- [ ] Testar todos os comandos migrados no Claude Code

**Comandos a migrar**:
```
Business Context (15-20 comandos):
├── Starter (5): spec, refine, estimate, task, help
├── Intermediate (10): collect, light-arch, extract-meeting, consolidate-meetings, convert-to-tasks, whisper, ...
└── Advanced: create-*, meta-comandos

Technical Context (20-25 comandos):
├── Starter (5): plan, work, pr, sync, help
├── Intermediate (10): validate-estimate, review, test, deploy, docs, ...
└── Advanced: create-*, meta-comandos
```

---

### 🎯 FASE 3: Sistema de Níveis e Help Inteligente 📋 **PLANEJADA**
- [ ] Implementar estrutura starter/intermediate/advanced em cada contexto
- [ ] Criar comando `/help` inteligente com estatísticas de uso
- [ ] Implementar descoberta progressiva (comandos sugerem outros)
- [ ] Criar `/business/help --level=intermediate`
- [ ] Criar `/technical/help --level=advanced`
- [ ] Documentar sistema de níveis em `docs/onion/levels-system.md`

---

### 🎯 FASE 4: IDE Loaders e Compatibilidade 🔌 **PLANEJADA**
- [ ] Implementar Claude Code Loader funcional (`.onion/ide/claude-code/onion-loader.js`)
- [ ] Testar descoberta de comandos pelo Claude Code
- [ ] Implementar Windsurf Loader (`.onion/ide/windsurf/onion-bridge.ts`)
- [ ] Implementar Claude Code Loader (`.onion/ide/claude/onion-adapter.py`)
- [ ] Testar loaders em todos os IDEs suportados
- [ ] Documentar integração IDE em `docs/onion/ide-integration-complete.md`

---

### 🎯 FASE 5: Abstractions e Extensibilidade 🔧 **PLANEJADA**
- [ ] Implementar `/meta/create-abstraction` completo
- [ ] Criar Transcription Service abstraction
- [ ] Criar Notification Manager abstraction
- [ ] Criar Storage Manager abstraction
- [ ] Criar Audit Logger abstraction
- [ ] Documentar padrão SDAAL para abstractions em `docs/onion/sdaal-abstractions.md`

---

### 🎯 FASE 6: Gestão de Contexto e Learnings 📋 **PLANEJADA**
- [ ] Implementar `.onion/contexts/{context}/sessions/` para rastreabilidade
- [ ] Criar `.onion/core/knowledge-base/learnings/` para aprendizado contínuo
- [ ] Implementar `/meta/extract-learning` para criar KBs automáticas
- [ ] Implementar ciclo fechado (sessions → learnings → KBs)
- [ ] Documentar ciclo completo em `docs/onion/complete-cycle.md`

---

### 🎯 FASE 7: Story Points Cross-Context 📊 **PLANEJADA**
- [ ] Implementar ciclo: business estima → technical valida → business reavalia → technical afera
- [ ] Criar histórico de estimativas em banco de dados
- [ ] Implementar machine learning para sugestões de SP
- [ ] Criar dashboard de estimativas em `docs/onion/story-points-dashboard.md`
- [ ] Documentar ciclo cross-context em `docs/onion/story-points-cycle.md`

---

### 🎯 FASE 8: Documentação e Onboarding 📚 **PLANEJADA**
- [ ] Reescrever `README.md` com visão Multi-Context Orchestrator
- [ ] Criar `docs/onion/multi-context-architecture.md`
- [ ] Criar `docs/onion/getting-started-by-context.md` (15-30 min/contexto)
- [ ] Criar vídeos/demos por contexto
- [ ] Criar templates e exemplos reais
- [ ] Atualizar `CHANGELOG.md` com Onion v4.0.0

---

## 🧪 Pontos de Atenção para Teste

### Testes Essenciais
1. **CLI Wizard**: Testar setup completo em projeto limpo
2. **IDE Loaders**: Testar descoberta de comandos em Claude Code, Windsurf, Claude
3. **Sistema de Níveis**: Validar separação starter/intermediate/advanced
4. **Help Inteligente**: Testar estatísticas e sugestões de comandos
5. **Migração**: Testar migração de projeto Onion v3 → v4
6. **Abstractions**: Testar criação de nova abstraction via `/meta/create-abstraction`
7. **Ciclo Completo**: Testar KB → Agentes → Comandos → Regras → Sessions → Learnings

### Testes de Integração
1. **Multi-IDE**: Mesmo projeto funciona em Claude Code + Windsurf + Claude
2. **Multi-Context**: Business + Technical contextos funcionam independentemente
3. **Task Manager**: Abstraction funciona com ClickUp, Asana, Linear
4. **Story Points**: Ciclo cross-context funciona corretamente

### Testes de Performance
1. **Onboarding**: Tempo real para aprender 5 comandos starter (meta: 15-30 min)
2. **Discovery**: Tempo para descobrir comando intermediate (meta: < 5 min)
3. **CLI**: Tempo de execução de `onion init` (meta: < 30 segundos)

---

## 📊 Métricas de Sucesso

| Métrica | Valor Atual (v3) | Meta (v4) | Status |
|---------|------------------|-----------|--------|
| **Onboarding Time** | 4-8 horas | 15-30 min (por contexto) | 🎯 |
| **Comandos Conhecidos** | 90 (confuso) | 5 starter + descoberta progressiva | 🎯 |
| **IDE Support** | Claude Code only | Claude Code + Windsurf + Claude | 🎯 |
| **Complexidade Percebida** | "Muito alta" | "Adequada para metodologia completa" | 🎯 |
| **Separação de Contextos** | ❌ Misturado | ✅ Clara e explícita | 🎯 |
| **Portabilidade** | `.claude/` apenas | `.onion/` universal | 🎯 |

---

## 🔄 Estratégia de Migração

### Migração Gradual (Recomendada)
1. **FASE 1** ✅: CLI standalone (concluída)
2. **FASE 2-3**: Migrar comandos + sistema de níveis (2 semanas)
3. **FASE 4**: IDE loaders (1 semana)
4. **FASE 5-6**: Abstractions + learnings (2 semanas)
5. **FASE 7-8**: Story points + documentação (1 semana)

**Total estimado**: 6 semanas

### Retrocompatibilidade
- ✅ Manter `.claude/` como symlinks para `.onion/` durante transição
- ✅ Suportar ambas estruturas simultaneamente
- ✅ Documentar migração em `docs/onion/migration-guide-v3-to-v4.md`

---

## 💡 Benefícios Esperados

### Para Usuários
- ✅ **Onboarding 10-15x mais rápido** (4-8h → 15-30min por contexto)
- ✅ **Descoberta progressiva** (aprende comandos quando necessário)
- ✅ **Multi-IDE** (não depende só do Claude Code)
- ✅ **Separação clara** (business ≠ technical ≠ CS)
- ✅ **Starter kits** (5 comandos essenciais cobrem 80% dos casos)

### Para o Sistema
- ✅ **Arquitetura agnóstica de IDE** (portabilidade total)
- ✅ **Extensibilidade** (novos contextos, abstractions, comandos)
- ✅ **Ciclo fechado** (aprendizado contínuo com learnings)
- ✅ **Categoria única** (Multi-Context Development Orchestrator)
- ✅ **Sem concorrentes diretos** (único no mercado)

---

## 🚨 Riscos e Mitigações

### Risco 1: Onboarding Ainda Complexo
**Risco**: Mesmo com starter kits, 5 comandos podem ser muito

**Mitigação**:
- Sistema de help inteligente com estatísticas pessoais
- Descoberta progressiva (comandos sugerem outros)
- Vídeos e demos por contexto
- Templates e exemplos reais

### Risco 2: IDEs Não Suportam Loaders
**Risco**: Windsurf ou Claude não permitem loaders customizados

**Mitigação**:
- Fallback para `AGENTS.md` universal
- Contribuir para projetos open-source dos IDEs
- RFC do Onion Format para padronização

### Risco 3: Breaking Changes v3 → v4
**Risco**: Projetos existentes quebram na migração

**Mitigação**:
- Symlinks `.claude/` → `.onion/` para compatibilidade
- Script `onion migrate` automático
- Suporte a ambas estruturas durante transição
- Documentação detalhada de migração

### Risco 4: Complexidade de Manutenção
**Risco**: Manter 6+ contextos é muito trabalho

**Mitigação**:
- Meta-comandos para autogeração
- Validações automáticas
- CI/CD para testes contínuos
- Documentação sempre sincronizada

---

## 📅 Timeline Proposta

### Semana 1-2: FASE 2 - Migração de Comandos e Agentes
- Migrar comandos business context
- Migrar comandos technical context
- Classificar por nível (starter/intermediate/advanced)
- Criar `.context-config.yml` por contexto

### Semana 3: FASE 3 - Sistema de Níveis
- Implementar estrutura de níveis
- Criar `/help` inteligente
- Implementar descoberta progressiva
- Documentar sistema de níveis

### Semana 4: FASE 4 - IDE Loaders
- Implementar Claude Code Loader funcional
- Implementar Windsurf Loader
- Implementar Claude Loader
- Testar em todos os IDEs

### Semana 5: FASE 5-6 - Abstractions e Learnings
- Implementar `/meta/create-abstraction` completo
- Criar abstractions essenciais
- Implementar gestão de learnings
- Ciclo fechado funcionando

### Semana 6: FASE 7-8 - Story Points e Documentação
- Implementar ciclo cross-context SP
- Histórico e ML para estimativas
- Reescrever documentação completa
- Release Onion v4.0.0

---

## 🎯 Decisões Críticas

### ❓ Decisão 1: Nomenclatura de Pasta
**Opções**:
- A) `knowbase` (atual, consistente com `docs/knowledge-base/`)
- B) `kb` (mais curto)
- C) `knowledge-base` (mais explícito)

**Recomendação**: ✅ **`knowbase`** (consistente com estrutura atual)

### ❓ Decisão 2: Estratégia de CLI
**Opções**:
- A) CLI gera `.onion/` e `.claude/` automaticamente
- B) CLI gera `.onion/` e usuário cria symlinks manualmente
- C) CLI gera apenas `.onion/` e IDEs precisam suportar

**Recomendação**: ✅ **Opção A** (gera ambos para máxima compatibilidade)

### ❓ Decisão 3: Retrocompatibilidade
**Opções**:
- A) Suportar `.claude/` indefinidamente
- B) Deprecar `.claude/` em v5.0.0
- C) Deprecar `.claude/` imediatamente

**Recomendação**: ✅ **Opção B** (suporte durante v4, deprecar em v5)

---

## 📚 Referências

### Documentos de Visão
- `docs/knowledge-base/frameworks/onion-multi-context-orchestrator-vision.md`
- `docs/knowledge-base/frameworks/onion-complete-cycle-understanding.md`
- `docs/knowledge-base/frameworks/onion-ide-integration-strategy.md`
- `docs/knowledge-base/frameworks/onion-system-critical-analysis-2025.md`

### Documentação Técnica
- `docs/onion/cli-implementation-complete.md`
- `docs/onion/MILESTONE-first-successful-init.md`
- `docs/knowledge-base/concepts/spec-driven-development.md`
- `docs/knowledge-base/concepts/spec-as-code-strategy.md`

### Ferramentas Similares
- OpenSpec: https://github.com/Fission-AI/OpenSpec
- GitHub Spec-Kit: https://github.com/github/spec-kit
- BMAD-method: https://github.com/bmad-code-org/BMAD-METHOD

---

## ✅ **Este plano está correto? Posso proceder com a criação da task no Task Manager?** [Y/n]

**⚠️ IMPORTANTE:** 
- Este é um **ÉPICO** (4-6 semanas, > 13 story points)
- Será quebrado em **8 tasks menores** (uma por FASE)
- **FASE 1 já concluída** (CLI wizard implementado)
- Próxima: **FASE 2** (Migração de comandos)

**💡 RECOMENDAÇÃO:**
- Criar task ÉPICA principal para tracking
- Criar 7 subtasks (uma por FASE restante)
- Começar com FASE 2 imediatamente

---

**Última atualização**: 2025-12-20  
**Versão**: 1.0.0  
**Status**: Aguardando confirmação do usuário

