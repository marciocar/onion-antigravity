---
description: Iniciar desenvolvimento de feature — cria contexto de sessão e analisa tasks. Suporta múltiplos task managers via TASK_MANAGER_PROVIDER.
---

# Engineer Start

Workflow para iniciar o desenvolvimento de uma funcionalidade. Entrada do usuário
(após o comando): o `<feature-slug>` e/ou um ou mais task IDs.

## 🔧 Pré-requisito: detectar provider
Antes de tudo, aplicar a regra `task-manager-routing`:
1. Ler `TASK_MANAGER_PROVIDER` no `.env`.
2. Se houver task ID na entrada, validar compatibilidade com o provider configurado.
3. Se incompatível: avisar e sugerir ajustar o `.env` ou rodar `/meta-setup-integration`.

## Configuração
- Se não estivermos em uma feature branch, peça permissão para criar uma (`/git-feature-start`).
- Se a branch atual corresponde ao nome da funcionalidade, seguir.
- Garantir que existe `docs/sessions/<feature-slug>/` (contexto versionado, complementar aos Artifacts do Antigravity).
- Pedir ao usuário o input desta sessão (um ou mais tasks).

## Análise
Analise as tasks (pais e filhos) e construa o entendimento inicial. Leia tasks via
o provider ativo (ClickUp/Jira/Asana/Linear) — ver regra `task-manager-routing`.
Documente provider, nome e URL da task no `context.md`.

### Validação de Story Points (recomendado)
Antes de iniciar, verificar se a task tem estimativa:
- **Sem estimativa** → recomendar `/product-estimate` antes de prosseguir.
- **> 13 pontos (épico)** → recomendar quebrar via `/product-refine`.
- **Válida** → seguir.

### Questões de análise
Entender com clareza: contexto (por quê), objetivo (resultado esperado), abordagem
(direcional), novas APIs/ferramentas, como testar, dependências e restrições.

Formule as 3-5 clarificações mais importantes e pergunte ao humano, fornecendo seu
entendimento e sugestões. Itere até aprovação explícita. Salve o entendimento em
`docs/sessions/<feature-slug>/context.md` e peça revisão.

## Arquitetura
Desenvolva a arquitetura da funcionalidade considerando padrões e best practices do
projeto. O documento deve incluir:
- Visão de alto nível (antes/depois)
- Componentes afetados e relações/dependências
- Padrões mantidos ou introduzidos
- Dependências externas, restrições e suposições
- Trade-offs e alternativas
- Lista dos principais arquivos a editar/criar

Salve em `docs/sessions/<feature-slug>/architecture.md` e peça revisão.

## Auto-update do Task Manager
Se o provider está configurado e há task ID:
- Atualizar status para `in_progress`.
- Adicionar comentário de início (formato conforme o provider — ver regra `task-manager-routing`).
- Criar **mapeamento fase→subtask** no `context.md` para uso pelo `/engineer-work`.

## Pesquisa
Em caso de dúvida sobre uma biblioteca, pesquise (MCP/documentação oficial) — não adivinhe.

## Próximo passo
`/engineer-work` para executar as fases planejadas.

## Referências
- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
- Task Manager Abstraction: `docs/knowledge-base/concepts/task-manager-abstraction.md`
