---
description: Gera e executa testes end-to-end (Black-box) com detecção de framework, cenários por feature, selectors inteligentes e gravação de vídeo/screenshots.
---

# 🎭 Test E2E

Gera e executa testes end-to-end com detecção de framework, geração de cenários
baseados em features e integração com gravação de vídeo/screenshots.

## Entrada do usuário

O texto após o comando informa: `feature-name` (kebab-case, obrigatório — ex.:
`login`, `checkout`), `--generate`, `--run`, `--headless` (sem interface, default
`true`), `--record` (grava vídeo/screenshots) e `--framework`
(`cypress|playwright|selenium`, sobrescreve a auto-detecção).

## ⚡ Fluxo de Execução

### Passo 1: Validar Feature Name

Vazio → ❌ ERRO. Formato deve ser kebab-case (`^[a-z][a-z0-9-]*$`); senão ❌ ERRO.

### Passo 2: Detectar Framework E2E

Por prioridade: (1) configs — `cypress.config.*`→Cypress, `playwright.config.*`→
Playwright, `wdio.conf.*`→WebdriverIO/Selenium, deps em `package.json`; (2) arquivos
existentes (`cypress/e2e/**`, `e2e/**`, `tests/e2e/**`); (3) inferência por
estrutura. `--framework` sobrescreve. Detectar também a Base URL (config ou `.env`).

### Passo 3: Analisar Estrutura de Testes Existente

Buscar specs E2E e extrair page objects, nomenclatura, selectors preferidos
(data-testid/classes/IDs), helpers/fixtures e base URL.

### Passo 4: Gerar Cenários Baseados na Feature

- **Login:** valid/invalid credentials, empty fields, forgot password, remember me
- **Checkout:** complete flow, invalid payment, empty cart, shipping, order summary
- **User Registration:** valid data, duplicate email, weak password, terms
- **Search:** valid/empty query, special chars, filters, pagination
- **Genérico:** happy path, invalid input, empty state, edge cases

### Passo 5: Verificar Arquivo de Teste Existente

Padrões: Cypress `cypress/e2e/<feature>.spec.{js,ts}`; Playwright
`e2e/<feature>.spec.{js,ts}`; Selenium `tests/e2e/<feature>.test.{js,ts}`. Se existe:
continua (pula geração se `--generate`). Se não existe: gera (se `--generate`) ou erro.

### Passo 6: Gerar Arquivo de Teste (SE `--generate`)

**Selectors inteligentes** (ordem): data attributes (`[data-testid]`, `[data-cy]`) →
semantic HTML → ARIA → text content → classes/IDs (último recurso). Gerar estrutura
AAA por framework (Cypress `describe/it` + `cy.*`; Playwright `test.describe/test` +
`page.*`; Selenium `describe/it` + `browser/$`), com beforeEach (visit) e testes de
happy path, error handling e edge cases. Adicionar page objects se o projeto usar o
padrão. Criar o arquivo.

### Passo 7: Executar Testes (SE `--run`)

- **Cypress:** `npx cypress run --spec "cypress/e2e/<feature>.spec.ts"` [`--headless`] [`--record`]
- **Playwright:** `npx playwright test e2e/<feature>.spec.ts` [`--headed=false`] [`--video=on`]
- **Selenium:** `npx wdio run wdio.conf.ts --spec tests/e2e/<feature>.test.ts` [`--headless`]

Headless é default; `--headless false` abre o browser; `--record` habilita gravação.
Capturar resultados, screenshots/vídeos, erros, tempo e artifacts.

### Passo 8: Apresentar Resultados

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ TESTES E2E - <feature-name>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 Detecção: framework · config · base URL · headless
📊 Padrões: page objects [✅/❌] · selectors · estrutura
📝 Arquivo: [✅ Existente|✅ Gerado|❌] <test-file> → happy [N] · error [N] · edge [N]
🧪 Execução: [✅|❌|⚠️] [X/Y] passaram · tempo [X]s
📹 Gravação (se --record): vídeos · screenshots · artifacts
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 Próximos: revisar selectors · re-executar `--run` · /validate-test-strategy-create
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 📋 Exemplos de Uso

```bash
/test-e2e login --generate --run --record
/test-e2e checkout --run --headless false
/test-e2e user-registration --generate
/test-e2e search --run --record
```

## ⚠️ Validações e Regras

- `feature-name` vazio ou fora do kebab-case → ❌ ERRO; sem framework detectado e
  sem `--framework` → ❌ ERRO; `--run` exige arquivo de teste ou `--generate`.
- Auto-detecção tem prioridade exceto se `--framework`; geração segue padrões do
  projeto; selectors priorizam data-attributes; headless é default para CI/CD;
  `--record` captura também em sucessos (falhas são sempre capturadas).

## 🔧 Suporte por Framework

| Framework | Headless | Gravação | Page Objects | CI/CD |
|-----------|----------|----------|--------------|-------|
| Cypress | ✅ | ✅ | ✅ | ✅ |
| Playwright | ✅ | ✅ | ✅ | ✅ |
| Selenium | ✅ | ⚠️ | ✅ | ✅ |

## 🔗 Referências

- Framework de Testes: `docs/knowledge-base/frameworks/framework_testes.md`
- Relacionados: `/test-unit` · `/test-integration` · `/validate-test-strategy-create` · `/engineer-work`
- Persona: @test-engineer (`.agents/AGENTS.md`)
