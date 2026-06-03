---
description: Sincronizar e organizar as sessões de trabalho do Sistema Onion, preservando contexto e decisões em docs/sessions/.
---

# 🔄 Sync Sessions — Sincronização de Sessões Onion

Sincroniza e organiza todas as sessões de trabalho do Sistema Onion, garantindo que o contexto de desenvolvimento esteja preservado e acessível para referência futura.

Entrada do usuário: o texto após o comando — opcionalmente o nome da sessão e/ou flags.

## 🎯 Objetivo

Analisa o trabalho realizado na sessão atual, organiza a documentação gerada e sincroniza com a estrutura `docs/sessions/`, mantendo um histórico organizado de todas as atividades de desenvolvimento.

## 🎯 Funcionalidades

### Organização de Sessões
- Detecta o trabalho realizado na sessão atual
- Cria estrutura organizada por data e tópico
- Preserva contexto e decisões tomadas
- Gera índice navegável de sessões

### Sincronização Automática
- Identifica arquivos criados/modificados
- Captura workflows Onion executados
- Preserva interações e decisões
- Mantém histórico de mudanças

### Validação e Integridade
- Verifica completude da documentação da sessão
- Valida estrutura de diretórios
- Identifica sessões incompletas
- Sugere melhorias na organização

## 🚀 Como Usar

```bash
/docs-sync-sessions                          # Sincronizar sessão atual
/docs-sync-sessions "implementacao-feature-x" # Sincronizar com nome customizado
/docs-sync-sessions --archive                # Sincronizar e arquivar sessão
/docs-sync-sessions --validate-only          # Apenas validar sem sincronizar
```

## 📋 Processo Executado

### 1. Análise da Sessão Atual
- Identifica data/hora de início
- Lista arquivos criados/modificados
- Captura workflows executados
- Extrai decisões e contexto

### 2. Estruturação
```
docs/sessions/
└── YYYY-MM-DD_HHMM_topic-name/
    ├── README.md              # Resumo da sessão
    ├── context.md             # Contexto inicial
    ├── decisions.md           # Decisões tomadas
    ├── changes.md             # Mudanças realizadas
    ├── notes.md               # Notas e observações
    ├── files-changed.txt      # Lista de arquivos
    └── commands-executed.txt  # Workflows usados
```

### 3. Geração de Documentação
- **README.md**: resumo executivo da sessão
- **context.md**: contexto e motivação
- **decisions.md**: decisões arquiteturais e técnicas
- **changes.md**: log detalhado de mudanças
- **notes.md**: anotações e insights

### 4. Sincronização
- Move/copia arquivos para estrutura correta
- Atualiza índice de sessões
- Gera links de navegação
- Valida integridade

## 📁 Estrutura de Sessão

### README.md
```markdown
# [Topic Name] - [Date]

## 🎯 Objetivo
[Descrição do objetivo da sessão]

## 📊 Resultados
- [Lista de entregas]
- [Arquivos criados/modificados]

## 🔗 Links Relacionados
- [Documentação relacionada]
- [Issues/PRs relacionados]

## ⏱️ Tempo Investido
[Duração aproximada]
```

### context.md
```markdown
# Contexto - [Topic]

## Situação Inicial
[Estado do projeto antes da sessão]

## Motivação
[Por que este trabalho foi necessário]

## Restrições
[Limitações técnicas, tempo, recursos]

## Referências
[Links, documentos, discussões relevantes]
```

### decisions.md
```markdown
# Decisões Tomadas - [Topic]

## Decisão 1: [Título]
- **Contexto**: [Por que esta decisão foi necessária]
- **Opções Consideradas**:
  - Opção A: [Prós/Contras]
  - Opção B: [Prós/Contras]
- **Decisão**: [Opção escolhida]
- **Justificativa**: [Razões]
- **Impacto**: [Consequências]
```

### changes.md
```markdown
# Mudanças Realizadas - [Topic]

## Arquivos Criados
- `path/to/file1.ts` - [Descrição]

## Arquivos Modificados
- `path/to/existing.ts`
  - [Descrição da mudança]

## Workflows Executados
1. `/docs-build-tech-docs` - [Resultado]
2. `/git-feature-start` - [Branch criada]

## Testes Adicionados
- [Lista de testes criados]
```

## 🤖 Integração com Personas

Este workflow convoca automaticamente:
- **@branch-documentation-writer**: gera documentação estruturada
- **@metaspec-gate-keeper**: valida conformidade com padrões
- **@gitflow-specialist**: auxilia em questões Git se necessário

## ⚙️ Opções Avançadas

### Flags Disponíveis
```bash
--archive          # Move sessão para archived/
--validate-only    # Apenas valida sem sincronizar
--force            # Força sincronização mesmo com erros
--skip-git         # Não inclui informações Git
--detailed         # Gera relatório detalhado
```

### Exemplos Avançados
```bash
/docs-sync-sessions "refactoring-api" --archive
/docs-sync-sessions --validate-only
/docs-sync-sessions --force --detailed
```

## 📊 Métricas Capturadas

Captura automaticamente: arquivos (criados/modificados/deletados), linhas de código (adicionadas/removidas), workflows Onion executados, tempo aproximado, personas convocadas e commits Git relacionados (se aplicável).

## ⚠️ Resolução de Problemas

- **Sessão já existe**: erro "Session directory already exists" → use `--force` ou renomeie a sessão
- **Arquivos não detectados**: lista incompleta por arquivos fora do workspace ou gitignored → verifique `.gitignore` e os limites do workspace
- **Contexto insuficiente**: README com pouca informação → execute workflows Onion com mais contexto antes de sincronizar

## 🔍 Validações Realizadas

- ✅ Estrutura de diretórios correta
- ✅ Todos os arquivos markdown obrigatórios presentes
- ✅ README.md com seções mínimas
- ✅ Links internos funcionando
- ✅ Sem duplicação de sessões
- ✅ Índice de sessões atualizado

## 📈 Output Esperado

```
🔄 Sincronizando Sessão Onion...

📊 Análise da Sessão:
  • Tópico: implementação-dashboard-operacoes
  • Data: 2025-10-03 10:30
  • Arquivos Criados: 15
  • Arquivos Modificados: 8
  • Workflows Executados: 5
  • Personas Convocadas: 3

📁 Estrutura Criada:
  ✅ docs/sessions/2025-10-03_1030_dashboard-operacoes/
     ✅ README.md
     ✅ context.md
     ✅ decisions.md
     ✅ changes.md
     ✅ notes.md
     ✅ files-changed.txt
     ✅ commands-executed.txt

🔗 Índice Atualizado:
  ✅ docs/sessions/INDEX.md

✅ Sessão sincronizada com sucesso!
```

## 🎯 Casos de Uso

- **Fim de sessão**: `/docs-sync-sessions "refactoring-contracts-module"`
- **Antes de trocar de branch**: `/docs-sync-sessions --detailed` e então `git checkout other-branch`
- **Auditoria de trabalho**: `/docs-sync-sessions --validate-only --detailed`
- **Arquivar trabalho concluído**: `/docs-sync-sessions "feature-x-completed" --archive`

## 🔗 Workflows Relacionados

- `/docs-build-index` - reconstrói índice de documentação
- `/docs-docs-health` - verifica saúde da documentação
- `/docs-validate-docs` - valida completude
- `/git-help` - ajuda com Git workflows

## 📝 Notas Importantes

1. **Frequência**: execute ao final de cada sessão significativa de trabalho
2. **Contexto**: quanto mais contexto fornecer, melhor a documentação gerada
3. **Consistência**: manter padrão de nomenclatura ajuda na navegação
4. **Limpeza**: arquive sessões antigas periodicamente

## 🎓 Best Practices

1. Nomeie sessões descritivamente
2. Documente o "porquê" das decisões, não apenas o "o quê"
3. Preserve contexto (links para issues, PRs, discussões)
4. Seja consistente nos padrões de nomenclatura
5. Arquive sessões antigas em `archived/` periodicamente

---

**Persona Responsável**: `@branch-documentation-writer`
**Validador**: `@metaspec-gate-keeper`
**Categoria**: Documentação / Organização
