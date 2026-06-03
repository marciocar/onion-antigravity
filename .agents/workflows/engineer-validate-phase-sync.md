---
description: Valida e corrige a sincronização entre as fases do plan.md e o status das subtasks no Task Manager. Use ao final de sessões, antes de PRs ou após interrupções.
---

# 🔄 Validate Phase-Subtask Sync

Valida e corrige a sincronização automática entre as fases do `plan.md` e o status das
subtasks no Task Manager configurado. Identifica discrepâncias e corrige status desatualizados.

Entrada do usuário (opcional, como prose após o comando): `--fix-all` para corrigir todas
as inconsistências, `--report-only` para apenas relatar sem aplicar correções.

## 🎯 Funcionalidades

### Validação Automática de Status
- Lê todas as fases do `plan.md` e identifica o status atual (Completada ✅, Em Progresso ⏰, Não Iniciada ⏳).
- Verifica o status das subtasks correspondentes no provider via Phase-Subtask Mapping.
- Identifica discrepâncias entre `plan.md` e o provider.
- Gera relatório de inconsistências encontradas.

### Correção Automática de Status
- Atualiza o status das subtasks para refletir o estado real das fases.
- Adiciona comentários retroativos nas subtasks com timestamp de correção.
- Documenta ações de correção e valida a integridade do mapeamento Phase→Subtask.

### Sistema de Alertas Proativo
- Alerta quando o mapeamento Phase-Subtask está ausente ou incompleto.
- Sugere criação de mapeamento quando detecta subtasks sem correlação.
- Identifica fases órfãs (sem subtask correspondente) e subtasks órfãs (sem fase correspondente).

## 🤝 Integração com o Task Manager

Aplique a regra `task-manager-routing`: carregue o `.env` e leia `TASK_MANAGER_PROVIDER`
para rotear ao provider/adapter correto. Operações típicas:
- **Leitura de task** com subtasks para obter a estrutura completa.
- **Update de status** nas subtasks com o status correto.
- **Comentários de correção** para documentar ajustes.
- **Validação de integridade** do mapeamento.

Detalhes: `docs/knowledge-base/concepts/task-manager-abstraction.md`.

### Mapeamento Phase-Subtask
Lê o mapeamento de `docs/sessions/[slug]/context.md`:
```markdown
## 📋 Phase-Subtask Mapping
- **Phase 1**: "Template Consolidation" → Subtask ID: [id-1]
- **Phase 2**: "Feature Commands" → Subtask ID: [id-2]
- **Phase 3**: "Release Commands" → Subtask ID: [id-3]
```

### Correções Aplicadas
- Fases "Completada ✅" → Subtask status "done"
- Fases "Em Progresso ⏰" → Subtask status "in progress"
- Fases "Não Iniciada ⏳" → Subtask status "to do"

## ⚙️ Processo de Validação

1. **Detecta sessão ativa** em `docs/sessions/`.
2. **Lê context.md**: carrega o mapeamento Phase-Subtask e o task ID principal.
3. **Analisa plan.md**: extrai o status atual de todas as fases.
4. **Consulta o provider**: obtém o status atual das subtasks.
5. **Identifica discrepâncias**: compara status `plan.md` vs provider.
6. **Aplica correções**: atualiza o status das subtasks conforme necessário.
7. **Documenta ações**: registra todas as correções aplicadas.

## ⚠️ Resolução de Problemas

- **"Mapeamento Phase-Subtask não encontrado"** → verifique se o `context.md` contém a seção "Phase-Subtask Mapping".
- **"Subtask não encontrada no provider"** → os IDs do mapeamento podem estar incorretos; verifique os IDs no provider, atualize o mapeamento no `context.md` e revalide.
- **"Múltiplas fases para mesma subtask"** → revise a estrutura: uma subtask deve corresponder a uma fase específica; considere quebrar a fase complexa em várias.

## 💡 Integração com Workflow

Uso recomendado:
- **Durante o desenvolvimento**: ao final de cada sessão de trabalho.
- **Antes de PRs**: validar sincronização completa antes de `/engineer-pr`.
- **Após interrupções**: garantir consistência ao retomar o trabalho.
- **Debugging**: identificar problemas de tracking de progresso.

Prevenção automática — para projetos futuros:
1. `/engineer-start` deve criar o mapeamento automaticamente.
2. `/engineer-work` deve usar este mapeamento para updates automáticos.
3. Este comando serve como backup/validação do processo automático.

---

**🎯 Critical fix**: resolve a falha onde fases completadas não atualizavam
automaticamente o status das subtasks correspondentes, garantindo sincronização entre
`plan.md` e o Task Manager.
