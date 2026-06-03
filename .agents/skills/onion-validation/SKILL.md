---
name: onion-validation
description: >
  Regras de validação para componentes do Sistema Onion no Antigravity. Use ao
  criar, revisar, auditar ou debuggar workflows, skills, rules e personas. Cobre
  frontmatter obrigatório, categorias válidas, checklists de qualidade, limites de
  linhas, detecção de duplicações e scoring. Ative ao validar artefatos em
  `.agents/`, mesmo sem o usuário mencionar "validação".
---

## Validação de Workflows (`.agents/workflows/*.md`)

### Frontmatter obrigatório
| Campo | Constraint |
|-------|------------|
| `description` | 1-2 linhas claras; o que faz + quando usar (aparece no menu `/`) |

> O nome do workflow vem do filename (`<categoria>-<comando>.md`). Metadados ricos
> (modelo, parâmetros) viram prosa no corpo — o Antigravity só consome `description`.

### Categorias válidas (prefixo do filename)
`engineer`, `product`, `git`, `docs`, `meta`, `validate`, `test`, `quick`, `development`

### Checklist
- [ ] Filename `<categoria>-<comando>.md` em kebab-case, prefixo de categoria válido
- [ ] `description` clara e específica
- [ ] Sem colisão de nome com outro workflow
- [ ] < 400 linhas
- [ ] Seção "Objetivo" e "Fluxo de Execução" presentes

## Validação de Skills (`.agents/skills/<nome>/SKILL.md`)

### Frontmatter
- `name` — lowercase-hífen (= nome do dir)
- `description` — **obrigatória**, com trigger específico ("use quando…")

### Checklist
- [ ] `description` específica (não vaga tipo "Database tools")
- [ ] < 500 linhas (lifecycle persistente)
- [ ] Sem duplicação de SKILL.md em outras pastas
- [ ] Frontmatter YAML válido
- [ ] Sem prompts interativos em scripts (agentes não respondem TTY)

## Validação de Rules (`.agents/rules/*.md`)
- [ ] Curta e imperativa (system instruction, não prosa)
- [ ] Tema único e claro no filename
- [ ] Sem sobreposição com outra rule

## Validações automatizadas
```bash
# Duplicação de nomes de workflow
ls .agents/workflows/*.md | xargs -n1 basename | sort | uniq -d

# Workflows > 400 linhas
find .agents/workflows -name "*.md" -exec wc -l {} \; | awk '$1 > 400'

# Skills > 500 linhas
find .agents/skills -name "SKILL.md" -exec wc -l {} \; | awk '$1 > 500'

# Workflows sem description
for f in .agents/workflows/*.md; do grep -q "^description:" "$f" || echo "SEM DESCRIPTION: $f"; done

# Prefixo de categoria inválido
for f in .agents/workflows/*.md; do
  case "$(basename "$f")" in
    engineer-*|product-*|git-*|docs-*|meta-*|validate-*|test-*|quick-*|development-*) ;;
    *) echo "PREFIXO INVÁLIDO: $f" ;;
  esac
done
```

## Score de qualidade (0-100)
| Critério | Pontos |
|----------|--------|
| Frontmatter válido | +25 |
| Naming/categoria corretos | +20 |
| Dentro do limite de linhas | +15 |
| Seções obrigatórias | +20 |
| Documentação clara | +20 |

**Thresholds**: 80-100 ✅ aprovado · 60-79 ⚠️ melhorias · <60 ❌ rejeitado

## Antes de criar artefato
1. Verificar se nome já existe em `.agents/`
2. Validar prefixo/categoria
3. Confirmar `description` presente e específica
4. Verificar limite de linhas

## Integração com .env
- `TASK_MANAGER_PROVIDER` obrigatório (`clickup`|`jira`|`asana`|`linear`|`none`)
- Se ausente: avisar e sugerir `/meta-setup-integration`

## Referências
- Regra `onion-conventions` (estrutura e naming)
- Regra `language-standards` (idioma)
- Workflow `/meta-metaspec-validate` (conformidade com meta-specs)
