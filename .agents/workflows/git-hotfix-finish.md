---
description: Finalizar hotfix com merge para main/master e develop, criação de tag de patch e deploy emergencial em produção.
---

# ✅ Git Flow - Finalizar Hotfix

Finalizar correção emergencial realizando deploy para produção com merge em
main/master e develop, criação de tags emergenciais e cleanup. Workflow crítico
para release de emergency fixes. Entrada do usuário: o texto após o comando (nome
da correção opcional; auto-detecta a hotfix atual quando vazio).

## 🎯 Funcionalidades

### Emergency Release e Deployment
- Merge emergencial de hotfix branch para main/master
- Back-merge imediato para develop branch
- Criação automática de emergency patch tags
- Deploy preparation para production environment
- Cleanup automático pós-deployment

### Production Safety e Validation
- Validações críticas pré-merge para produção
- Emergency conflict detection e resolution guidance
- Production readiness verification
- Rollback preparation automática
- Emergency testing validation

### Critical Operations Management
- Conclusão da task com emergency status no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`; suporta Jira, ClickUp, Asana, Linear)
- Team notification de emergency deployment
- Emergency documentation automática
- Production deployment tracking
- Integration com CI/CD emergency pipelines

## 🚀 Como Usar

```bash
/git-hotfix-finish                        # Auto-detecta hotfix atual
/git-hotfix-finish fix-payment-gateway    # Finaliza hotfix específica
```

**Pré-requisitos**: Em hotfix branch ou especificar nome da correção

### Processo Executado
1. **Emergency Detection**: detecta hotfix branch atual ou busca específica
2. **Critical Validation**: verifica hotfix state, conflicts, production readiness
3. **Production Preview**: exibe impacto do emergency deployment
4. **Emergency Confirmation**: solicita confirmação para production release
5. **Production Merge**: executa merge para main + back-merge para develop
6. **Emergency Tagging**: cria patch tag com emergency release notes
7. **Production Deploy**: prepara deployment e, se `TASK_MANAGER_PROVIDER` != `none`, atualiza status da task no provider ativo (ex.: ClickUp, Jira, Asana, Linear)
8. **Emergency Cleanup**: remove hotfix branch e finaliza emergency workflow

### Emergency Deployment Strategy
Durante finalização emergencial:
- Production merge: emergency-optimized merge strategy
- Conflict handling: priority resolution guidance
- Tag creation: emergency patch version increment
- Team communication: immediate notification de production changes

## 🤝 Integração @gitflow-specialist

*Este comando sempre consulta @gitflow-specialist para emergency merge validation, critical conflict resolution, production deployment strategy e troubleshooting de emergency release complexos.*

## ⚠️ Resolução de Problemas

### Hotfix Branch Not Found
- **Sintoma**: não consegue detectar hotfix branch ativa
- **Solução**: `git checkout hotfix/name` ou especificar nome no comando

### Emergency Merge Conflicts
- **Causa**: conflicts críticos entre hotfix e production branches
- **Fix**: emergency resolution guidance via @gitflow-specialist

### Production Branch Protection
- **Sintoma**: branch protection impede emergency merge
- **Solução**: emergency override procedures ou Pull Request workflow

### Critical Tests Failing
- **Causa**: emergency tests falham durante validation
- **Fix**: emergency test strategy ou production override (com approval)

### Emergency Tag Creation Failed
- **Sintoma**: problemas na criação de emergency patch tags
- **Solução**: @gitflow-specialist orienta sobre tag strategy e conflicts

### Production Deployment Issues
- **Causa**: problemas durante emergency deployment preparation
- **Fix**: emergency deployment guidance e rollback preparation

### Team Notification Failed
- **Sintoma**: Task Manager configurado (ex.: ClickUp, Jira, Asana, Linear) ou team notifications não funcionando
- **Solução**: verificar `TASK_MANAGER_PROVIDER` e credenciais via `/meta-setup-integration`; manual emergency communication + @gitflow-specialist guidance
