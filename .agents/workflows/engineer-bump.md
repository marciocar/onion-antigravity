---
description: Aumenta o número de versão do projeto seguindo semver (major, minor ou patch). Use ao preparar um release.
---

# Engineer Bump

Vamos preparar o projeto para um release aumentando o número da versão.

Siga estas regras para versionamento `x.y.z`:

- **x (major)**: incremente para mudanças incompatíveis na API ou funcionalidade. Exemplos:
  - Mudanças que quebram APIs públicas (ex.: remover ou renomear métodos).
  - Reescritas majors ou refatoração que alteram comportamento.
  - Mudanças que requerem que usuários atualizem seu código ou dependências para manter compatibilidade.
- **y (minor)**: incremente ao adicionar novas funcionalidades ou melhorias de forma retrocompatível. Exemplos:
  - Adicionar novos métodos, endpoints ou funcionalidades.
  - Depreciar funcionalidades (mas não removê-las ainda).
  - Melhorias que não quebram funcionalidades existentes.
- **z (patch)**: incremente para correções de bugs retrocompatíveis ou pequenas atualizações. Exemplos:
  - Corrigir bugs sem alterar funcionalidade pretendida.
  - Pequenas melhorias de performance.
  - Atualizações de documentação ou mudanças de metadata.

## Execução

1. Altere a versão no `pyproject.toml`.
2. Execute `uv sync --all-extras` para regenerar o lock file.

> Adapte ao ecossistema do projeto: em projetos Node, ajuste a versão em `package.json` e regenere o lock equivalente.
