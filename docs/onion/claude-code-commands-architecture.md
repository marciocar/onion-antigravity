# 🎮 Arquitetura de Workflows do Antigravity - Sistema Onion

> **Nota de migração**: este documento descrevia originalmente os "Claude Code Commands". O Sistema Onion migrou para o **Google Antigravity** em 2026-06 (ver [ADR-001](../analysis/onion-antigravity-migration-adr-2026-06.md)). O conteúdo abaixo foi adaptado para as primitivas nativas do Antigravity: **workflows**, **rules**, **skills** e **hooks** em `.agents/`.

Este documento explica como funcionam os workflows do Sistema Onion e a diferença crítica entre **workflows do Antigravity** (saved prompts `/`-invocáveis) e comandos de terminal.

## ⚡ **CONCEITO FUNDAMENTAL: Workflows do Antigravity**

### 🎯 **O que são workflows**
Workflows são saved prompts personalizados, definidos em Markdown em `.agents/workflows/`, executados diretamente no **agente do Google Antigravity** via `/`. Cada arquivo `<categoria>-<comando>.md` define um workflow invocável por `/<categoria>-<comando>`.

### ✅ **Como Usar (CORRETO)**
```markdown
# No agente do Antigravity:
/git-init                      # Inicializar Git Flow
/git-feature-start "login"     # Criar feature branch
/engineer-work "implement API" # Iniciar desenvolvimento
/product-task "add dashboard"  # Criar task no Task Manager ativo
```

### ❌ **Como NÃO Usar (INCORRETO)**
```bash
# ❌ NO TERMINAL - NÃO FUNCIONA:
$ /git-init                    # Comando não encontrado
$ ./git-feature-start          # Arquivo não executável
$ bash /git-init               # Não é script bash direto
```

---

## 🏗️ **Arquitetura do Sistema**

### 📁 **Estrutura de Arquivos**
```
.agents/
├── AGENTS.md                  # Personas / subagents (equipe de IA)
├── rules/                     # System instructions always-on
│   ├── onion-identity.md
│   ├── language-standards.md
│   ├── task-manager-routing.md
│   └── onion-conventions.md
├── workflows/                 # Saved prompts /-invocáveis (flat + prefixo de categoria)
│   ├── git-init.md            # Define /git-init
│   ├── git-help.md            # Define /git-help
│   ├── git-feature-start.md   # Define /git-feature-start
│   ├── git-feature-publish.md # Define /git-feature-publish
│   ├── git-feature-finish.md  # Define /git-feature-finish
│   ├── engineer-start.md      # Define /engineer-start
│   ├── engineer-work.md       # Define /engineer-work
│   ├── product-task.md        # Define /product-task
│   └── product-spec.md        # Define /product-spec
├── skills/                    # Conhecimento contextual on-demand
│   ├── onion/SKILL.md
│   └── onion-validation/SKILL.md
├── hooks.json                 # Hooks de ciclo de vida (Pre/PostToolUse, Pre/PostInvocation)
└── mcp_config.example.json    # Template MCP → ~/.gemini/config/mcp_config.json
```

> Comandos antes aninhados (`git/feature/start`) viram nomes achatados com prefixo (`git-feature-start.md`), garantindo unicidade no menu `/`.

### 🔄 **Fluxo de Execução**

| Passo | Camada | Tecnologia | Função |
|-------|--------|------------|--------|
| 1 | **Interface** | Agente do Antigravity | Usuário digita `/git-init` |
| 2 | **Detecção** | Antigravity (IA) | Reconhece o workflow |
| 3 | **Carregamento** | File System | Lê `.agents/workflows/git-init.md` |
| 4 | **Interpretação** | Antigravity (IA) | Analisa workflow + rules always-on |
| 5 | **Execução** | Ferramentas/Scripts | Executa bash/python dentro do workflow |
| 6 | **Artifacts** | Antigravity | Task lists / implementation plans / walkthroughs |
| 7 | **Feedback** | Agente do Antigravity | Resposta rica e educativa |

---

## 🎯 **Exemplo Detalhado: `/git-init`**

### 📝 **1. Usuário Executa o Workflow**
```markdown
# No agente do Antigravity:
User: /git-init
```

### 📄 **2. Antigravity Carrega a Definição**
```markdown
# Arquivo: .agents/workflows/git-init.md
# Define workflow completo de inicialização Git Flow
```

### 🧠 **3. Antigravity (IA) Interpreta o Workflow**
```bash
# Bash scripts embutidos no markdown executam:
# - Detecção master/main
# - Criação develop branch
# - Configuração Git Flow
# - Validações de segurança
```

### 💬 **4. Resposta Rica no Agente**
```markdown
🔧 GIT FLOW - Modern Initialization Wizard
━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 REPOSITORY ANALYSIS:
   ▶ Primary branch: ✅ main detected
   ▶ Git Flow status: ⚠️ Not initialized

❓ Initialize Git Flow in this repository? [Y/n]
```

---

## 🎨 **Vantagens dos Workflows do Antigravity**

### 🤖 **AI-Powered Intelligence**
-  **Context Awareness**: o Antigravity entende arquivos abertos, histórico, projeto e rules always-on
-  **Adaptive Execution**: workflows se adaptam ao contexto atual
-  **Error Recovery**: sugestões inteligentes para problemas
-  **Educational Feedback**: explica o que está fazendo

### 🎯 **Developer Experience**
-  **Natural Interface**: agente natural, sem sintaxe complexa
-  **Rich Responses**: feedback visual rico com cores e formatação
-  **Context Preservation**: estado persistente via Artifacts (task lists, implementation plans)
-  **Universal Access**: funciona em qualquer pasta, qualquer projeto

### 🔒 **Enterprise Features**
-  **Safety First**: confirmações para operações críticas (Allow/Deny/Ask)
-  **Team Integration**: Task Manager (Jira/ClickUp/Asana/Linear), Artifacts, project management
-  **Audit Trail**: histórico completo no agente + Artifacts revisáveis
-  **Knowledge Sharing**: workflows compartilháveis entre equipe (versionados em `.agents/`)

---

## 🛠️ **Para Desenvolvedores do Sistema**

### 📝 **Criando Novos Workflows**
```markdown
# 1. Criar arquivo markdown:
.agents/workflows/<categoria>-<comando>.md

# 2. Definir cabeçalho com metadados (nome, descrição)
# 3. Escrever workflow em prosa + bash/python
# 4. Usar funções UX: cli_header, cli_success_box, etc.
# 5. Testar via agente: /<categoria>-<comando>
```

> Dica: use o workflow `/meta-create-command` para gerar novos workflows seguindo os padrões do Sistema Onion.

### 🎨 **Padrões UX**
```bash
# Usar funções da biblioteca UX:
source ".agents/modern-cli-ux.sh"

cli_header "TITLE" "color"          # Headers consistentes
cli_success_box "TITLE" "message"   # Success feedback
cli_error_box "TITLE" "message"     # Error handling
cli_progress_start "message"        # Progress indicators
```

### 🔗 **Integrações**
```bash
# Task Manager via MCP (provider ativo definido em .env)
task_get_id_from_artifact           # Detectar task ativa
task_add_comment $TASK_ID           # Adicionar comentário
task_update_status $TASK_ID         # Atualizar status

# Estado persistente
# → Artifacts do Antigravity (task lists, implementation plans, walkthroughs)
# → opcionalmente versionado em docs/sessions/<feature>/
```

---

## 📚 **Recursos Adicionais**

### 🔗 **Documentação**
- [Sistema Onion Workflows Guide](commands-guide.md)
- [Engineering Flows](engineering-flows.md)
- [KB Antigravity](../knowledge-base/platforms/antigravity.md)
- [ADR-001: Migração Claude Code → Antigravity](../analysis/onion-antigravity-migration-adr-2026-06.md)

### 🎯 **Exemplos Práticos**
- [Practical Examples](practical-examples.md)
- [Tools Reference](tools-reference.md)

### 🚀 **Getting Started**
- [Configuração Inicial](getting-started.md)
- [Primeiro Uso](getting-started.md#primeiro-uso)
- [Troubleshooting](getting-started.md#troubleshooting)

---

## ⚠️ **Avisos Importantes**

### 🔴 **NÃO Confundir Com:**
- ❌ **Bash scripts diretos** (não são executáveis de terminal)
- ❌ **NPM scripts** (não estão no package.json)
- ❌ **Make targets** (não usam Makefile)
- ❌ **CLI tools globais** (não são instalados via npm/pip)

### ✅ **São Workflows do Antigravity Porque:**
-  **Executados no agente** do Google Antigravity
-  **Definidos em markdown** em `.agents/workflows/`
-  **Interpretados pela IA do Antigravity** com context awareness e rules always-on
-  **Integrados ao ambiente** de desenvolvimento

---

**🎯 Lembre-se sempre: Sistema Onion = workflows do Antigravity executados no agente do Google Antigravity!** 🚀
