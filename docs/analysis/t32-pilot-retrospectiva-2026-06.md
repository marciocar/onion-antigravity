---
title: Retrospectiva T3.2 — Validação de /docs:build-*-docs (auto-piloto)
date: 2026-06-02
status: executado
tarefa: T3.2 (plano de saneamento) + Fase 3 do plano de V&V
piloto: auto-piloto no próprio onion-claude (cenário legacy)
desbloqueia: T3.6 (definição de piloto) resolvido via auto-piloto
---

# Retrospectiva T3.2 — Validação de `/docs:build-*-docs`

Validação ponta a ponta dos comandos de geração de documentação do Onion,
usando o **próprio repositório `onion-claude`** como projeto-alvo piloto
(cenário **legacy**: repo real com código em `.claude/` e docs). Executado em
**worktree descartável** (`/tmp/onion-t32-pilot`, removido ao final) para não
poluir o repo-mãe.

## Método

| Comando | Como foi validado |
|---|---|
| `/docs:build-index` | **Execução ao vivo** do scan não-interativo (coleta de dados real) |
| `/docs:build-tech-docs` | **Probe de geração real**: fase de descoberta sobre o repo + geração de fatia representativa (index + charter + 1 ADR + codebase-guide) ancorada em evidência |
| `/docs:build-business-docs` | Validação de prontidão (agentes + template + estrutura); geração plena requer sessão interativa |
| `/docs:build-compliance-docs` | **Fora de escopo** do auto-piloto legacy — exige projeto regulado; follow-up |

> Os comandos `build-*-docs` são **interativos** (discovery → discussion →
> generation, com ≥10 perguntas ao usuário). A geração plena das 4 camadas
> requer uma sessão interativa; o probe prova que o **pipeline e a estrutura**
> funcionam.

## Resultados

### Pré-checagem (prontidão) — ✅ PASSOU

- **15/15 agentes** referenciados pelos build commands existem e carregam
  (business: product-agent, research-agent, storytelling-business-specialist,
  branding-positioning-specialist; tech: c4-architecture-specialist,
  c4-documentation-specialist, docs-reverse-engineer,
  system-documentation-orchestrator, mermaid-specialist; compliance:
  security-information-master, iso-27001/22301, soc2, pmbok,
  corporate-compliance-specialist em `review/`).
- **2/2 templates** resolvem (`business_context_template.md`,
  `technical_context_template.md`).

### `build-index` — ✅ EXECUTÁVEL

Scan não-interativo coletou corretamente: 77 comandos invocáveis, 49 agentes,
4 skills, 89 docs markdown, índices de seção (knowledge-base, meta-specs, onion).
Comando apto.

### `build-tech-docs` (probe) — ✅ APTO COM RESSALVAS

Gerou fatia representativa (281 linhas, 4 arquivos) seguindo a estrutura de 4
camadas sem ambiguidade bloqueante. A própria geração **expôs inconsistências
reais do repo** (ver achados) — evidência de que o comando entrega valor.

## Achados (gaps a tratar antes de aplicar em projeto-alvo externo)

### No comando/template

1. **Divergência de convenção de nomes template ↔ comando.** O template sugere
   `UPPERCASE` para genéricos (`CODEBASE_GUIDE.md`); o comando usa kebab-case
   (`codebase-guide.md`). Um operador que ler o template primeiro produz nomes
   errados. **Ação:** o comando deve declarar que sobrepõe a convenção do template.
2. **Stack non-code não coberta pela descoberta.** A Fase 1 assume manifestos
   (`package.json`, CI/CD). Projetos doc-as-code (como o próprio Onion) não têm.
   **Ação:** adicionar ramo de descoberta "sem build/manifesto".
3. **Sem regra de precedência para evidência conflitante.** O QA exige "ancorar
   em código" mas não diz como resolver fontes que se contradizem.
   **Ação:** definir precedência (ex.: `CLAUDE.md` > docs legados).
4. **Modo não-interativo ausente.** A Fase 2 (≥10 perguntas) não é testável/
   executável em modo agente. **Ação:** documentar modo "infer-from-evidence".

### Inconsistências reais do repo descobertas pelo probe (follow-ups próprios)

5. **`CONTRIBUTING.md` ainda descreve "Onion v4.0 / onion-cli / Node ≥16"** —
   contradiz a identidade atual (framework template, sem CLI). Resíduo do plano
   v4.0 abandonado. **Follow-up:** atualizar CONTRIBUTING.md.
6. **`CLAUDE.md` desatualizado:** afirma "1 skill (`onion`)"; o repo tem **4
   skills**. **Follow-up:** sincronizar CLAUDE.md (ver Fase 4 deste plano).
7. **Análise canônica não commitada:** `CLAUDE.md` cita
   `docs/analysis/onion-review-2026-05.md` mas o arquivo está **untracked** (não
   apareceu no worktree em HEAD). **Follow-up:** commitar os artefatos do
   saneamento (vários `??` em `git status`).

## Veredito

`build-index`, `build-tech-docs` e `build-business-docs` (por prontidão) estão
**aptos para projeto-alvo, com ressalvas não-bloqueantes** (gaps 1-4). T3.6 fica
**resolvido** via auto-piloto. `build-compliance-docs` permanece **pendente de
piloto regulado** (gap conhecido, não bloqueia uso geral).

Os achados 5-7 são dívidas de documentação do repo-mãe, parcialmente endereçadas
na Fase 4 deste plano (sincronizar CLAUDE.md/INDEX) — os demais ficam como
follow-up dedicado.
