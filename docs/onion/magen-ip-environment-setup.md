# 🧅 Configuração do Ambiente Onion — Magen IP

> **Data:** 2026-05-15
> **Branch:** `main`
> **Repositórios:** `onion-claude` + `onion-claude-magen` (privado)
> **Provider de Tasks:** Jira Cloud

Documento operacional consolidando o setup do ambiente de desenvolvimento da **Magen IP** sobre o Sistema Onion, incluindo configuração do Jira, fundamentos arquiteturais, ciclos de trabalho e uso da skill `onion` para orientação contínua.

---

## 1. 🔌 Configuração do Jira

**Provider ativo:** `TASK_MANAGER_PROVIDER=jira`

| Variável | Valor |
|----------|-------|
| `JIRA_HOST` | `magen.atlassian.net` (Cloud — REST v3 + ADF) |
| `JIRA_EMAIL` | `magen@gmail.com` |
| `JIRA_API_TOKEN` | configurado · [gerar/gerenciar token](https://id.atlassian.com/manage-profile/security/api-tokens) |
| `JIRA_PROJECT_KEY` | `MAGEN` |
| `JIRA_AUTH_TYPE` | `basic` (default) |
| `JIRA_API_VERSION` | `3` (default) |

**Roteamento ativo:** qualquer comando que toca tasks delega para [`@jira-specialist`](../../.claude/agents/development/jira-specialist.md).

**Particularidades do Jira Cloud v3 que o Onion respeita automaticamente:**
- Descriptions e comments em **ADF** (Atlassian Document Format — JSON estruturado), nunca Markdown puro
- Workflow-aware: nunca seta `status` direto — sempre `POST /issue/{key}/transitions`
- Search: `POST /rest/api/3/search/jql` com `nextPageToken` (o antigo `GET /search` foi removido em maio/2025)
- Bulk operations via `POST /rest/api/3/issue/bulk` (até 50/req)

**Issue de referência ativa:** `MAGEN-17` (revisão estrutural `.claude/`).

---

## 2. 🧅 Sistema Onion — Apresentação e Arquitetura

### O que é
Framework de comandos `.claude/` para uso interno (não-pacote, não-CLI npm). Multi-Context Development Orchestrator que mescla:

- **94 comandos** em 11 categorias (`engineer`, `product`, `git`, `docs`, `meta`, `validate`, `test`, `common`, `development`, `quick`, `global`)
- **49 agentes especializados** em 9 categorias (`development`, `product`, `meta`, `compliance`, `git`, `testing`, `review`, `research`, `deployment`)
- **Skills** com ativação semântica (padrão novo, equivalente a custom commands mas com ativação por contexto)
- **Task Manager Abstraction** plugável (Jira/ClickUp/Asana/Linear/none)

### Estrutura de Diretórios
```
onion-claude/
├── .claude/
│   ├── commands/          # 94 comandos (workflows)
│   ├── agents/            # 49 agentes (subagents especializados)
│   ├── skills/            # Skills (padrão novo, ativação semântica)
│   │   ├── onion/         # Cérebro: routing + dynamic injection
│   │   ├── onion-patterns/    # Padrões estruturais
│   │   ├── onion-validation/  # Regras de validação
│   │   └── language-standards/# Idioma EN/PT-BR
│   ├── sessions/          # Contexto persistente por feature
│   └── utils/task-manager/# Adapters Jira/ClickUp/Asana/Linear
├── docs/
│   ├── meta-specs/        # L0 — constituição (specs do sistema)
│   ├── business-context/  # Especificação de negócio
│   ├── technical-context/ # Especificação técnica
│   ├── knowledge-base/    # KBs estruturadas para consumo por IA
│   │   ├── concepts/
│   │   ├── frameworks/
│   │   ├── platforms/
│   │   └── tools/
│   └── onion/             # Docs operacionais do framework
└── CLAUDE.md              # Instruções globais (provider-aware)
```

### Fundamentos do Onion
1. **Spec as Code Multi-Context** — Meta Specs (L0) → Business/Technical Context (L1) → Feature Specs (L2) → Task Specs (L3). Toda decisão arquitetural rastreável.
2. **Provider-Aware Routing** — comandos não conhecem o provider; lêem `TASK_MANAGER_PROVIDER` e delegam.
3. **Progressive Disclosure** — skills carregam só `description` no startup; conteúdo completo só quando relevante.
4. **Sessões de contexto** — `.claude/sessions/<feature-slug>/` mantém `context.md`, `plan.md`, `architecture.md` entre invocações.
5. **Idioma duplo** — código/commits/branches em EN; comments/docs/respostas em PT-BR.

---

## 3. 🛠️ O que foi feito hoje

**Push para GitHub:**
- `origin` → https://github.com/marciocar/onion-claude
- `magen` → https://github.com/marciocar/onion-claude-magen (privado)

---

## 4. 📚 Knowledge Base, Business e Technical Context para Magen IP

A **estrutura está pronta**, mas `business-context/` e `technical-context/` ainda só têm `README.md` placeholder. Para popular com o conhecimento da Magen IP:

### Comandos de geração de docs
| Intenção | Comando |
|----------|---------|
| Gerar docs de negócio (mission, vision, ICP, market) | `/docs:build-business-docs` |
| Gerar docs técnica (stack, arquitetura, integrações) | `/docs:build-tech-docs` |
| Criar nova knowledge base específica | `/meta:create-knowledge-base` |
| Atualizar índices após popular | `/docs:build-index` |
| Engenharia reversa de projeto existente | `/docs:reverse-consolidate` |

### Sequência recomendada para Magen IP
```
1. /product:collect            # Coletar inputs sobre a Magen IP
2. /product:refine             # Refinar e estruturar
3. /docs:build-business-docs   # Gera docs/business-context/ (negócio, ICP, market)
4. /docs:build-tech-docs       # Gera docs/technical-context/ (stack, arquitetura)
5. /meta:create-knowledge-base # KBs específicas (ex: domínio IP, frameworks setoriais)
6. /docs:build-index           # Atualizar índices
```

### Boas práticas
- **Cada arquivo em `business-context/`** = uma decisão de negócio rastreável (ICP, value prop, pricing, GTM)
- **Cada arquivo em `technical-context/`** = uma decisão técnica (ADR, integrations, infra, security model)
- **Knowledge bases** ficam em `docs/knowledge-base/<categoria>/<topico>.md` — categorias: `concepts`, `frameworks`, `platforms`, `tools`
- Tudo conectado às **meta-specs** (`docs/meta-specs/`) — a constituição valida conformidade via `@metaspec-gate-keeper`

---

## 5. 🔍 Engenharia Reversa — Aplicar o Onion em Projetos Existentes

> **Uma das capacidades mais poderosas do Sistema Onion:** documentar projetos legados em minutos, usando a IA para fazer 80-90% do trabalho pesado de leitura, mapeamento e descrição — restando ao profissional apenas refinar os 10-20% finais.

Essa estratégia é **espetacular** para:
- 📚 **Documentar sistemas legados** sem documentação ou com docs desatualizadas
- 💸 **Recuperar débitos técnicos** trazendo clareza sobre o que existe antes de refatorar
- 🧠 **Onboarding acelerado** de novos devs em códigos complexos
- 🏗️ **Spec as Code retroativo** — transformar código existente em especificação estruturada
- 🎯 **Auditoria arquitetural** de projetos herdados de outras equipes

### 🚀 Procedimento Completo (5 passos)

#### Passo 0 — Preparar o terreno
Clonar o Sistema Onion e copiar `.claude/` + `docs/` para o projeto-alvo:

```bash
# Clonar o Sistema Onion
git clone https://github.com/marciocar/onion-claude.git

# Copiar a estrutura Onion para o seu projeto legado
cp -r onion-claude/.claude /caminho/do/projeto-legado/
cp -r onion-claude/docs    /caminho/do/projeto-legado/

# Entrar no projeto e configurar
cd /caminho/do/projeto-legado/
# (configure .env com o task manager se for usar)
```

> 💡 **Resultado:** agora o projeto-alvo tem acesso aos 94 comandos, 49 agentes e 4 skills do Onion — incluindo toda a inteligência de engenharia reversa.

#### Passo 1 — Engenharia reversa do projeto
```bash
/docs:reverse-consolidate
```
Cria a documentação base em `docs/reverse/*` — mapeia stack, estrutura, dependências, fluxos principais.

#### Passo 2 — Gerar Business Context
```bash
/docs:build-business-docs docs/reverse
```
A IA fará perguntas estratégicas (modelo de negócio, ICP, value prop). **Responda com sinceridade** — quanto melhor o input, melhor o output.
> Resultado: `docs/business-context/` populado com decisões de negócio rastreáveis.

#### Passo 3 — Gerar Technical Context
```bash
/docs:build-tech-docs docs/reverse
```
A IA fará perguntas técnicas (arquitetura, integrações, dívidas, riscos). **Responda as perguntas** — esse é o momento de transferir conhecimento tácito para documentação.
> Resultado: `docs/technical-context/` populado com ADRs, decisões técnicas e mapas de integração.

#### Passo 4 — Indexar a documentação completa
```bash
/docs:build-index
```
Atualiza `docs/INDEX.md` e índices de seção, conectando tudo num hub navegável.

#### Passo 5 — Começar a trabalhar
```bash
/warm-up
```
Carrega todo o contexto recém-criado e a inteligência do Onion. **Pronto para a batalha.**

### 🧠 O Mantra do Spec as Code com IA

> **"A IA faz 80% a 90% — depois o profissional revisa e completa os 10% a 20% que faltam."**

A IA traz velocidade absurda na leitura, classificação e descrição inicial. O profissional traz **contexto histórico**, **decisões não escritas** e **intuição arquitetural** que só anos de projeto entregam. Essa combinação é o que transforma um sistema legado opaco em **Spec as Code vivo** em poucas horas — algo que tradicionalmente leva semanas ou meses.

### 🎯 Por que isso é espetacular

| Sem Onion | Com Onion |
|-----------|-----------|
| Semanas lendo código sem rumo | Horas com mapa estruturado completo |
| Documentação fica desatualizada | Specs versionadas em Git ao lado do código |
| Conhecimento concentrado em poucas pessoas | Spec navegável por qualquer dev ou IA |
| Refatorar é arriscado (não sabe o que quebra) | Refatorar com clareza do impacto |
| Onboarding leva meses | Onboarding em dias com `/warm-up` |
| Débito técnico invisível | Débito técnico catalogado e priorizado |

### ⚠️ Cuidados importantes

1. **Revise o output da IA** — ela infere muito do código, mas pode errar nuances de negócio.
2. **Responda as perguntas com tempo** — perguntas mal respondidas geram docs medíocres.
3. **Trate como rascunho navegável** — refine iterativamente conforme aprende mais do sistema.
4. **Versione tudo no Git** — commits dos `docs/` capturam evolução do entendimento.
5. **Use `@metaspec-gate-keeper`** para validar conformidade das specs geradas com a constituição do projeto.

### 📋 Visual rápido do fluxo

```
git clone onion-claude
       │
       ▼
copy .claude/ + docs/ → projeto-legado/
       │
       ▼
/docs:reverse-consolidate   ──► docs/reverse/*
       │
       ▼
/docs:build-business-docs docs/reverse  (responda perguntas)
       │                                  ──► docs/business-context/
       ▼
/docs:build-tech-docs docs/reverse      (responda perguntas)
       │                                  ──► docs/technical-context/
       ▼
/docs:build-index           ──► docs/INDEX.md atualizado
       │
       ▼
/warm-up                    ──► pronto para trabalhar com clareza total
```

---

## 6. 🔄 Próximos Passos — Ciclos de Produto e Engenharia

O Onion separa **ciclo de produto** (descoberta + planejamento) do **ciclo de engenharia** (implementação + entrega). Ambos integram com Jira automaticamente.

### 🎯 Ciclo de Produto (descoberta → task)

Fluxo: **coletar → refinar → especificar → estimar → criar task no Jira**

| # | Comando | O que faz | Saída no Jira |
|---|---------|-----------|---------------|
| 1 | `/product:collect` | Coleta requisitos brutos (reunião, áudio, texto) | — |
| 2 | `/product:refine` | Estrutura inputs em hipóteses e dores | — |
| 3 | `/product:spec` | Especificação completa do produto/feature | — |
| 4 | `/product:estimate` | Story points via [`@story-points-framework-specialist`](../../.claude/agents/product/story-points-framework-specialist.md) | — |
| 5 | `/product/task` | Cria task hierárquica no Jira via `@jira-specialist` | Issue criada em `MAGEN` |

**Comandos auxiliares de produto:**
- `/product:whisper` — transcrever áudios de reuniões
- `/product:extract-meeting` — extrair ata estruturada (framework EXTRACT)
- `/product:consolidate-meetings` — sintetizar várias reuniões em insights
- `/product:convert-to-tasks` — converter qualquer documento em tasks Jira
- `/product:analyze-pain-price` — analisar dor do cliente e value-pricing
- `/product:branding` — branding e posicionamento

**Warm-up de produto:** `/product/warm-up` (carrega contexto Magen IP no início da sessão).

---

### 🛠️ Ciclo de Engenharia (task → código → PR)

Fluxo: **iniciar → trabalhar → preparar PR → criar PR → sincronizar**

| # | Comando | O que faz | Efeito no Jira |
|---|---------|-----------|----------------|
| 1 | `/engineer/start <MAGEN-XX>` | Valida story points, cria branch GitFlow, sessão `.claude/sessions/<slug>/` | Transition para "In Progress" + comment ADF |
| 2 | `/engineer/work` | Loop iterativo de desenvolvimento dentro da sessão | Sync de progresso (comments) |
| 3 | `/engineer/plan` | Planejamento detalhado de implementação (subtasks) | Cria subtasks se necessário |
| 4 | `/engineer/pre-pr` | Lint, testes, code review preventivo | Comment de readiness |
| 5 | `/engineer/pr` | Cria Pull Request com template | Comment com URL do PR |
| 6 | `/engineer/pr-update` | Atualiza PR existente após revisão | — |
| 7 | `/git:sync` | Sincroniza branch após merge (rebase main) | Transition para "Done" |

**Fluxos paralelos:**
- **Hotfix:** `/engineer/hotfix → /engineer/work → /engineer/pr → /git:hotfix:finish`
- **Bump de versão:** `/engineer/bump` (semver automático)

**Warm-up de engenharia:** `/engineer/warm-up` (carrega contexto técnico antes de codar).

---

### 🧪 Qualidade e Validação (transversal)

| Quando | Comando |
|--------|---------|
| Antes de PR | `/test:unit` + `/test:integration` |
| Antes de release | `/test:e2e` |
| Review de branch completa | `@branch-code-reviewer` |
| Validar conformidade arquitetural | `@metaspec-gate-keeper` |
| Validar workflow do Onion | `/validate:workflow` |

---

### 📋 Visão consolidada do ciclo completo (Magen IP no Jira)

```
┌─────────────────── CICLO DE PRODUTO ───────────────────┐
│                                                         │
│  /product:collect → /product:refine → /product:spec    │
│                          │                              │
│                          ▼                              │
│                  /product:estimate                      │
│                          │                              │
│                          ▼                              │
│                    /product/task   ──────► Jira MAGEN-XX
│                                                         │
└─────────────────────────┼───────────────────────────────┘
                          │
                          ▼
┌────────────────── CICLO DE ENGENHARIA ─────────────────┐
│                                                         │
│  /engineer/start MAGEN-XX ──► branch + sessão Jira     │
│                          │                              │
│                          ▼                              │
│                  /engineer/work (loop)                  │
│                          │                              │
│                          ▼                              │
│                  /engineer/pre-pr                       │
│                          │                              │
│                          ▼                              │
│                    /engineer/pr ──► PR GitHub + Jira    │
│                          │                              │
│                          ▼                              │
│                       /git:sync ──► Done no Jira        │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 7. 🧅 Como usar a Skill `onion` — O guia anti-perdido

A skill [`onion`](../../.claude/skills/onion/SKILL.md) é o **cérebro do sistema**: orquestrador master que se ativa automaticamente quando você precisa de orientação. Use-a sempre que estiver em dúvida — ela responde com **estado atual do projeto + roteamento exato**.

### 🎯 Quando a skill `onion` é ativada

A skill ativa automaticamente (sem você precisar digitar nada) quando você:

| Situação | Frase típica |
|----------|--------------|
| Está perdido | "o que faço agora?", "por onde começo?" |
| Precisa de próximos passos | "próximos passos", "qual o próximo comando?" |
| Quer entender o sistema | "como funciona?", "o que tem aqui?" |
| Procura um agente/comando | "qual agente para X?", "tem comando pra Y?" |
| Vai configurar integração | "como configuro o Jira?", "como troco de provider?" |
| Vai criar algo | "como crio um agente/skill/comando?" |
| Mudou de contexto | retornou após pausa, abriu janela nova |

Você também pode **invocar explicitamente** com `/onion` ou `/onion <pergunta livre>`.

### 🔍 O que a skill `onion` te entrega

Quando ativada, ela injeta dinamicamente:

1. **Estado atual do projeto**
   - Provider ativo (lendo `.env`): no nosso caso, `jira`
   - Sessões abertas: quais features estão em andamento em `.claude/sessions/`
   - Branch atual (Git)

2. **Tabelas de routing por intenção** — 7 domínios completos:
   - Desenvolvimento de Feature
   - Produto e Discovery
   - Documentação
   - Criar Componentes do Onion
   - Qualidade e Revisão
   - Git e Versionamento
   - Task Managers (provider-aware)

3. **Lista completa de agentes** por categoria (49 agentes em 9 categorias)

4. **Gotchas críticos** — armadilhas comuns:
   - Provider obrigatório no `.env`
   - Feature slug kebab-case
   - Sessões de contexto
   - Formatação ADF vs Markdown vs Unicode
   - Commands vs Skills vs Agentes
   - Search Jira 2025 (endpoint novo)

### 💡 Casos de uso típicos

#### Caso 1: "Comecei a sessão e não sei o que fazer"
```
você → /onion

onion → mostra:
  - Provider ativo: jira
  - Sessões abertas: (nenhuma)
  - Branch: main
  - Tabela completa de comandos
  - Recomendação: começar com /product:collect ou /engineer/start <issue>
```

#### Caso 2: "Tenho uma reunião gravada e quero virar tasks"
```
você → /onion como transformo reunião em tasks?

onion → roteia para:
  1. /product:whisper        (se for áudio)
  2. /product:extract-meeting (estruturar ata)
  3. /product:convert-to-tasks (gerar issues no Jira)
```

#### Caso 3: "Vou começar a desenvolver MAGEN-23"
```
você → /onion vou começar MAGEN-23

onion → checa sessões abertas + sugere:
  1. /engineer/start MAGEN-23  (cria branch + sessão)
  2. /engineer/work             (loop de dev)
  3. /engineer/pre-pr           (antes do PR)
  4. /engineer/pr               (criar PR)
```

#### Caso 4: "Esqueci qual agente usa pra documentação"
```
você → /onion qual agente para diagramas?

onion → tabela de routing mostra:
  - @c4-architecture-specialist  (C4 Model + Mermaid)
  - @mermaid-specialist           (Mermaid puro)
  - @c4-documentation-specialist  (textual C4)
```

#### Caso 5: "Quero criar um agente novo pra um domínio específico"
```
você → /onion como crio um agente?

onion → roteia para:
  /meta:create-agent

  E mostra os agentes que validam o resultado:
  - @agent-creator-specialist
  - @metaspec-gate-keeper
```

### 🚀 Comandos relacionados de navegação

| Comando | Quando usar |
|---------|-------------|
| `/onion` | Visão geral + routing (qualquer dúvida) |
| `/warm-up` | Warm-up geral do projeto (início de dia) |
| `/product/warm-up` | Foco em produto (descoberta, ICP, requisitos) |
| `/engineer/warm-up` | Foco em engenharia (código, arquitetura, testes) |
| `/meta:all-tools` | Lista exaustiva de todas as ferramentas |

### ⚠️ Dicas para maximizar a skill

1. **Ative cedo na sessão** — invoque `/onion` no início para popular o contexto com estado atual.
2. **Use linguagem natural** — não precisa decorar comandos. `/onion preciso atualizar um PR que abri ontem` funciona.
3. **Confie no routing** — se a tabela diz `@jira-specialist`, é onde a expertise está. Não tente reinventar.
4. **Leia os gotchas** — eles refletem erros reais já cometidos no projeto (provider ausente, slug com underscore, formatação errada no Jira).
5. **Verifique o provider** — sempre que mudar de máquina ou clonar o repo, confirme que `.env` está com `TASK_MANAGER_PROVIDER=jira` antes de qualquer comando de tasks.

### 🔗 Skills complementares

A skill `onion` trabalha junto com:
- [`onion-patterns`](../../.claude/skills/onion-patterns/SKILL.md) — ativa ao editar `.claude/**` (padrões estruturais)
- [`onion-validation`](../../.claude/skills/onion-validation/SKILL.md) — ativa ao validar artefatos `.claude/**`
- [`language-standards`](../../.claude/skills/language-standards/SKILL.md) — ativa ao escrever código/docs/commits

Você não precisa invocá-las — elas ativam automaticamente pelo contexto.

---

## ✅ Status Final

- **Ambiente Onion:** operacional (skills + comandos + agentes alinhados)
- **Jira (Magen IP):** conectado (project `MAGEN`, Cloud v3 + ADF)
- **GitHub:** 2 repos sincronizados (`onion-claude` + `onion-claude-magen` privado)
- **Conteúdo Magen IP nos contexts:** estrutura pronta, **conteúdo pendente** (próxima sessão de discovery)

**Próximo comando sugerido:**
- Se você tem inputs prontos da Magen IP → `/product:collect`
- Se você quer popular docs técnicas direto → `/docs:build-tech-docs`
- Se está sem direção → `/onion` (a skill te orienta)

---

**Mantido por:** Sistema Onion
**Referências:**
- [Índice geral](../INDEX.md)
- [Guia de comandos](commands-guide.md)
- [Referência de agentes](agents-reference.md)
- [Task Manager Abstraction](../knowledge-base/concepts/task-manager-abstraction.md)
