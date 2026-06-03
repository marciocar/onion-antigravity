---
description: Gerenciar e atualizar os índices de documentação do projeto, mantendo a estrutura navegável.
---

# 🗂️ Build Index — Índices de Documentação

Este workflow gerencia os índices de documentação do projeto, mantendo a estrutura organizada e navegável.

Entrada do usuário: o texto após o comando — opcionalmente o nome de uma seção a reconstruir.

**Estrutura de Documentação do projeto**:
```
docs/
├── INDEX.md                    # Índice principal (hub central)
├── business-context/           # Contexto de negócio
│   └── index.md
├── technical-context/          # Contexto técnico
│   └── index.md
├── compliance/                 # Compliance e Governança
│   ├── index.md
│   ├── security/               # ISO 27001
│   ├── business-continuity/    # ISO 22301
│   ├── soc2/                   # SOC2 Type II
│   ├── ai-governance/          # AI Governance
│   ├── privacy/                # LGPD
│   └── due-diligence/          # Due Diligence
├── onion/                      # Sistema Onion
├── guidelines/                 # Guidelines de desenvolvimento
└── files/                      # Recursos diversos

.agents/
├── workflows/                  # Workflows organizados por categoria
└── AGENTS.md                   # Personas e subagents especializados
```

## Uso

### `/docs-build-index`

**Sem argumentos**: Reconstrói o arquivo `INDEX.md` principal na pasta `docs/`.

Este índice central fornece:
- Visão geral do projeto
- Links para todas as seções de documentação
- Descrição de cada seção
- Estatísticas da documentação
- Guias de navegação por perfil (dev, PM, vendas, arquitetos, CISO/Compliance)
- Mapa de navegação rápida
- Referência às personas e subagents especializados em `.agents/AGENTS.md`
- Métricas de maturidade de compliance (ISO 27001, ISO 22301, SOC2, LGPD)

**Comportamento**:
1. Escaneia todas as pastas em `docs/`
2. Lê os arquivos `index.md` de cada seção
3. Escaneia `.agents/workflows/` e `.agents/AGENTS.md` para contar recursos
4. Extrai informações relevantes (título, descrição, arquivos principais)
5. Gera/atualiza `docs/INDEX.md` com estrutura completa
6. Mantém estatísticas atualizadas (arquivos markdown, workflows, personas/subagents, e contagens por seção)
7. Gera métricas de maturidade de compliance:
   - ISO 27001:2022
   - ISO 22301:2019
   - SOC2 Type II
   - LGPD
   - AI Governance

### `/docs-build-index <section-name>`

**Com argumento**: Reconstrói o índice de uma seção específica da documentação.

**Seções disponíveis**:
- `business-context` - Documentação de negócio
- `technical-context` - Documentação técnica
- `compliance` - Compliance e Governança (ISO 27001, ISO 22301, SOC2, LGPD)
- `onion` - Sistema Onion (workflows e personas)
- `guidelines` - Guidelines de desenvolvimento

**Comportamento**:
1. Percorre a estrutura de arquivos da seção especificada
2. Identifica arquivos principais e subpastas
3. Gera/atualiza o `index.md` da seção
4. Mantém links relativos corretos
5. Preserva estrutura e organização

**Exemplos**:
```bash
/docs-build-index business-context
# Reconstrói docs/business-context/index.md

/docs-build-index technical-context
# Reconstrói docs/technical-context/index.md

/docs-build-index compliance
# Reconstrói docs/compliance-context/index.md
```
