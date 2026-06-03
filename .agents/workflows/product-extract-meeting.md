---
description: Extrai conhecimento estruturado de transcrições de reuniões via Framework EXTRACT e @extract-meeting-specialist. Use para transformar contexto bruto em artefatos de alto valor para humanos, sistemas e IA.
---

# 📋 Extrair Conhecimento de Reunião

Transformação de transcrições brutas em conhecimento estruturado usando o Framework EXTRACT.

Entrada do usuário: o texto após o comando — caminho do arquivo ou pasta com
transcrição(ões) (`source`) e, opcionalmente, o nível (`level`: compact, executive,
complete, graph — padrão executive) e o foco (`focus`: decisions, tasks, gaps, all —
padrão all).

## 🎯 Objetivo

Processar transcrições de reuniões e gerar outputs estruturados consumíveis por humanos,
sistemas e IA, seguindo as 7 dimensões do Framework EXTRACT.

## ⚡ Fluxo de Execução

### Passo 1: Validar input
Verificar se `source` existe e identificar o tipo: pasta (processar arquivos `.txt`/`.md`)
ou arquivo único.

### Passo 2: Ler conteúdo
Ler a(s) transcrição(ões) do `source`.

### Passo 3: Aplicar Framework EXTRACT

Invocar `@extract-meeting-specialist`:

```markdown
## Tarefa
Processar a transcrição abaixo aplicando o Framework EXTRACT completo.

## Framework EXTRACT (7 Dimensões)
- Essência: resumo executivo em 3 linhas
- Xpectativas: objetivos da reunião e status (atingido/parcial/não)
- Tarefas: ações definidas (quem, o quê, quando)
- Resoluções: decisões tomadas com justificativa
- Ambiguidades: gaps, contradições, pontos não resolvidos
- Conexões: dependências, stakeholders, documentos relacionados
- Timeline: datas e marcos mencionados

## Nível de Output: {{level}} (default: executive)
## Foco: {{focus}} (default: all)

## Regras
1. NUNCA inventar — usar [INFERIDO] ou [NÃO ESPECIFICADO]
2. Indicar confidence level em decisões (high/medium/low)
3. Capturar TODOS os gaps — o que NÃO foi decidido é crítico
4. Tasks com owner + deadline sempre que possível
5. Preservar citações importantes entre aspas

## Transcrição
[CONTEÚDO DO ARQUIVO]
```

### Passo 4: Gerar output conforme o nível

- **compact:** título, decisão principal, ações (quem/quê/quando), principal gap.
- **executive:** resumo (3-5 linhas), decisões, tabela de ações (responsável/ação/prazo),
  pendências, timeline.
- **complete:** YAML completo seguindo o schema do knowledge base.
- **graph:** JSON com entidades e relacionamentos para sistemas.

### Passo 5: Salvar resultado
Salvar em `docs/meetings/<nome-do-source>-extract.md` (criar o diretório se não existir).

## 📤 Output Esperado

```
✅ EXTRAÇÃO CONCLUÍDA
📁 Arquivo: docs/meetings/[nome]-extract.md
📊 EXTRACT: Essência ✅ · Expectativas ✅ · Tarefas [N] · Resoluções [N] ·
   Ambiguidades [N] · Conexões [N] · Timeline [N]
📈 Qualidade: Confidence [0.X] · Tasks com Owner [X]% · com Deadline [X]% ·
   Gaps com Owner Sugerido [X]%
🚀 Próximos passos: revisar gaps críticos · validar decisões · criar tasks
   (@task-specialist)
```

## 🎯 Exemplos de Uso

- Extrair reunião específica (executive default): `/product-extract-meeting source=rhilo-reuniao-28-nov.txt`
- Nível completo com YAML: `/product-extract-meeting source=reuniao.txt level=complete`
- Foco em decisões: `/product-extract-meeting source=reuniao.txt focus=decisions`
- Processar pasta de contexto: `/product-extract-meeting source=contextos/projeto-x/`
- Grafo para sistemas: `/product-extract-meeting source=reuniao.txt level=graph`

## ⚠️ Notas

- Processar em até 24h após a reunião (contexto fresco).
- Validar decisões críticas com participantes.
- Gaps são tão importantes quanto decisões.
- Para criar tasks automaticamente, usar `@task-specialist` após a extração.

## 🔗 Referências

- **Agente:** `@extract-meeting-specialist` · Relacionados: `@product-agent`, `@task-specialist`
- **Knowledge Base:** `docs/knowledge-base/concepts/meeting-transcription-to-knowledge-base.md`
- **Relacionados:** `/product-task`, `/docs-build-tech-docs`, `/meta-create-knowledge-base`
- **Personas/subagentes:** `.agents/AGENTS.md`
