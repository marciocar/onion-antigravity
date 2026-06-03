---
description: Criar apresentações profissionais via Gamma.app a partir de um tema, task ou documento — use para pitch, product, technical, business ou report.
---

# Criação de Apresentações

Wrapper para criação de apresentações via @presentation-orchestrator.
Entrada do usuário: o `topic` (tema, task ID ou caminho de documento) e, opcionalmente,
o `type` (pitch/product/technical/business/report). Tudo após o comando.

## Objetivo

Criar apresentações profissionais no Gamma.app de forma automatizada.

## Fluxo de Execução

### Passo 1: Identificar Tipo de Input

Analisar o `topic` para determinar a fonte:

| Pattern | Tipo | Ação |
|---------|------|------|
| ID de task | Task ID | Buscar dados no Task Manager ativo (regra `task-manager-routing`) |
| `docs/...` | Documento | Ler arquivo |
| Texto livre | Tema | Pesquisar codebase |

### Passo 2: Detectar Tipo de Apresentação

SE `type` fornecido → usar diretamente. SENÃO → inferir do contexto:

| Contexto | Tipo Inferido |
|----------|---------------|
| Investidores, funding | `pitch` |
| Novo recurso, release | `product` |
| Arquitetura, API | `technical` |
| Resultados, métricas | `business` |
| Status, progresso | `report` |

### Passo 3: Coletar Dados

- **Task** → buscar nome, descrição, subtasks/progresso, comentários e métricas via o
  Task Manager ativo (regra `task-manager-routing`).
- **Documento** → ler arquivo e extrair título, resumo, pontos principais, dados, conclusões.
- **Tema geral** → pesquisar no codebase: arquivos relacionados, documentação, README e specs.

### Passo 4: Estruturar Narrativa

Delegar para @storytelling-business-specialist para definir audiência-alvo, objetivo,
arco narrativo e pontos-chave (5-10).

### Passo 5: Gerar Apresentação

Invocar @presentation-orchestrator com `topic`, `type`, `audience`, `key_points` e `data`.

### Passo 6: Entregar Resultado

## Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ APRESENTAÇÃO CRIADA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 [Título da Apresentação]
📋 Detalhes: Tipo · Slides · Audiência · Fonte
🎨 Assets: Gamma (URL) · PDF · Outline
🚀 Próximos Passos: revisar conteúdo · ajustar design · compartilhar
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Estrutura por Tipo

| Tipo | Slides | Estrutura |
|------|--------|-----------|
| `pitch` | 10-15 | Problema → Solução → Mercado → Tração → Ask |
| `product` | 8-12 | Contexto → Feature → Demo → Benefícios → CTA |
| `technical` | 12-20 | Arquitetura → Componentes → Fluxos → API |
| `business` | 10-15 | Contexto → Resultados → Análise → Próximos |
| `report` | 5-10 | Status → Progresso → Bloqueios → Timeline |

## Referências
- Agente principal: @presentation-orchestrator
- Storytelling: @storytelling-business-specialist
- API técnica: @gamma-api-specialist
- Personas em `.agents/AGENTS.md`

## Notas
- Requer Gamma.app configurado (`GAMMA_API_KEY`). Para config: `/meta-setup-integration gamma`.
- Tempo médio: 2-5 minutos por apresentação.
