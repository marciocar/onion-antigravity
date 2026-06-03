# Regra: Padrões de Idioma e Documentação

Aplica-se a todo artefato textual (código, commits, comentários, docs, mensagens).

## Inglês (en-US) — SEMPRE
- Nomes de **variáveis, funções, classes, interfaces, types**
- Nomes de **arquivos e diretórios** (kebab-case: `user-profile.tsx`)
- **Nomes de branches** Git (`feature/user-dashboard`, `fix/auth-bug`)
- **Documentação técnica de API** (schemas, endpoints, response shapes)
- **Logs e debugging** (mensagens técnicas internas)

## Commits — Conventional Commits, descrição em pt-BR
Tipo/escopo em inglês, descrição em português: `feat(auth): adiciona login via OAuth`,
`fix: corrige cálculo de story points`, `chore: remove dados de empresa do framework`.

## Português brasileiro (pt-BR) — SEMPRE
- **Comentários no código** (inline e JSDoc)
- **Respostas e explicações** do assistente IA
- **Documentação de processos e workflows**, READMEs e guias
- **Mensagens de erro** para usuário final
- **Logs de aplicação** voltados para debugging

## Quick Reference
| Contexto | Idioma | Exemplo |
|----------|--------|---------|
| Variáveis, funções, classes | EN | `getUserProfile()` |
| Comentários no código | PT-BR | `// Busca perfil do usuário` |
| Commits | EN | `fix: resolve auth bug` |
| Branches | EN | `feature/payment-flow` |
| Documentação técnica | PT-BR | `## Instalação` |
| Respostas do assistente | PT-BR | `Vou criar o componente...` |
| Nomes de arquivos | EN | `user-profile.tsx` |
| Mensagens de erro UI | PT-BR | `throw new Error('Usuário não encontrado')` |

## Configurações (.env)
- **NUNCA** commitar `.env` com valores sensíveis (está no `.gitignore`)
- **SEMPRE** manter `.env.example` atualizado com placeholders
- Prefixos por integração: `CLICKUP_`, `JIRA_`, `GITHUB_`, `GAMMA_`, `POSTGRES_`
- Workflows e personas devem **funcionar sem integrações** quando possível
- Se integração não configurada: avisar e sugerir o workflow `/meta-setup-integration`

## Gotchas
- Tipo do commit em inglês, descrição em pt-BR — rever antes do `git commit`
- Nomes de arquivos em pt-BR quebram convenções de framework — sempre EN
- `Error: invalid user ID` (técnico, EN) vs `Usuário não encontrado` (UX, pt-BR)

## Sintaxes oficiais
**NUNCA inventar sintaxe.** Consultar documentação oficial da versão em uso e os
especialistas (`@engineer` / subagents de frontend/backend) antes de implementar.
