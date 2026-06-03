---
description: Validação abrangente de completude, consistência, estrutura, links e padrões da documentação do projeto.
---

# ✅ Validar Documentação

Validação abrangente de estrutura e qualidade de docs.

Entrada do usuário: o texto após o comando. Parâmetros (como prosa): `path` (opcional — caminho a validar, padrão `docs/`) e `fix` (opcional — corrigir problemas de formatação automaticamente, padrão `false`).

## 🎯 Objetivo

Verificar completude, consistência e padrões da documentação.

## ⚡ Fluxo de Execução

### Passo 1: Validar Estrutura

Verificar arquivos obrigatórios (`README.md`, `CHANGELOG.md`) e inspecionar a hierarquia de diretórios sob o `path`.

#### Checklist de Estrutura
- [ ] `README.md` na raiz
- [ ] `docs/` com índice
- [ ] Naming em kebab-case
- [ ] Extensão `.md`

### Passo 2: Validar Conteúdo

Para cada arquivo `.md`: confirmar presença de header (`#` na primeira linha) e sinalizar arquivos muito curtos (menos de 10 linhas).

#### Seções Obrigatórias

| Tipo de Doc | Seções Requeridas |
|-------------|-------------------|
| README | Objetivo, Uso, Instalação |
| API | Endpoints, Exemplos |
| Guide | Pré-requisitos, Steps |
| Spec | Objetivo, Requisitos |

### Passo 3: Validar Links

Extrair links internos Markdown para arquivos `.md` e verificar se cada destino existe; reportar links quebrados.

### Passo 4: Gerar Relatório

## 📤 Output Esperado

### Sem Erros

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ VALIDAÇÃO CONCLUÍDA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 Resumo:
∟ Arquivos: 45
∟ Erros: 0
∟ Avisos: 3

✅ Estrutura: OK
✅ Conteúdo: OK
✅ Links: OK

⚠️ Avisos:
∟ 3 arquivos sem update >30d
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Com Erros

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
❌ ERROS ENCONTRADOS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 Resumo:
∟ Arquivos: 45
∟ Erros: 5
∟ Avisos: 8

❌ Erros:
∟ docs/api.md: link quebrado (line 45)
∟ docs/guide.md: sem header
∟ README.md: seção "Uso" ausente

⚠️ Avisos:
∟ 8 arquivos muito curtos (<50 linhas)

💡 Para corrigir: /docs-validate-docs fix="true"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Health check: `/docs-docs-health`
- Persona: `@system-documentation-orchestrator`

## ⚠️ Notas

- Executar antes de releases
- `fix="true"` corrige apenas formatação
- Links quebrados requerem correção manual
