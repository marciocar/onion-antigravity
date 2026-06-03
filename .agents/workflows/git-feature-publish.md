---
description: Publicar feature branch no remote para colaboração, com tracking automático e atualização opcional do Task Manager.
---

# 🤝 Git Flow - Publicar Feature

Publicar feature branch para remote repository permitindo colaboração em equipe
com setup automático de tracking, validações de readiness e integração com o Task
Manager configurado (se houver) para team awareness e code review workflow.
Entrada do usuário: o texto após o comando (nome de feature opcional; default é a
branch atual).

## 🎯 Funcionalidades

### Team Collaboration e Sharing
- Push seguro da feature branch para remote origin
- Setup automático de upstream tracking para colaboração
- Validações de collaboration readiness (tests, commits, documentation)
- Team notification via atualização de status no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`)
- Code review preparation automática

### Git Flow Compliance e Automação
- Publicação seguindo padrão oficial GitFlow (feature → remote)
- Automatic branch tracking configuration
- Atualização de status da task para "In Review" no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`; suporta Jira, ClickUp, Asana, Linear)
- Team guidance para next steps após publicação
- Integration com workflows de code review

### Educational e Team UX
- Context display mostrando impacto da publicação na equipe
- Progress indicators durante operações de remote
- Educational content sobre feature collaboration
- Team guidance e best practices para colaboração

## 🚀 Como Usar

```bash
/git-feature-publish                   # Publica branch atual (se feature)
/git-feature-publish feature-name      # Publica feature específica
```

**Pré-requisitos**: Branch deve existir localmente e ser uma feature branch

### Processo Executado
1. **Validation**: verifica se é feature branch e se está ready para publicação
2. **Readiness Check**: valida tests, commits, working directory
3. **Remote Setup**: configura upstream tracking se necessário
4. **Push**: executa push seguro para remote origin
5. **Task Manager Update** (condicional): se `TASK_MANAGER_PROVIDER` != `none`, atualiza status para "In Review" no provider ativo (ex.: ClickUp, Jira, Asana, Linear) e notifica team; caso contrário, etapa é ignorada
6. **Team Guidance**: fornece next steps para code review workflow

### Team Collaboration Features
Durante execução, facilita colaboração em equipe:
- Automatic remote branch creation se não existir
- Team notification via Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`)
- Code review readiness validation
- Next steps guidance para collaboration workflow

## 🤝 Integração @gitflow-specialist

*Este comando sempre consulta @gitflow-specialist para validação de remote operations, configuração de tracking, análise de readiness para team collaboration e guidance para code review preparation.*

## ⚠️ Resolução de Problemas

### Feature Branch Não Encontrada
- **Sintoma**: branch especificada não existe localmente
- **Solução**: `git checkout -b feature/name` ou usar branch existente

### Not on Feature Branch
- **Causa**: branch atual não é uma feature branch
- **Fix**: `git checkout feature/name` ou especificar feature-name no comando

### Remote Already Exists
- **Sintoma**: branch já existe no remote com divergências
- **Solução**: `git pull origin feature/name` para sincronizar antes de publicar

### Tests Failing
- **Sintoma**: validation detecta testes falhando
- **Solução**: corrigir testes antes da publicação para manter qualidade da team

### Working Directory Not Clean
- **Causa**: uncommitted changes impedem publicação segura
- **Fix**: `git add . && git commit -m "changes"` antes de publicar

### Remote Tracking Issues
- **Sintoma**: problemas de configuração de upstream tracking
- **Solução**: comando configura automaticamente via @gitflow-specialist
