---
description: Design de arquitetura leve para uma feature — use após refinar requisitos, para mapear componentes, padrões e trade-offs antes de implementar.
---

# Arquitetura Leve de Feature

Workflow para disparar o design de arquitetura leve de uma funcionalidade.
Entrada do usuário: o `<feature-slug>` (texto após o comando).

## Análise

1. Passe pelos cards/tasks, pais e filhos se necessário, e construa um entendimento
   inicial do que precisa ser construído. Pense com cuidado e certifique-se de entender:
   - Por que isso está sendo construído (contexto)
   - Qual o resultado esperado para esta task (objetivo)
   - Como deve ser construído, direcionalmente (abordagem)
   - Se requer novas APIs/ferramentas — você as entende?
   - Como deve ser testado?
   - Quais são as dependências e restrições?

2. Elabore os 3-5 esclarecimentos mais importantes para completar a tarefa.

3. Pergunte ao humano essas questões, fornecendo seu entendimento e sugestões.
   PAUSE para aguardar as respostas.

4. Considere se precisa de mais perguntas. Se sim, pergunte e PAUSE novamente.

5. Quando tiver bom entendimento, declare-o claramente ao humano para revisão
   (use um Artifact do Antigravity para facilitar a revisão).

6. Se o humano concordar, prossiga. Caso contrário, itere até aprovação explícita.

7. Se algo discutido afeta os requisitos escritos, peça permissão ao humano para
   editá-los (mudanças estruturais) ou adicionar comentários (esclarecimentos).
   Se o requisito está numa task do Task Manager ativo, atualize a task —
   ver regra `task-manager-routing`.

8. Não prossiga sem o humano dar sinal verde explícito nesta fase.

## Arquitetura

Com o entendimento consolidado, desenvolva a arquitetura da funcionalidade — mapeando
o que será construído, componentes, dependências, padrões, tecnologias, restrições,
suposições, trade-offs, alternativas e consequências. Considere o melhor caminho
respeitando os padrões e best practices do projeto.

1. Passe pelo código-fonte relevante; entenda estrutura e propósito e localize os
   arquivos importantes para esta implementação.

2. Revise as meta specs técnicas do projeto para garantir alinhamento com a visão técnica.

3. Construa uma proposta de arquitetura alinhada aos padrões e best practices do projeto.

Dicas:
- Encontre arquivos específicos a partir das respostas de descoberta.
- Mergulhe em funcionalidades e padrões similares; analise detalhes de implementação.
- Use WebSearch ou MCP de documentação para best practices / docs de biblioteca (se necessário).

Seu documento de arquitetura deve incluir:
- Visão geral de alto nível do sistema (antes e depois da mudança)
- Componentes afetados e seus relacionamentos/dependências
- Padrões e best practices mantidos ou introduzidos
- Dependências externas usadas ou a serem adicionadas
- Restrições e suposições
- Trade-offs e alternativas
- Consequências negativas (se houver) ao implementar este design
- Lista dos principais arquivos a editar/criar

Se ajudar, construa um diagrama MERMAID.

4. Se surgirem dúvidas ou algo contradizer o entendimento anterior, peça esclarecimento ao humano.

5. Mostre ao usuário (como Artifact) e aguarde aprovação. Itere até estar pronto. PAUSE.

6. Quando o humano concordar, prossiga salvando os detalhes da arquitetura como
   comentário na task original do Task Manager ativo (ver regra `task-manager-routing`).

## Pesquisa

Se não tiver certeza de como uma biblioteca funciona, pesquise (MCP/documentação oficial) —
não tente adivinhar.

## Referências
- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
- Task Manager Abstraction: `docs/knowledge-base/concepts/task-manager-abstraction.md`
