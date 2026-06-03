---
description: Transcrever áudio com Whisper de forma eficiente — use para instalar, transcrever reuniões, resolver problemas ou conectar a transcrição ao workflow de reuniões.
---

# Whisper - Transcrição de Áudio

Workflow facilitador para usar a persona @whisper-specialist com eficiência máxima.
Entrada do usuário: a `query` (pergunta/necessidade) e, opcionalmente, `audio_file`
(arquivo a transcrever) e `platform` (windows/linux/macos). Tudo após o comando.

## Objetivo
Facilitar a transcrição de áudio, integrando com workflows do Sistema Onion: detectar a
necessidade do usuário, delegar para @whisper-specialist com contexto otimizado, conectar
ao workflow de processamento de reuniões e fornecer comandos prontos para uso.

## Fluxo de Execução

### Passo 1: Detectar Necessidade
Analisar `query` e `audio_file`:
- **`audio_file` fornecido** → transcrever arquivo específico (preparar comando otimizado).
- **`query` com palavras-chave:** "instalar/setup/configurar" → instalação ·
  "usar/transcrever/processar" → uso · "erro/problema/não funciona" → troubleshooting ·
  "workflow/integração/sistema onion" → workflow completo.
- **Sem parâmetro** → mostrar ajuda geral e opções.

### Passo 2: Delegar para @whisper-specialist
Invocar a persona com contexto otimizado (query completa + plataforma + arquivo + necessidade
detectada). Pedir: comandos específicos da plataforma, explicação dos passos, validação/testes
e próximos passos no workflow (se aplicável).

### Passo 3: Integrar com o Workflow do Sistema Onion
Se a transcrição foi realizada, apresentar os próximos passos:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ TRANSCRIÇÃO CONCLUÍDA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📄 Arquivo transcrito: [arquivo.txt]
🔗 Próximos passos:
1. /product-extract-meeting source=[arquivo.txt] level=executive
2. /product-consolidate-meetings source=docs/meet/
3. /product-convert-to-tasks source=docs/meet/consolidation-*.md
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Output Esperado
- **Instalação:** comandos específicos da plataforma + comando de validação.
- **Transcrição:** comando Whisper otimizado + parâmetros (modelo, idioma `pt`, formato `txt`,
  GPU se disponível) + próximo passo (`/product-extract-meeting`).
- **Troubleshooting:** problema identificado + solução detalhada + comandos + prevenção.

## Casos de Uso
- **Instalação:** `/product-whisper "Como instalar Whisper?" platform=linux`
- **Transcrição simples:** `/product-whisper audio_file=reuniao.mp3`
- **Workflow completo:** transcrever → `/product-extract-meeting` →
  `/product-consolidate-meetings` → `/product-convert-to-tasks`
- **Troubleshooting:** `/product-whisper "FFmpeg não encontrado no Windows"`
- **Ajuda geral:** `/product-whisper` (sem parâmetros)

## Otimizações Automáticas
- Formato de saída sempre `txt` (melhor integração).
- Modelo recomendado: `small` ou `medium` para português (equilíbrio).
- GPU detectada e usada automaticamente se disponível. Idioma sempre `pt`.

## Parâmetros
- **`query`** (opcional): pergunta/necessidade (instalação, uso, troubleshooting, workflow).
- **`audio_file`** (opcional): caminho do áudio (.mp3, .m4a, .wav, .mp4 etc.) — prepara
  comando de transcrição otimizado.
- **`platform`** (opcional): windows | linux | macos — se ausente, detecta ou pergunta.

## Boas Práticas
1. Especifique a plataforma para instalação (mais preciso).
2. Forneça o arquivo de áudio para transcrição direta.
3. Use o workflow completo para máximo aproveitamento.
4. Consulte @whisper-specialist para questões técnicas detalhadas.
5. Revise a transcrição antes de processar com o Framework EXTRACT.

## Relacionamentos
- **Persona principal:** @whisper-specialist (consulta `docs/knowledge-base/tools/whisper.md`).
- **Workflows relacionados:** `/product-extract-meeting`, `/product-consolidate-meetings`,
  `/product-convert-to-tasks`.

## Referências
- Personas em `.agents/AGENTS.md` · KB: `docs/knowledge-base/tools/whisper.md`
