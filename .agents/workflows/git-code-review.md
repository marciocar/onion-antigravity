---
description: Gerenciador de ChatGPT-CodeReview para setup, validação e otimização do code review automático em projetos.
---

# 🤖 ChatGPT Code Review Manager

Gerenciador inteligente de ChatGPT-CodeReview para o Sistema Onion. Entrada do
usuário: o texto após o comando (modo `auto` | `setup` | `validate` | `status`).

## 🎯 Objetivo

Automatizar setup, validação e otimização do code review automático.

## ⚡ Modos de Operação

```bash
/git-code-review           # AUTO: detecta e executa ação apropriada
/git-code-review setup     # Criar/reconfigurar arquivo
/git-code-review validate  # Validar configuração existente
/git-code-review status    # Mostrar status atual
```

## 🔄 Fluxo de Execução

### Passo 1: Detectar Modo

Verificar se `.github/workflows/code-review.yml` existe. Se existe → `validate`;
caso contrário → `setup`. Se o usuário informou um modo, usar o modo especificado.

### Passo 2: Executar Modo

#### 🆕 SETUP MODE

1. **Detectar Stack**
   - Package manager: `pnpm-lock.yaml` → pnpm; `package-lock.json` → npm
   - Monorepo: `nx.json` → nx
   - Backend/Frontend: procurar `fastify` / `react` no `package.json`

2. **Gerar Template** — criar `.github/workflows/code-review.yml`:

```yaml
name: ChatGPT Code Review
on:
  pull_request:
    types: [opened, synchronize]
jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: anc95/ChatGPT-CodeReview@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

3. **Aplicar Configurações por Stack**
   - TypeScript → adicionar regras de tipos
   - React → regras de hooks
   - NX → regras de monorepo

#### 🔍 VALIDATE MODE

1. **Verificar Arquivo**: existe? YAML válido? Secrets configurados?
2. **Analisar Configuração** usando o checklist `common/prompts/code-review-checklist.md`
3. **Gerar Relatório**:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 CODE REVIEW VALIDATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Arquivo: .github/workflows/code-review.yml
✅ YAML: Válido
⚠️ Secrets: OPENAI_API_KEY não detectado

💡 Recomendações:
∟ Configurar OPENAI_API_KEY em Settings > Secrets
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### 📊 STATUS MODE

Mostrar dashboard:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 CODE REVIEW STATUS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔧 Configuração:
∟ Arquivo: ✅ Configurado
∟ Stack: TypeScript + React + NX
∟ Última atualização: 2025-11-24

📈 Métricas (últimos 30 dias):
∟ PRs revisados: 45
∟ Issues detectados: 127
∟ Auto-fixes aplicados: 23

🎯 Saúde: 95% ████████████░░░░░
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Passo 3: Atualizar Task Manager

Se há task associada: adicionar comentário com resultado e atualizar status se necessário.

## 📤 Output Esperado

### Setup Sucesso

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ CODE REVIEW CONFIGURADO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📁 Criado: .github/workflows/code-review.yml

🔧 Stack Detectado:
∟ Package Manager: pnpm
∟ Monorepo: NX
∟ Backend: Fastify
∟ Frontend: React

⚠️ Próximos Passos:
1. Configurar OPENAI_API_KEY em Settings > Secrets
2. Testar com um PR de teste

🚀 Comando: /git-code-review validate
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Validação com Issues

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠️ CODE REVIEW - ISSUES ENCONTRADOS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔴 Críticos:
∟ Secret OPENAI_API_KEY não configurado

🟡 Importantes:
∟ Versão do action desatualizada (usar @v1.2.0)

💡 Sugestões:
∟ Adicionar filtro de arquivos .ts/.tsx

🔧 Auto-fix disponível? Sim
Executar auto-fix? (s/n)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Checklist: `common/prompts/code-review-checklist.md`
- Padrões Git: `common/prompts/git-workflow-patterns.md`
- Agente: @code-reviewer para reviews manuais

## ⚠️ Notas

- Requer GitHub Actions habilitado
- Secret `OPENAI_API_KEY` obrigatório
- Funciona com qualquer stack JavaScript/TypeScript
