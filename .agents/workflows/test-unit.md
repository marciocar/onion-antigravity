---
description: Gera e executa testes unitários automaticamente com detecção inteligente de framework, análise de código e suporte a coverage e watch.
---

# 🧪 Test Unit

Gera e executa testes unitários com detecção de framework, análise de código e
integração com ferramentas de coverage.

## Entrada do usuário

O texto após o comando informa: `file-path` (arquivo fonte para testar,
obrigatório), `--generate` (gera o arquivo de teste se não existir), `--run`
(executa após gerar/validar), `--coverage`, `--watch` e `--framework`
(`jest|vitest|pytest|junit`, sobrescreve a auto-detecção).

## ⚡ Fluxo de Execução

### Passo 1: Validar Arquivo Fonte

Se o arquivo não existe: ❌ ERRO. Extrair extensão, diretório base e nome. Tipos
suportados: `.js/.ts/.tsx/.jsx/.py/.java/.go/.rs` (outros → ⚠️ AVISO).

### Passo 2: Detectar Framework de Teste

Em ordem de prioridade: (1) configs — `package.json` (Jest/Vitest), `pytest.ini`
(PyTest), `pom.xml`/`build.gradle` (JUnit), `go.mod` (Go), `Cargo.toml` (Rust); (2)
arquivos de teste existentes; (3) inferência por linguagem. `--framework`
sobrescreve a detecção.

### Passo 3: Analisar Código Fonte

Ler o arquivo e extrair funções/métodos públicos por linguagem (JS/TS `export`;
Python `def` sem `_`; Java `public`; Go maiúscula inicial; Rust `pub`). Identificar
dependências externas para mocks. Reportar funções/classes/dependências/complexidade.

### Passo 4: Verificar Arquivo de Teste Existente

Padrões: Jest/Vitest `<file>.test.{js,ts,tsx}`; PyTest `test_<file>.py`; JUnit
`<Class>Test.java`; Go `<file>_test.go`; Rust `#[cfg(test)]`. Se existe e `--generate`:
pula geração. Se não existe e sem `--generate`: ❌ ERRO sugerindo `--generate`.

### Passo 5: Gerar Arquivo de Teste (SE `--generate`)

Ler padrões existentes para extrair estrutura/imports/nomenclatura. Gerar testes em
padrão AAA (Arrange, Act, Assert) por função pública: happy path, edge cases (null,
vazios, limites) e error handling. Configurar mocks (`jest.mock()`/`vi.mock()`).
Criar o arquivo.

Esqueleto (Jest/Vitest):
```typescript
describe('nomeFuncao', () => {
  test('should return expected result with valid input', () => {
    expect(nomeFuncao('valid input')).toBe('expected output');
  });
  test('should handle edge case', () => {
    expect(() => nomeFuncao(null)).toThrow();
  });
});
```

### Passo 6: Executar Testes (SE `--run`)

Comandos por framework (acrescentar `--coverage`/`--watch` conforme flags):
- **Jest:** `npx jest <test-file>` · **Vitest:** `npx vitest run <test-file>`
- **PyTest:** `pytest <test-file> --cov=<dir>` · **JUnit:** `mvn test -Dtest=<Class>`
- **Go:** `go test ./<pkg> -cover` · **Rust:** `cargo test <name>`

Capturar resultados (pass/fail), coverage, erros e tempo.

### Passo 7: Apresentar Resultados

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ TESTES UNITÁRIOS - <file-path>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 Detecção: framework · config · package manager
📊 Código: funções [N] · classes [N] · deps [lista] · complexidade
📝 Teste: [✅ Existente|✅ Gerado|❌] <test-file> → happy [N] · edge [N] · error [N]
🧪 Execução: [✅|❌|⚠️] [X/Y] passaram · tempo [X]s
📈 Coverage (se --coverage): Statements/Branches/Functions/Lines %
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 Próximos: revisar/expandir · re-executar `--run` · /validate-test-strategy-create · /git-code-review
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 📋 Exemplos de Uso

```bash
/test-unit src/utils/validation.js --generate --run --coverage
/test-unit app/models/user.py --generate --framework pytest
/test-unit components/Button.tsx --run --watch
/test-unit src/services/api.ts --run --coverage
```

## ⚠️ Validações e Regras

- Arquivo fonte deve existir; framework detectável ou via `--framework`; `--run`
  exige arquivo de teste ou `--generate`.
- Auto-detecção tem prioridade exceto se `--framework`; geração segue padrões do
  projeto; coverage requer framework com suporte; watch mantém o processo até Ctrl+C;
  testes gerados cobrem happy path, edge cases e error handling básicos.

## 🔧 Suporte por Linguagem

| Linguagem | Frameworks | Coverage | Watch |
|-----------|-----------|----------|-------|
| JS/TS | Jest, Vitest, Mocha | ✅ | ✅ |
| Python | PyTest, unittest | ✅ | ⚠️ |
| Java | JUnit 5/4 | ✅ | ❌ |
| Go | testing | ✅ | ⚠️ |
| Rust | cargo test | ⚠️ | ❌ |

## 🔗 Referências

- Relacionados: `/test-integration` · `/test-e2e` · `/validate-test-strategy-create` · `/engineer-work` · `/git-code-review`
- Framework de Testes: `docs/knowledge-base/frameworks/framework_testes.md`
- Persona: @test-engineer (`.agents/AGENTS.md`)
