---
description: Health check completo da documentação do projeto com score, gaps e recomendações de qualidade.
---

# 🏥 Docs Health Check

Verificação abrangente de saúde da documentação.

Entrada do usuário: o texto após o comando. Parâmetro `path` (opcional — caminho para analisar, padrão `docs/`).

## 🎯 Objetivo

Fornecer diagnóstico completo com score, gaps e recomendações.

## ⚡ Fluxo de Execução

### Passo 1: Coletar Métricas

Contar arquivos de docs no `path`, inspecionar a estrutura de diretórios e listar arquivos grandes (>500 linhas).

### Passo 2: Analisar Estrutura

#### Checklist de Estrutura
- [ ] README.md existe
- [ ] Índice/navegação presente
- [ ] Pastas organizadas por categoria
- [ ] Naming consistente (kebab-case)

### Passo 3: Avaliar Qualidade

| Critério | Peso | Verificação |
|----------|------|-------------|
| Completude | 25% | Seções obrigatórias |
| Consistência | 20% | Formatação uniforme |
| Atualidade | 20% | Datas de update |
| Links | 15% | Links funcionais |
| Exemplos | 10% | Código de exemplo |
| TOC | 10% | Table of contents |

### Passo 4: Identificar Gaps

Identificar arquivos sem update recente (>90 dias) e arquivos muito pequenos (<50 linhas).

### Passo 5: Calcular Score

```
Score = (Estrutura × 0.25) + (Qualidade × 0.25) +
        (Cobertura × 0.25) + (Atualidade × 0.25)
```

| Score | Status |
|-------|--------|
| 90-100 | 🟢 Excelente |
| 70-89 | 🟡 Bom |
| 50-69 | 🟠 Regular |
| <50 | 🔴 Crítico |

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🏥 DOCS HEALTH REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 Score Geral: 78/100 🟡

📈 Métricas:
∟ Arquivos: 45
∟ Linhas: 12,450
∟ Cobertura: 72%

✅ Pontos Fortes:
∟ Estrutura organizada
∟ README completo
∟ Exemplos presentes

⚠️ Gaps Identificados:
∟ 5 arquivos sem atualização >90d
∟ API docs incompleta
∟ Falta troubleshooting

💡 Recomendações:
1. Atualizar docs de API
2. Adicionar seção troubleshooting
3. Revisar arquivos antigos

🎯 Meta: 85/100 (próximo quarter)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Validação: `/docs-validate-docs`
- Persona: `@system-documentation-orchestrator`

## ⚠️ Notas

- Executar mensalmente para tracking
- Score histórico pode ser preservado em Artifacts do Antigravity ou em `docs/sessions/`
