---
description: Adiciona todas as mudanças e faz commit rápido — uso típico no fluxo do Sistema Onion.
---

# Fast Commit

Adiciona todas as mudanças e faz commit com a mensagem especificada. Entrada do
usuário: o texto após o comando (a mensagem de commit).

## 🎯 Uso

```bash
/git-fast-commit "feat: implement admin dashboard basic flow"
```

## ⚡ Fluxo de Execução

1. `git add .` — adiciona todas as mudanças
2. `git commit -m "<mensagem>"` — commit com a mensagem

## 📋 Convenção de Mensagens

Use [Conventional Commits](https://conventionalcommits.org):

| Tipo | Descrição |
|------|-----------|
| `feat:` | Nova funcionalidade |
| `fix:` | Correção de bug |
| `docs:` | Documentação |
| `refactor:` | Refatoração |
| `chore:` | Manutenção |

## ⚠️ Notas

- SEMPRE revise `git status` antes
- Prefira commits atômicos e descritivos
