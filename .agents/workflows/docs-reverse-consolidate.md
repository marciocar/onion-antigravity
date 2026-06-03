---
description: Engenharia reversa de um projeto para gerar documentação consolidada, usada como pré-processador de /docs-build-tech-docs.
---

# 🔍 Engenharia Reversa de Projetos

Orquestrador de engenharia reversa para gerar documentação consolidada.

Entrada do usuário: o texto após o comando. Parâmetros (como prosa): `project_path` (obrigatório — caminho do projeto a analisar) e `output_path` (opcional — padrão `docs/reverse/`).

## 🎯 Objetivo

Analisar qualquer projeto e gerar documento consolidado para `/docs-build-tech-docs`.

## ⚡ Fluxo de Execução

### Passo 1: Validar Input
Verificar se o `project_path` existe e se aparenta ser um projeto de software (presença de arquivos de configuração). Se config não for detectada, avisar e seguir com análise estrutural.

### Passo 2: Detectar Stack
Identificar o package manager e a stack a partir dos manifestos: `package.json` → JavaScript, `requirements.txt` → Python, `Cargo.toml` → Rust, `go.mod` → Go. Detectar frameworks (ex.: React, Fastify) e monorepo (ex.: NX) inspecionando dependências.

### Passo 3: Analisar Estrutura
Delegar para `@docs-reverse-engineer`:

1. **Directory Scan**: estrutura de pastas, padrões de arquivos, convenções de naming
2. **Dependency Analysis**: dependências principais, devDependencies, peer dependencies
3. **Architecture Detection**: padrões (MVC, DDD, Clean), camadas identificadas, pontos de integração

### Passo 4: Gerar Documento
Criar `<output_path>/consolidated.md`:

```markdown
# Documentação Consolidada: [Nome do Projeto]

## 📊 Metadados
- Stack: [stack detectado]
- Framework: [framework]
- Monorepo: [sim/não]
- Tamanho: [arquivos/linhas]

## 🏗️ Arquitetura
[Descrição da arquitetura detectada]

## 📁 Estrutura
[Árvore de diretórios comentada]

## 🔧 Componentes Principais
[Lista de módulos/componentes]

## 🔗 Integrações
[APIs, databases, serviços externos]

## 📋 Recomendações
[Sugestões para documentação adicional]
```

### Passo 5: Finalizar

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ ANÁLISE CONCLUÍDA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 Projeto: <project_path>

🔍 Stack Detectado:
∟ Linguagem: TypeScript
∟ Framework: React + Fastify
∟ Monorepo: NX
∟ Arquitetura: Clean Architecture

📁 Arquivos Gerados:
∟ docs/reverse/consolidated.md
∟ docs/reverse/structure.json
∟ docs/reverse/dependencies.json

📈 Métricas:
∟ Arquivos analisados: 245
∟ Linhas de código: 12,450
∟ Módulos: 18

🚀 Próximo: /docs-build-tech-docs
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Persona: `@docs-reverse-engineer`
- Próximo passo: `/docs-build-tech-docs`

## ⚠️ Notas

- Tempo médio: 2-5 minutos dependendo do tamanho
- Funciona com JavaScript, Python, Rust, Go
- Output otimizado para consumo por IA
