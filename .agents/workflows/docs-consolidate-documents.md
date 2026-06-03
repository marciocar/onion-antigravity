---
description: Consolidar múltiplos documentos identificando divergências, convergências, insights estratégicos e gaps num documento unificado.
---

# 📚 Consolidar Documentos

Workflow para consolidar múltiplos documentos relacionados, identificando padrões, divergências, convergências e insights estratégicos.

Entrada do usuário: o texto após o comando. Parâmetros (como prosa): `source` (obrigatório — pasta ou arquivo(s) a consolidar), `focus` (opcional — `all|divergences|convergences|insights|gaps|structure`) e `output_path` (opcional — padrão `docs/consolidated/`).

## 🎯 Objetivo

Transformar múltiplos documentos em conhecimento estratégico consolidado, identificando:
- **Divergências**: conflitos e desalinhamentos entre documentos
- **Convergências**: pontos de alinhamento e consistência
- **Insights Estratégicos**: padrões não explícitos e conexões importantes
- **Gaps de Informação**: informações faltantes ou incompletas
- **Estrutura Otimizada**: organização ideal do conhecimento consolidado
- **Inconsistências**: dados ou informações contraditórias

## ⚡ Fluxo de Execução

### Passo 1: Detectar Tipo de Entrada

Analisar o `source` fornecido — pode ser uma pasta, um arquivo único ou múltiplos arquivos separados por espaço.

**Se for pasta:**
- Listar arquivos de documentos na pasta
- Filtrar por extensões relevantes (.md, .txt, .json, .yaml, etc.)
- Ordenar por data de modificação ou nome
- Identificar documentos relacionados por tema

**Se for arquivo(s):**
- Processar arquivo(s) diretamente
- Validar que são documentos válidos
- Identificar tipo e categoria de cada documento

### Passo 2: Coletar Arquivos de Documentos

**Cenário A — Pasta fornecida:**
1. Listar arquivos na pasta (recursivo se necessário)
2. Filtrar arquivos de documentos (extensões `.md`, `.txt`, `.json`, `.yaml`, `.yml`, `.rst`, `.adoc`; padrões de nome `*docs*`, `*documentation*`, `*spec*`, `*guide*`)
3. Ordenar por data de modificação (mais recente primeiro) ou nome
4. Validar que são documentos válidos
5. Identificar documentos relacionados por tema, categoria (business-context, tech-docs, etc.) e tipo (spec, guide, reference)
6. Coletar lista de arquivos para processar

**Cenário B — Arquivo(s) fornecido(s):**
1. Validar que arquivo(s) existe(m)
2. Verificar se são documentos válidos
3. Identificar tipo e categoria de cada documento
4. Coletar lista de arquivos para processar

### Passo 3: Preparar Contexto para Consolidação

Antes de processar, montar contexto estruturado com: arquivos a consolidar (com paths), foco da análise e metadados dos documentos.

**Metadados a coletar:** nome e caminho completo, data de modificação, tamanho, tipo de documento, categoria, tema principal, estrutura (seções principais), referências cruzadas.

### Passo 4: Analisar e Consolidar Documentos

Processar os documentos para produzir consolidação completa, cobrindo:

1. **Análise de Conteúdo**: tema principal e secundários, informações-chave, estrutura/organização
2. **Divergências**: conflitos de informação, dados contraditórios, desalinhamentos conceituais, versões diferentes da mesma informação
3. **Convergências**: pontos de alinhamento, informações consistentes, padrões recorrentes, consenso
4. **Insights Estratégicos**: padrões não explícitos, conexões importantes, oportunidades de melhoria, gaps de conhecimento
5. **Gaps**: informações faltantes, documentos incompletos, referências quebradas, áreas não cobertas
6. **Estrutura Otimizada**: organização ideal, hierarquia, relacionamentos entre conceitos, fluxo lógico de leitura
7. **Principais Pontos de Atenção**: inconsistências críticas, informações desatualizadas, conflitos a resolver, priorização por criticidade
8. **Recomendações Estratégicas**: ações para resolver divergências, oportunidades de consolidação, melhorias, próximos passos

**Se foco específico for fornecido**, restrinja a análise:
- `divergences`: apenas divergências e conflitos
- `convergences`: apenas convergências e alinhamentos
- `insights`: apenas insights estratégicos
- `gaps`: apenas gaps de informação
- `structure`: foco em estrutura e organização otimizada

### Passo 5: Validar Output da Consolidação

Verificar que a consolidação contém: análise de conteúdo, divergências, convergências, insights estratégicos, gaps, estrutura otimizada, pontos de atenção priorizados e recomendações estratégicas.

### Passo 6: Salvar Consolidação

Determinar o caminho de saída (`output_path` ou padrão `docs/consolidated/`). Nomear o arquivo como `consolidation-<YYYY-MM-DD>-<tema>.md` e incluir cabeçalho com: data da consolidação, número de documentos consolidados, lista de arquivos originais e período (data mais antiga → mais recente).

## 📤 Output Esperado

O workflow deve produzir:
1. Consolidação completa seguindo template estruturado
2. Análise de conteúdo com tema principal
3. Divergências com recomendações de resolução
4. Convergências para capitalizar consistência
5. Insights estratégicos não explícitos
6. Gaps de informação
7. Estrutura otimizada proposta
8. Pontos de atenção priorizados
9. Recomendações estratégicas acionáveis
10. Documento consolidado salvo em local apropriado

## 🔗 Referências

- Workflow relacionado: `/product-consolidate-meetings` (consolidação de reuniões)
- Workflows de documentação: `/docs-build-tech-docs`, `/docs-build-business-docs`
- Validação: `/docs-validate-docs`

## ⚠️ Notas Importantes

### Regras Críticas
1. Sempre validar arquivos antes de processar
2. Sempre coletar metadados quando disponíveis
3. Sempre identificar foco se especificado
4. Sempre salvar output em local apropriado
5. Sempre preservar referências aos documentos originais
6. Sempre identificar divergências críticas
7. Sempre propor estrutura otimizada

### Quando Usar
- Há múltiplos documentos sobre o mesmo tema
- Necessita identificar padrões, divergências ou convergências
- Precisa de insights estratégicos não explícitos
- Quer identificar gaps de informação
- Necessita visão consolidada ou documento unificado a partir de múltiplas fontes
- Precisa identificar inconsistências entre documentos

### Exemplos de Uso
```bash
/docs-consolidate-documents "docs/business-context/"
/docs-consolidate-documents "docs/business-context/journey.md docs/business-context/personas.md"
/docs-consolidate-documents "docs/business-context/" --focus="divergences"
/docs-consolidate-documents "docs/business-context/" --focus="structure"
/docs-consolidate-documents "docs/business-context/" --output_path="docs/consolidated/business-context/"
```

### Focos Disponíveis

| Foco | Descrição |
|------|-----------|
| `all` | Consolidação completa (padrão) |
| `divergences` | Apenas divergências e conflitos |
| `convergences` | Apenas convergências e alinhamentos |
| `insights` | Apenas insights estratégicos |
| `gaps` | Apenas gaps de informação |
| `structure` | Apenas estrutura e organização otimizada |

### Estrutura do Documento Consolidado

```markdown
# Consolidação de Documentos: [Tema Principal]

**Data da Consolidação**: [data]
**Documentos Consolidados**: [número] documentos
**Arquivos Originais**: [lista]
**Período**: [data mais antiga] - [data mais recente]

## 📋 Índice
## 📊 Análise de Conteúdo
## ⚠️ Divergências Identificadas
## ✅ Convergências Identificadas
## 💡 Insights Estratégicos
## 🔍 Gaps de Informação
## 📐 Estrutura Otimizada
## 🎯 Principais Pontos de Atenção
## 💡 Recomendações Estratégicas
```

## 🎯 Checklist de Validação

- [ ] Arquivos identificados e validados
- [ ] Metadados coletados quando disponíveis
- [ ] Análise de conteúdo realizada
- [ ] Divergências identificadas
- [ ] Convergências identificadas
- [ ] Insights estratégicos gerados
- [ ] Gaps de informação identificados
- [ ] Estrutura otimizada proposta
- [ ] Pontos de atenção priorizados
- [ ] Recomendações estratégicas fornecidas
- [ ] Output salvo em local apropriado
- [ ] Referências aos documentos originais preservadas
