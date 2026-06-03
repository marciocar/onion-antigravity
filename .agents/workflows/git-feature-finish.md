---
description: Finalizar feature com merge seguro para develop, cleanup de branch e archival de sessão, com confirmação obrigatória.
---

# ✅ Git Flow - Finalizar Feature

Finalizar desenvolvimento de feature realizando merge seguro para develop branch
com validações automáticas e cleanup completo. Processo seguro com confirmações
obrigatórias para prevenir erros de produção. Entrada do usuário: o texto após o
comando (auto-detecta a branch atual quando vazio).

## 🎯 Funcionalidades

### Safety-First e Validações
- Confirmação obrigatória antes de merge feature → develop
- Análise automática de conflitos e working directory
- Validação de status da develop branch (sincronização)
- Preview detalhado das mudanças que serão mergeadas
- Guidance para resolução de problemas encontrados

### GitFlow Compliance e Automação
- Merge seguindo padrão oficial GitFlow (feature → develop)
- Cleanup automático de branch local e remote após merge
- Atualização da task no Task Manager configurado e session archival
- Integração preservada com @gitflow-specialist para operações complexas

### Educação e UX
- Context display mostrando impacto das mudanças
- Progress indicators durante operação
- Educational content sobre GitFlow workflow
- Next steps guidance após finalização

## 🚀 Como Usar

```bash
/git-feature-finish                    # Auto-detecta branch atual
```

**Pré-requisitos**: Execute na branch de feature que deseja finalizar

### Processo Executado
1. **Análise**: detecta branch atual e valida estado do repositório
2. **Validações**: verifica working directory, conflicts e status develop
3. **Preview**: exibe impacto das mudanças (commits, files, lines)
4. **Confirmação**: solicita confirmação explícita do usuário
5. **Merge**: executa merge seguro feature → develop
6. **Cleanup**: remove branch local/remote e atualiza a task no Task Manager configurado
7. **Archive**: move session para estado finalizado

### Educational Context
Durante execução, o comando ensina conceitos GitFlow:
- Visualização do workflow: `develop → feature/name → develop`
- Impacto da operação na team collaboration
- Best practices para feature development
- Guidance para próximos passos

## 🤝 Integração @gitflow-specialist

*Este comando sempre consulta @gitflow-specialist para análise de conflitos, validação de merge strategy, execução segura do merge e guidance para resolução de problemas complexos.*

## ⚠️ Resolução de Problemas

### Uncommitted Changes
- **Sintoma**: working directory não está limpo
- **Solução**: `git add . && git commit -m "final changes"` antes de finalizar

### Merge Conflicts Detectados
- **Causa**: mudanças conflitantes entre feature e develop
- **Fix**: resolver conflicts manualmente ou usar `git merge develop` na feature branch primeiro

### Develop Branch Desatualizada
- **Sintoma**: develop branch está atrás do remote
- **Solução**: `git checkout develop && git pull origin develop` antes de finalizar feature

### Tests Failing
- **Sintoma**: testes automatizados falhando
- **Solução**: corrigir testes ou usar flag de override (não recomendado)

### Feature Branch Não Encontrada
- **Causa**: not em uma feature branch ou branch name incorreto
- **Fix**: `git checkout feature/your-feature-name` antes de executar comando

### Remote Branch Issues
- **Sintoma**: problemas com remote branch tracking
- **Solução**: `git push -u origin feature/name` para estabelecer tracking
