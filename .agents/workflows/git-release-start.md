---
description: Iniciar release branch a partir de develop com versionamento semver, preparação de changelog e validações pré-release.
---

# 🚀 Git Flow - Iniciar Release

Iniciar processo de release criando branch de release com versionamento
automático, preparação de changelog e validações pré-release. Workflow completo
para gestão segura de releases seguindo padrões GitFlow. Entrada do usuário: o
texto após o comando (versão específica ou tipo de bump: `patch` | `minor` | `major`).

## 🎯 Funcionalidades

### Release Workflow e Versionamento
- Criação de release branch a partir de develop branch
- Auto-detecção e bump inteligente de versionamento (semver)
- Preparação automática de changelog baseada em commits
- Validações de estado pré-release (working directory, conflicts)
- Setup de task de release tracking no Task Manager configurado (condicional — somente se `TASK_MANAGER_PROVIDER` != `none`; suporta Jira, ClickUp, Asana, Linear)

### Versionamento Inteligente e Automação
- Detecção automática de package.json e version files
- Bump semântico (major.minor.patch) com validation
- Support para diferentes tipos de projeto e convenções
- Validações de tag conflicts e branch state
- Integration com @gitflow-specialist para release strategy

### Validações e Safety-First
- Verificações de repository state e uncommitted changes
- Primary branch detection (main/master) automática
- Develop branch sync validation antes da release
- Release branch creation com error handling robusto
- Educational guidance durante processo de release

## 🚀 Como Usar

```bash
/git-release-start "v2.1.0"           # Release com versão específica
/git-release-start "2.1.0"            # Versão sem prefixo v
/git-release-start "patch"            # Auto-bump patch (2.0.1 → 2.0.2)
/git-release-start "minor"            # Auto-bump minor (2.0.1 → 2.1.0)
/git-release-start "major"            # Auto-bump major (2.0.1 → 3.0.0)
```

**Pré-requisitos**: Working directory limpo, develop branch disponível

### Processo Executado
1. **Validações**: verifica repository state, versão fornecida, working directory
2. **Branch Detection**: detecta primary branch (main/master) e develop
3. **Version Processing**: processa versioning (específica ou auto-bump)
4. **Release Branch**: cria release/version branch a partir de develop
5. **Task Manager Setup** (condicional): se `TASK_MANAGER_PROVIDER` != `none`, cria task de release tracking e atualiza status no provider ativo (ex.: ClickUp, Jira, Asana, Linear); caso contrário, etapa é ignorada
6. **Changelog Prep**: prepara changelog baseado em commits desde última release

### Version Bump Intelligence
Durante execução, processa diferentes tipos de versionamento:
- Versões explícitas: valida formato semver e disponibilidade
- Auto-bump: detecta última tag e incrementa conforme tipo
- Project detection: identifica package.json, version files, etc.
- Conflict prevention: verifica tags existentes antes de proceder

## 🤝 Integração @gitflow-specialist

*Este comando sempre consulta @gitflow-specialist para release strategy validation, version bump analysis, branch creation guidance e troubleshooting de release workflows complexos.*

## ⚠️ Resolução de Problemas

### Version Required Error
- **Sintoma**: comando executado sem especificar versão
- **Solução**: fornecer versão específica ou tipo de bump (patch/minor/major)

### Uncommitted Changes
- **Causa**: working directory não está limpo antes da release
- **Fix**: `git add . && git commit -m "prepare for release"` antes de iniciar

### Develop Branch Not Found
- **Sintoma**: develop branch não existe ou não está disponível
- **Solução**: `git checkout -b develop` ou `git fetch origin develop`

### Version Already Exists
- **Causa**: tag ou branch de release já existe para versão especificada
- **Fix**: usar versão diferente ou limpar tags/branches conflitantes

### Primary Branch Detection Issues
- **Sintoma**: não consegue detectar main/master branch
- **Solução**: comando detecta automaticamente via @gitflow-specialist guidance

### Release Branch Creation Failed
- **Causa**: problemas durante criação da branch de release
- **Fix**: @gitflow-specialist fornece strategy específica para resolução
