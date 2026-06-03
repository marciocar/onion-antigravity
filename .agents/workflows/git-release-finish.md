---
description: Finalizar release com merge para main/master e develop, criação de tag anotada, publicação e cleanup da branch.
---

# ✅ Git Flow - Finalizar Release

Finalizar processo de release realizando merge seguro para main/master e develop,
criação de tags, publicação e cleanup. Workflow completo de release deployment com
validações automáticas e integração com o Task Manager configurado (se houver).
Entrada do usuário: o texto após o comando (versão opcional; auto-detecta a release
branch atual quando vazio).

## 🎯 Funcionalidades

### Release Completion e Merge Strategy
- Merge seguro de release branch para main/master branch
- Back-merge para develop branch mantendo sincronização
- Criação automática de tags anotadas com release notes
- Validações pré-merge (conflicts, tests, working directory)
- Cleanup automático de release branch após finalização

### Publishing e Deployment Integration
- Tag publishing para remote repository
- Release notes generation baseada em changelog
- Conclusão da task e atualização de status no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`; suporta Jira, ClickUp, Asana, Linear)
- Team notification via release completion workflow
- Integration com CI/CD pipelines através de tags

### Safety-First e Validações
- Confirmação obrigatória antes de merge para main
- Análise de impacto completa (commits, files, changes)
- Validação de release branch state e readiness
- Preview detalhado das mudanças que serão mergeadas
- Rollback guidance caso problemas sejam detectados

## 🚀 Como Usar

```bash
/git-release-finish                   # Auto-detecta release branch atual
/git-release-finish v2.1.0            # Finaliza release específica
```

**Pré-requisitos**: Em release branch ou especificar versão da release

### Processo Executado
1. **Detection**: detecta release branch atual ou busca por versão específica
2. **Validations**: verifica release branch state, conflicts, working directory
3. **Preview**: exibe impacto do merge (commits, files, deployment implications)
4. **Confirmation**: solicita confirmação explícita para merge em main
5. **Merge Strategy**: executa merge para main + back-merge para develop
6. **Tag Creation**: cria tag anotada com release notes automáticas
7. **Publishing**: publica tags e atualiza remote branches
8. **Cleanup**: remove release branch e, se `TASK_MANAGER_PROVIDER` != `none`, registra completion da task no provider ativo (ex.: ClickUp, Jira, Asana, Linear)

### Merge Strategy Intelligence
Durante execução, aplica strategy inteligente:
- Main merge: fast-forward quando possível, merge commit quando necessário
- Develop back-merge: garante sincronização sem perder desenvolvimento
- Conflict detection: identifica e orienta resolução antes do merge
- Tag management: cria tags consistentes com convention estabelecida

## 🤝 Integração @gitflow-specialist

*Este comando sempre consulta @gitflow-specialist para merge strategy validation, conflict resolution guidance, tag creation best practices e troubleshooting de release deployment complexo.*

## ⚠️ Resolução de Problemas

### Release Branch Not Found
- **Sintoma**: não consegue detectar release branch ativa
- **Solução**: `git checkout release/version` ou especificar versão no comando

### Merge Conflicts Detected
- **Causa**: conflicts entre release branch e main/develop
- **Fix**: resolver conflicts manualmente antes de finalizar release

### Uncommitted Changes in Release
- **Sintoma**: release branch tem changes não commitadas
- **Solução**: `git add . && git commit -m "final release changes"`

### Tag Already Exists
- **Causa**: tag da versão já existe no repository
- **Fix**: usar `git tag -d tagname` para remover ou escolher versão diferente

### Main Branch Protection
- **Sintoma**: branch protection impede merge direto
- **Solução**: usar Pull Request workflow ou ajustar branch protection

### Tests Failing in Release
- **Causa**: release branch não passa nos testes automatizados
- **Fix**: corrigir testes ou usar override com approval (não recomendado)

### Remote Publishing Issues
- **Sintoma**: problemas ao publicar tags ou branches
- **Solução**: @gitflow-specialist orienta sobre remote configuration e permissions
