# Onion em Projeto Novo (Greenfield)

> Guia passo a passo para aplicar o Sistema Onion em um projeto sem código nem documentação prévia.

---

## Pré-requisitos

- Google Antigravity instalado (config global em `~/.gemini/`)
- Git instalado
- Acesso ao repositório do Onion (para copiar `.agents/` e estrutura `docs/`)
- Decisão sobre Task Manager: Jira, ClickUp, Asana, Linear ou `none` (operação offline)
- Decisão sobre se o projeto exige compliance regulatório (se sim, ver também [applying-regulated.md](./applying-regulated.md))

---

## Quando este guia se aplica

- Projeto que **ainda não existe** ou **acabou de ser inicializado** (`git init` recente)
- Sem README, sem código fonte, sem arquitetura prévia
- Equipe quer começar com Onion desde o dia zero

Se o projeto **já tem código**, consultar [applying-legacy.md](./applying-legacy.md).

---

## Passo 1 — Estrutura inicial do projeto-alvo

```bash
# No diretório do projeto-alvo
mkdir meu-projeto && cd meu-projeto
git init
```

---

## Passo 2 — Copiar o Onion

Copiar do repositório do Onion para o projeto-alvo:

- `.agents/` integral (AGENTS.md, rules/, workflows/, skills/, hooks.json, mcp_config.example.json)
- `docs/meta-specs/` (constituição do framework — pode ser referenciada via symlink ou cópia)
- `docs/reference/` (Task Manager Abstraction + utilitários consumidos pelos workflows)
- `docs/sdaal/` (KB do padrão SDAAL)
- Templates de `docs/business-context/README.md`, `docs/technical-context/README.md`, `docs/compliance-context/README.md`
- `.env.example` (renomear para `.env` e configurar)
- (opcional) copiar `.agents/mcp_config.example.json` para `~/.gemini/config/mcp_config.json`

> As personas/regras do Onion vivem em `.agents/AGENTS.md` + `.agents/rules/` (substituem o antigo `CLAUDE.md`). Ajustar conforme passo 4.

Estrutura resultante mínima no projeto-alvo:

```
meu-projeto/
├── .agents/                # Operacional Onion (AGENTS.md, rules/, workflows/, skills/, hooks.json)
├── docs/
│   ├── business-context/   # Vazio inicialmente (será populado)
│   ├── technical-context/  # Vazio inicialmente
│   ├── compliance-context/ # Vazio (criar apenas se regulado)
│   ├── meta-specs/         # Constituição (cópia ou referência)
│   ├── reference/          # Task Manager Abstraction + utilitários
│   ├── sdaal/              # Referência do padrão SDAAL
│   └── knowledge-base/     # Vazio inicialmente
└── .env (não commitado)
```

> Config global do Antigravity (MCP, GEMINI.md, skills compartilhadas) vive em `~/.gemini/`, fora do repositório.

---

## Passo 3 — Configurar integrações

```bash
/meta-setup-integration
```

O workflow guiará a configuração de:

- `TASK_MANAGER_PROVIDER` (jira | clickup | asana | linear | none)
- Variáveis específicas do provider escolhido
- MCPs aplicáveis (em `~/.gemini/config/mcp_config.json`)

Se for operar offline (sem Task Manager): definir `TASK_MANAGER_PROVIDER=none`. Os workflows `/product-*` continuarão funcionando mas não persistirão em Task Manager externo.

---

## Passo 4 — Adaptar AGENTS.md e rules ao projeto-alvo

O `.agents/AGENTS.md` + `.agents/rules/` do Onion são genéricos. Para o projeto-alvo, ajustar:

1. Substituir descrição "Sistema Onion" pela descrição do projeto-alvo (em `AGENTS.md`)
2. Manter as rules de Task Manager routing (são úteis)
3. Adicionar contexto específico do projeto (stack pretendida, equipe, restrições)
4. Manter referência às meta-specs do Onion como guia normativo

---

## Passo 5 — Gerar contexto de negócio inicial

```bash
/docs-build-business-docs
```

O workflow passará por:

1. **Descoberta** — analisa o que existe (README inicial, materiais externos se fornecidos)
2. **Discussão** — pergunta visão, personas, público-alvo, modelo de negócio
3. **Geração** — preenche `docs/business-context/` seguindo a estrutura em [docs/business-context/README.md](../business-context/README.md)

Em greenfield, este passo define a base estratégica antes de qualquer código.

---

## Passo 6 — Gerar contexto técnico inicial

```bash
/docs-build-tech-docs
```

O workflow passará por:

1. **Descoberta** — escaneia o pouco que existe (arquivos de config, decisões inicialmente declaradas)
2. **Discussão** — pergunta sobre stack pretendida, padrões arquiteturais, restrições, trade-offs
3. **Geração** — preenche `docs/technical-context/` com ADRs iniciais, charter, AI development guide

Em greenfield, este passo define a arquitetura intencional antes da implementação.

---

## Passo 7 — Iniciar primeiro ciclo de desenvolvimento

### Camada Produto

```bash
/product-collect    # Coletar ideias iniciais de features
/product-refine     # Refinar via perguntas
/product-spec       # Criar spec da primeira feature
/product-task       # Decompor em tasks
/product-estimate   # Estimar story points
/product-feature    # Criar no Task Manager (se configurado)
```

### Camada Engenharia

```bash
/engineer-plan      # Planejar implementação
/engineer-start     # Criar sessão de desenvolvimento (Artifacts / docs/sessions)
/engineer-work      # Executar fase atual
# ... iterar work até pronto
/engineer-pre-pr    # Validação pré-PR
/engineer-pr        # Abrir Pull Request
```

---

## Passo 8 — Manter contextos atualizados

A cada mudança significativa de produto ou arquitetura:

```bash
/docs-build-business-docs   # Atualizar business context
/docs-build-tech-docs       # Atualizar technical context
/docs-build-index           # Reconstruir INDEX
```

---

## Compliance opcional

Se durante a evolução do projeto surgir requisito regulatório:

```bash
mkdir -p docs/compliance-context
# Copiar README de docs/compliance-context/README.md do Onion
/docs-build-compliance-docs
```

Detalhes em [applying-regulated.md](./applying-regulated.md).

---

## Troubleshooting

### Workflow não é reconhecido

- Verificar que `.agents/` foi copiado integralmente (incl. `workflows/`)
- Confirmar que o Google Antigravity está aberto no diretório correto do projeto-alvo
- Recarregar o workspace

### Task Manager não responde

- Verificar variáveis em `.env`
- Rodar `/meta-setup-integration` novamente
- Validar token/credenciais com o provider
- Conferir `~/.gemini/config/mcp_config.json` se a integração usar MCP

### Persona/subagent não encontrado

- Verificar que `.agents/AGENTS.md` foi copiado
- Confirmar que a persona está definida em `AGENTS.md` (Role/@handle/Goal/Traits/Constraint)

---

## Checklist de "primeiro workflow útil"

Considera-se Onion operacional no projeto-alvo quando:

- [ ] `.agents/` copiado integralmente
- [ ] `.env` configurado com `TASK_MANAGER_PROVIDER`
- [ ] `/meta-setup-integration` executado sem erros
- [ ] `/docs-build-business-docs` gerou `business-context/` populado
- [ ] `/docs-build-tech-docs` gerou `technical-context/` populado
- [ ] Primeiro `/product-task` criou task com sucesso (ou foi processado offline)
- [ ] Primeiro `/engineer-start` criou sessão (Artifacts / `docs/sessions/`)

---

**Próximo guia**: [Onion em projeto legado](./applying-legacy.md) | [Onion em projeto regulado](./applying-regulated.md)
