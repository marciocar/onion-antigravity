---
description: Cria novos workflows Antigravity (.agents/workflows/) com análise de contexto, seguindo os padrões do Sistema Onion.
---

# 📝 Criar Workflow Antigravity

Facilitador para criação de workflows seguindo padrões Onion v3.0. No Antigravity, os workflows ficam em `.agents/workflows/`.

Entrada do usuário: o texto após o comando — `command_name` (kebab-case,
obrigatório) e, opcionalmente, `category` e uma descrição do que o workflow faz.

## 🎯 Objetivo

Criar workflows que se integram ao ecossistema existente.

## ⚡ Fluxo de Execução

### Passo 1: Análise de Contexto

- Listar workflows existentes em `.agents/workflows/`.
- Verificar se a categoria (prefixo do nome) já é usada.
- Verificar duplicação do nome.

### Passo 2: Determinar Categoria

SE a categoria foi fornecida → usar diretamente.
SENÃO → inferir do propósito:

| Propósito | Categoria |
|-----------|-----------|
| Desenvolvimento, código | `engineer` |
| Tasks, specs, features | `product` |
| Git, branches, PRs | `git` |
| Documentação | `docs` |
| Comandos, agentes | `meta` |
| Validações | `validate` |

> Convenção Antigravity: workflows são flat com categoria como prefixo, ex.: `engineer-start`, `product-task`, `meta-create-command`.

### Passo 3: Gerar Estrutura

Workflows do Antigravity usam **apenas** o campo `description` no frontmatter
(uma frase clara, ≤ 200 chars). Parâmetros são descritos em prosa no corpo.

```markdown
---
description: [Uma frase clara descrevendo o que o workflow faz, ≤ 200 chars]
---

# [Título do Workflow]

[Descrição breve]. Entrada do usuário: o texto após o comando.

## 🎯 Objetivo
[O que este workflow faz]

## ⚡ Fluxo de Execução
### Passo 1: [Nome]
### Passo 2: [Nome]

## 📤 Output Esperado
[Formato de saída]

## 🔗 Referências
- [Referências relevantes]

## ⚠️ Notas
- [Notas importantes]
```

### Passo 4: Validações Obrigatórias

**CRÍTICO**: Executar TODAS as validações antes de criar:

1. **Duplicação** — o nome não pode existir em `.agents/workflows/`. Se existir → abortar.
2. **Formato** — kebab-case (`^[a-z][a-z0-9]*(-[a-z0-9]+)*$`).
3. **Categoria** — válida: `engineer | product | git | docs | meta | validate | quick | general`.

**Checklist de Validação:**
- [ ] Nome único (não existe em `.agents/workflows/`)
- [ ] Nome em kebab-case válido (com prefixo de categoria)
- [ ] Categoria válida
- [ ] Frontmatter contém apenas `description`
- [ ] < 400 linhas
- [ ] Seções obrigatórias (Objetivo, Fluxo, Output)

### Passo 5: Criar Arquivo

Criar `.agents/workflows/{{category}}-{{command_name}}.md`.

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ WORKFLOW CRIADO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📁 Arquivo: .agents/workflows/{{category}}-{{command_name}}.md

📋 Detalhes:
∟ Nome: {{command_name}}
∟ Categoria: {{category}}
∟ Linhas: ~150

🚀 Para usar: /{{category}}-{{command_name}}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Persona: `@command-creator-specialist`
- Skills do Antigravity: `.agents/skills/`

## ⚠️ Notas

- Máximo 400 linhas por workflow
- Frontmatter de workflow usa apenas `description`
- Sempre validar duplicação antes de criar
