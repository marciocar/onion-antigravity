---
description: Consolida múltiplas reuniões via @meeting-consolidator, identificando divergências, convergências, insights e pontos não ditos. Use para transformar várias reuniões em conhecimento estratégico consolidado.
---

# 🔄 Consolidar Reuniões

Workflow para consolidar múltiplas reuniões usando o Consolidador de Reuniões
(`@meeting-consolidator`).

Entrada do usuário: o texto após o comando — uma pasta com reuniões ou um/mais
arquivo(s) de reunião (`source`) e, opcionalmente, um foco (`focus`):
`all` (padrão) | `divergences` | `convergences` | `insights` | `gaps`.

## 🎯 Objetivo

Transformar múltiplas reuniões em conhecimento estratégico consolidado, identificando:
- **Divergências** — conflitos e desalinhamentos entre participantes/reuniões
- **Convergências** — pontos de alinhamento e consenso
- **Insights estratégicos** — padrões não explícitos e conexões importantes
- **Pontos não ditos** — assuntos que deveriam ter sido mencionados mas não foram
- **Pontos não compreendidos** — decisões/ideias que parecem não ter sido entendidas

## ⚡ Fluxo de Execução

### Passo 1: Detectar tipo de entrada
Analisar o `source` fornecido — pasta, arquivo único ou múltiplos arquivos.

### Passo 2: Coletar arquivos de reunião
- **Pasta:** listar arquivos, filtrar por extensões relevantes (`.md`, `.txt`,
  `.transcript`, `.json`) e padrões de nome (*meeting*, *reunion*, *transcript*,
  *extract*), ordenar por data, validar.
- **Arquivo(s):** validar existência e que são reuniões válidas.

### Passo 3: Preparar contexto para o consolidador
Estruturar: lista de arquivos (com paths), foco da análise e metadados disponíveis
(nome, data, participantes, tipo de reunião, duração).

### Passo 4: Invocar o Consolidador de Reuniões

```markdown
@meeting-consolidator

Consolidar as seguintes reuniões:
**Arquivos:** {{lista_com_paths}}
**Foco:** {{focus}}
**Metadados:** {{metadados_estruturados}}

Execute consolidação completa: classificação por tema, divergências, convergências,
insights estratégicos não explícitos, pontos não ditos/não compreendidos, principais
pontos de atenção e recomendações estratégicas. Saída estruturada conforme template.
```

Se um foco específico for fornecido, restringir a saída: `divergences` (só conflitos),
`convergences` (só alinhamentos), `insights` (só insights), `gaps` (só pontos não
ditos/compreendidos).

### Passo 5: Validar output do consolidador
Verificar presença de: classificação por tema · divergências · convergências · insights
estratégicos · sessão exclusiva de pontos não ditos/compreendidos · pontos de atenção
priorizados · recomendações estratégicas.

### Passo 6: Salvar consolidação
Salvar em `docs/meet/consolidation-[data]-[tema].md` com cabeçalho (data da consolidação,
nº de reuniões, período, participantes) seguido do conteúdo consolidado.

## 📤 Output Esperado

Consolidação completa com: classificação por tema · divergências (com recomendações) ·
convergências (para capitalizar) · insights estratégicos · sessão exclusiva de pontos não
ditos/compreendidos · pontos de atenção priorizados · recomendações acionáveis.

## ⚠️ Regras Críticas

1. Sempre validar arquivos antes de processar.
2. Sempre coletar metadados quando disponíveis.
3. Sempre identificar o foco se especificado.
4. Sempre salvar o output em local apropriado.
5. Sempre incluir sessão exclusiva com pontos não ditos/compreendidos.

## Quando Usar

Quando há múltiplas reuniões sobre o mesmo tema; necessita identificar padrões,
divergências ou convergências; quer insights estratégicos não explícitos; precisa
identificar pontos não ditos/não compreendidos; ou quer visão consolidada de várias
discussões.

## 🎯 Checklist de Validação

- [ ] Arquivos identificados e validados
- [ ] Metadados coletados quando disponíveis
- [ ] Classificação por tema realizada
- [ ] Divergências e convergências identificadas
- [ ] Insights estratégicos gerados
- [ ] Pontos não ditos/compreendidos identificados
- [ ] Pontos de atenção priorizados e recomendações fornecidas
- [ ] Output salvo em local apropriado

## 🔗 Referências

- **Agente:** `@meeting-consolidator`
- **Relacionado:** `/product-extract-meeting` (extração estruturada)
- **Knowledge Base:** `docs/knowledge-base/concepts/meeting-transcription-to-knowledge-base.md`
- **Personas/subagentes:** `.agents/AGENTS.md`
