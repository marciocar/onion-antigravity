---
description: Cria um Pull Request com integração GitFlow e sync automático pós-merge. Use ao finalizar o trabalho de uma branch e abrir o PR.
---

# 🚀 Engineer PR — GitFlow Integrado

Workflow para **criação de Pull Requests** com integração ao `/git-sync` otimizado do
Sistema Onion.

## 🤖 Integração GitFlow
Este comando inclui **sync automático pós-merge** usando:
- **GitFlow Analysis** via `@gitflow-specialist`
- **Performance otimizada** (cache + operações paralelas)
- **Cleanup inteligente** baseado na estratégia de branch
- **Session archiving** automático
- **Task Manager auto-update** para status "Done" (no provider configurado em `TASK_MANAGER_PROVIDER`)

---

Siga estes passos cuidadosamente:

1. **Garanta que todos os testes passam** na branch atual. Execute a suíte apropriada para o projeto. Se algum teste falhar, corrija antes de prosseguir.

2. **CRÍTICO — Criar Feature Branch PRIMEIRO:**
   a. Crie uma feature branch a partir da branch base (develop/main):
      ```bash
      git checkout -b feature/[descricao-sucinta]
      git push -u origin feature/[descricao-sucinta]
      ```
   b. Faça commit das mudanças com uma mensagem clara e concisa.
   c. Push dos commits para a feature branch.

3. **Mova a task** associada para "in progress" e adicione a tag "under-review". Antes, aplique a regra `task-manager-routing`: carregue o `.env` e leia `TASK_MANAGER_PROVIDER` (`jira` | `clickup` | `asana` | `linear` | `none`). Se `none`, pule esta etapa (não há persistência remota).

4. **Adicione um comentário na task** documentando o PR. Roteamento por provider (carregar `.env` → ler `TASK_MANAGER_PROVIDER` → seguir o adapter):
   - **`clickup`** → comentário em formatação Unicode via `@clickup-specialist` (skill `clickup-patterns`).
   - **`jira`** → comentário em ADF via `@jira-specialist`.
   - **`asana`** → comentário (story) via `@task-specialist`.
   - **`linear`** → comentário em Markdown via `@task-specialist`.
   - **`none`** → não persistir comentário remoto.

   O comentário deve documentar: URL do PR, branch, descrição das mudanças e status dos testes (passing | review | pending). Detalhes dos adapters: regra `task-manager-routing` e `docs/knowledge-base/concepts/task-manager-abstraction.md`.

5. **Abra o Pull Request** com os detalhes da implementação.

   > Importante: não mencione código relacionado a IA ou assistentes de IA no PR.

6. Após abrir o PR, aguarde ~3 minutos e verifique comentários da ferramenta automatizada de code review. Se nenhum aparecer, aguarde mais ~3 minutos e verifique novamente.

7. Ao receber os comentários, analise cada um. Determine quais requerem correções e quais podem ser ignorados ou explicados. Apresente suas sugestões ao usuário e peça permissão para fazer as mudanças.

8. Para os comentários que requerem correção:
   a. Faça as mudanças necessárias no código.
   b. Commit com mensagem clara.
   c. Push do(s) novo(s) commit(s) para a mesma branch.

9. Após abordar os comentários e fazer push, aguarde a confirmação de merge do PR.

10. **Sync Automático Pós-Merge**: uma vez merged, execute automaticamente `/git-sync`, que inclui:
    - 🤖 GitFlow Analysis com `@gitflow-specialist`
    - ⚡ Performance otimizada (cache + operações paralelas)
    - 🧹 Cleanup inteligente baseado na estratégia GitFlow
    - 📁 Session management automático com archiving
    - 🔗 Task Manager auto-update para status "Done" (provider configurado em `TASK_MANAGER_PROVIDER`)

**REGRA DE OURO**: faça commit APENAS dos arquivos que você alterou. Se houver mais arquivos, pergunte ao usuário se devem ser incluídos. Não use `git add .` para evitar commits indevidos, a não ser que o usuário confirme.

## Mensagem final ao usuário

```
Tarefa completada:
- Testes estão passando
- Mudanças commitadas
- Task [TASK ID] movida para "in progress" com tag "under-review" no Task Manager configurado ([PROVIDER])
- PR aberto: [PR TITLE]
- Comentários do code review automatizado abordados e correções pushed
- 🤖 GitFlow integration: Auto-sync configurado para pós-merge

O PR está agora pronto para sua revisão final e merge manual.

🚀 APÓS O MERGE: O comando /git-sync será executado automaticamente com:
   ∟ GitFlow analysis via @gitflow-specialist
   ∟ Performance otimizada (cache + operações paralelas)
   ∟ Cleanup inteligente baseado na estratégia GitFlow
   ∟ Session archiving automático
   ∟ Task Manager auto-update para status "Done" (provider em TASK_MANAGER_PROVIDER)

[PR LINK]
```
