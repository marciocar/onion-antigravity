---
description: Análise estruturada de problemas complexos com template oficial — use para análises críticas, migrações, arquitetura, performance ou segurança.
---

# 🔬 Análise de Problema Complexo

Criação de análises estruturadas usando template oficial.

Entrada do usuário: o texto após o comando — a descrição do problema/sistema a
analisar e, opcionalmente, o tipo (`critical` | `implementation` | `migration` |
`architecture` | `performance` | `security`).

## 🎯 Objetivo

Facilitar análises abrangentes para problemas complexos.

## ⚡ Fluxo de Execução

### Passo 1: Identificar Contexto

Analisar o problema para determinar o tipo:

| Tipo | Indicadores |
|------|-------------|
| `critical` | Bug crítico, sistema down |
| `implementation` | Nova feature, integração |
| `migration` | Upgrade, mudança de stack |
| `architecture` | Design, refatoração |
| `performance` | Lentidão, otimização |
| `security` | Vulnerabilidade, audit |

### Passo 2: Coletar Dados

- **Código**: buscar contexto no codebase e na estrutura relacionada.
- **Sistema**: inspecionar logs (`ERROR`/`WARNING`) e métricas relevantes.
- **Documentação**: ler docs existentes e `README.md`.

### Passo 3: Preencher Template

Criar documento de análise:

```markdown
# Análise: [problema]

## 📋 Resumo Executivo
- **Tipo**: [tipo detectado]
- **Severidade**: [alta/média/baixa]
- **Impacto**: [descrição]
- **Prazo**: [urgente/planejado]

## 🔍 Contexto
[Descrição do problema e contexto]

## 📊 Dados Coletados
[Evidências e métricas]

## 🎯 Análise
### Causa Raiz
[Identificação da causa]

### Impacto
[Áreas afetadas]

### Riscos
[Riscos identificados]

## 💡 Recomendações
### Opção 1: [Nome]
- Prós / Contras / Esforço

### Opção 2: [Nome]
- Prós / Contras / Esforço

## ✅ Decisão
[Recomendação final]

## 📋 Próximos Passos
1. [Ação 1]
2. [Ação 2]
3. [Ação 3]
```

### Passo 4: Validar

SE análise de arquitetura:
- Consultar `@metaspec-gate-keeper`
- Validar alinhamento com meta-specs

### Passo 5: Salvar

Salvar a análise em `docs/analysis/[slug]-analysis.md` (criar o diretório se necessário).

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ ANÁLISE CONCLUÍDA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 Problema: [problema]

📋 Resumo:
∟ Tipo: Architecture
∟ Severidade: Média
∟ Causa: [identificada]
∟ Recomendação: [opção escolhida]

📁 Documento:
∟ docs/analysis/[slug]-analysis.md

🚀 Próximos Passos:
∟ Revisar com stakeholders
∟ Criar tasks no task manager ativo
∟ Iniciar implementação
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Validação: `@metaspec-gate-keeper`

## ⚠️ Notas

- Tempo médio: 10-30 minutos
- Sempre validar decisões com stakeholders
