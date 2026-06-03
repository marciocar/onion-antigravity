# Regra: Identidade do Sistema Onion

O **Sistema Onion** é um **framework template em `.agents/`** projetado para ser
instalado e aplicado em qualquer projeto (novo, legado ou regulado) para
orquestrar o ciclo completo de desenvolvimento com o **Google Antigravity**.

## Invariantes
- Framework template em `.agents/` — **não é produto npm**, **não é distribuído
  publicamente**, **não tem CLI standalone próprio**.
- **Plataforma única: Google Antigravity** (IDE agêntico).
- Cobre **três dimensões peer** do ciclo: **produto**, **engenharia**,
  **compliance/governança** — não hierarquizadas.
- **Workflows faseados retomáveis** são invariantes do framework:
  - `product-collect → product-refine → product-spec → product-feature` (descoberta a backlog)
  - `engineer-plan → engineer-start → engineer-work → engineer-pre-pr → engineer-pr` (planejamento a entrega)
  - Não devem ser consolidados nem achatados em um único passo.

## Como o Onion roda no Antigravity
- **Rules** (`.agents/rules/`) — instruções always-on (identidade, idioma, roteamento, convenções).
- **Workflows** (`.agents/workflows/`) — saved prompts invocáveis com `/<categoria>-<comando>`.
- **Skills** (`.agents/skills/`) — conhecimento contextual carregado sob demanda.
- **Personas** (`.agents/AGENTS.md`) — a "equipe de IA" e leads de dimensão.
- **Artifacts** do Antigravity (task lists, implementation plans, walkthroughs)
  substituem as sessões persistentes; complementarmente, contexto de feature pode
  ser versionado em `docs/sessions/<feature-slug>/`.
- **MCP** (`~/.gemini/config/mcp_config.json`) — integrações de task manager e APIs.

## Princípio
Onion é sobre **eficiência**, **qualidade** e **automação inteligente**. Sempre
detectar o provider ativo antes de operar com tasks; o mesmo workflow funciona em
Jira, ClickUp, Asana ou Linear quando o roteamento respeita o `.env`.
