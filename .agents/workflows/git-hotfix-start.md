---
description: Iniciar hotfix branch a partir de main/master para correção emergencial em produção, com tracking urgente opcional.
---

# 🚨 Git Flow - Iniciar Hotfix

Iniciar correção emergencial criando hotfix branch a partir de main/master para
resolver problemas críticos em produção. Workflow otimizado para máxima velocidade
com validações de segurança essenciais. Entrada do usuário: o texto após o comando
(o nome da correção).

## 🎯 Funcionalidades

### Emergency Hotfix Workflow
- Criação imediata de hotfix branch a partir de main/master (produção)
- Auto-versioning para patch releases emergenciais
- Task urgente com prioridade máxima automática no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`; suporta Jira, ClickUp, Asana, Linear)
- Validações críticas de estado de produção
- Setup otimizado para correção imediata

### Safety-First Emergency Operations
- Detecção automática de primary branch (main/master)
- Validações essenciais de working directory
- Emergency override para uncommitted changes
- Rollback preparation automática
- Emergency documentation e tracking setup

### Criticidade e Tracking Urgente
- Tags de urgência automáticas no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`; ex.: ClickUp, Jira, Asana, Linear)
- Notificações escaladas para team awareness
- Emergency workflow prioritization
- Integration com @gitflow-specialist para emergency guidance
- Automated emergency documentation

## 🚀 Como Usar

```bash
/git-hotfix-start "fix-payment-gateway"    # Emergency payment fix
/git-hotfix-start "security-patch-auth"    # Security hotfix
/git-hotfix-start "critical-memory-leak"   # Performance emergency
/git-hotfix-start "api-timeout-fix"        # API emergency
```

**Pré-requisitos**: Nome da correção obrigatório, repositório Git válido

### Processo Executado
1. **Emergency Validation**: verifica nome hotfix, repository state, critical validations
2. **Primary Branch Detection**: detecta main/master automaticamente
3. **Emergency Override**: handles uncommitted changes com emergency stashing
4. **Hotfix Branch Creation**: cria hotfix/name branch a partir de produção
5. **Task Manager Emergency Setup** (condicional): se `TASK_MANAGER_PROVIDER` != `none`, cria task urgente com máxima prioridade no provider ativo (ex.: ClickUp, Jira, Asana, Linear); caso contrário, etapa é ignorada
6. **Team Notification**: alerta equipe sobre emergency fix em andamento

### Emergency Features
Durante execução emergencial:
- Working directory: emergency stash se necessário
- Branch detection: main/master identification automática
- Priority escalation: task com urgência máxima no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`)
- Team alerts: notification automática de emergency workflow

## 🤝 Integração @gitflow-specialist

*Este comando sempre consulta @gitflow-specialist para emergency workflow guidance, critical branch validation, hotfix strategy optimization e troubleshooting de situações emergenciais complexas.*

## ⚠️ Resolução de Problemas

### Hotfix Name Required
- **Sintoma**: comando executado sem nome da correção
- **Solução**: fornecer nome descritivo da correção emergencial

### Uncommitted Changes Emergency
- **Causa**: working directory não limpo durante emergency
- **Fix**: comando oferece emergency stash automático ou manual commit

### Primary Branch Detection Failed
- **Sintoma**: não consegue detectar main/master branch
- **Solução**: @gitflow-specialist fornece detection strategy para repository

### Not Git Repository
- **Causa**: executado fora de repositório Git válido
- **Fix**: `git init` seguido de `/git-init` para emergency setup

### Emergency Override Issues
- **Sintoma**: problemas durante emergency workflow execution
- **Solução**: @gitflow-specialist fornece emergency guidance específica

### Critical State Validation Failed
- **Causa**: repository em estado inconsistente para hotfix
- **Fix**: emergency validation guidance via @gitflow-specialist consultation
