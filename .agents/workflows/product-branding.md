---
description: Trabalha branding e posicionamento de marca via @branding-positioning-specialist usando docs/business-context/ como base. Use para posicionamento, brand guidelines, análise competitiva ou reposicionamento.
---

# Branding e Posicionamento de Marca

Workflow especializado para trabalhar Branding e Posicionamento de Marca usando o agente
`@branding-positioning-specialist`, com acesso ao contexto de negócio completo em
`docs/business-context/` para decisões fundamentadas.

Entrada do usuário: o texto após o comando (tipo de trabalho de branding desejado).
Tipos suportados: brand positioning statement · análise competitiva/matriz de
posicionamento · brand guidelines · estratégia de branding · análise de posicionamento
atual · rebranding/reposicionamento.

## Processo

### 1. Análise do Contexto de Negócio

**1.1. Carregar contexto** — ler arquivos relevantes em `docs/business-context/`:
`PRODUCT_STRATEGY.md`, `COMPETITIVE_LANDSCAPE.md`, `MESSAGING_FRAMEWORK.md`,
`CUSTOMER_PERSONAS.md`, `CUSTOMER_JOURNEY.md`, `VOICE_OF_CUSTOMER.md`, `SALES_PROCESS.md`,
`PRODUCT_METRICS.md`, `INDUSTRY_TRENDS.md`.

**1.2. Identificar informações relevantes** — proposta de valor atual, posicionamento
existente, público-alvo/personas, diferenciação competitiva, mensagens-chave, valores e
propósito da marca.

**1.3. Mapear gaps e oportunidades** — posicionamento vago, identidade inconsistente,
mensagens desalinhadas, ausência de brand guidelines, diferenciação não explorada.

### 2. Invocação do Agente Especializado

**2.1. Preparar contexto** — consolidar o business context em formato estruturado e
identificar o tipo de trabalho de branding necessário.

**2.2. Invocar `@branding-positioning-specialist`** — passar contexto consolidado,
objetivo do trabalho, público-alvo (personas), concorrentes (competitive landscape),
proposta de valor, mensagens existentes, valores e propósito.

**2.3. Colaborar com agentes relacionados (quando necessário):**
- `@storytelling-business-specialist` — narrativa de marca
- `@product-agent` — alinhamento estratégico
- `@research-agent` — pesquisa adicional de mercado

### 3. Desenvolvimento de Branding

Conforme o escopo solicitado, desenvolver um ou mais artefatos:

- **Brand Positioning Statement** — público-alvo, categoria de mercado, diferenciação,
  proof points; validar alinhamento com estratégia de produto.
- **Análise competitiva e matriz de posicionamento** — concorrentes do competitive
  landscape, dimensões relevantes, oportunidade única; validar viabilidade.
- **Brand Guidelines** — identidade visual e verbal (logo, paleta, tipografia, tom de
  voz alinhado ao messaging framework, elementos gráficos) + manual de aplicação.
- **Estratégia de experiência** — mapear pontos de contato, garantir consistência
  omnicanal, personalização e storytelling framework.

### 4. Integração com Contexto Existente

**4.1. Atualizar documentos de negócio** — `MESSAGING_FRAMEWORK.md` (positioning
statement, posicionamento, guidelines) e `COMPETITIVE_LANDSCAPE.md` (análise competitiva,
matriz, diferenciação).

**4.2. Validar consistência** — alinhar branding com estratégia de produto, mensagens,
personas, valores; resolver inconsistências.

**4.3. Documentar decisões** — manter histórico de evolução dos artefatos de branding.

### 5. Validação e Próximos Passos

**5.1. Revisar output** — verificar que o branding é fundamentado, diferenciado,
sustentável, alinhado e acionável.

**5.2. Identificar próximos passos** — implementar identidade visual, aplicar em
materiais, treinar equipe, validar com público-alvo, medir impacto.

## Guidelines

### ✅ Boas Práticas
- Sempre consultar business context antes de desenvolver branding.
- Usar personas para definir público-alvo; aproveitar análise competitiva existente.
- Alinhar com proposta de valor e estratégia de produto; considerar mensagens existentes.
- Criar posicionamento específico, diferenciado e sustentável; documentar decisões.
- Atualizar documentos de negócio e manter consistência entre branding e mensagens.

### ⚠️ Atenções
- Se o business context não existir, solicitar criação primeiro (`/docs-build-business-docs`).
- Validar atualidade das informações; não assumir dados não documentados.
- Garantir diferenciação e sustentabilidade do posicionamento; considerar evolução futura.
- Validar com stakeholders e público-alvo antes de implementar; medir impacto.

### ❌ Evitar
- Desenvolver branding sem contexto ou desconectado da estratégia.
- Posicionamento genérico ou em espaço já ocupado por concorrentes.
- Branding que contradiz mensagens/valores existentes ou guidelines não aplicáveis.
- Implementar sem validação; mudanças radicais sem justificativa; não documentar decisões.

## Checklist de Validação

- [ ] Business context consultado, informações extraídas, gaps identificados
- [ ] Posicionamento específico, diferenciado, sustentável e alinhado
- [ ] Documentos de negócio atualizados e consistência validada
- [ ] Decisões documentadas; próximos passos e métricas definidos

## Troubleshooting

- **Business context não existe** → executar `/docs-build-business-docs` primeiro.
- **Informações conflitantes** → identificar conflitos, consultar stakeholders, atualizar
  documentos antes de prosseguir.
- **Posicionamento não diferenciado** → revisar análise competitiva, identificar atributos
  únicos, ajustar.
- **Branding desalinhado com estratégia** → revisar estratégia de produto, ajustar e
  validar com stakeholders.

## Comandos Relacionados

- `/product-spec` — especificar features de produto
- `/product-task` — criar tasks de desenvolvimento
- `/docs-build-business-docs` — construir contexto de negócio

## Referências

- **Agente:** `@branding-positioning-specialist`
- **Relacionados:** `@storytelling-business-specialist`, `@product-agent`, `@research-agent`
- **Personas/subagentes:** `.agents/AGENTS.md`
