---
description: Gera e executa testes de integração (Grey-box) com detecção de framework, incluindo contract testing, boundary testing e fuzzing de API.
---

# 🔗 Test Integration

Orquestra geração e execução de testes de integração com detecção de framework,
perspectiva **Grey-box** (dev testando outro dev).

> **Teoria e padrões Grey-box** (White/Grey/Black-box, contract testing, boundary,
> fuzzing, métricas): `docs/knowledge-base/frameworks/framework_testes.md` (seções
> "Diferenças White/Black/Grey-box", "Padrões Grey-box" e "Técnicas Grey-box"). Este
> workflow não duplica a teoria; apenas a aplica.

## Entrada do usuário

O texto após o comando informa: `api-endpoint` (endpoint ou serviço, obrigatório —
ex.: `/api/users`, `UserService`), `--generate`, `--run`, `--contract` (foco em
schemas/contratos), `--boundary` (timeouts/erros/limites), `--fuzz` (dados
malformados), `--framework` (`supertest|pact|postman|wiremock|jest|vitest`) e
`--mock-external` (mocka serviços externos, default `true`).

## ⚡ Fluxo de Execução

### Passo 1: Validar Endpoint/Service

Se vazio: ❌ ERRO (obrigatório). Formato esperado: caminho de API ou nome de serviço.

### Passo 2: Detectar Framework de Integração

Por prioridade: (1) configs — `pact.config.*`→Pact, `postman.json`/`postman/`→Postman,
`wiremock/`/`mocks/`→Wiremock, deps em `package.json` (`supertest`,
`@pact-foundation/pact`, etc.), `jest`/`vitest config`→Jest/Vitest+Supertest; (2)
arquivos de teste existentes (`**/*.integration.{js,ts}`, `**/contracts/**`,
`**/pacts/**`); (3) inferência por estrutura. `--framework` sobrescreve.

### Passo 3: Analisar API/Service

Detectar tipo de endpoint (rotas `app.get()`/`@Get()`, classes de serviço, GraphQL).
Extrair contratos (OpenAPI/Swagger, JSON Schema, Pact, TS types, GraphQL schema).
Identificar dependências externas (APIs, terceiros, DBs, queues, cache). Reportar
tipo/endpoints/contratos/deps/mock strategy.

### Passo 4: Verificar Arquivo de Teste Existente

Padrões: Supertest `<endpoint>.integration.test.{js,ts}`; Pact
`<consumer>-<provider>.spec.{js,ts}`; Postman `<collection>.postman_collection.json`.
Se existe e `--generate`: pula. Se não existe e sem `--generate`: ❌ ERRO.

### Passo 5: Gerar Arquivo de Teste (SE `--generate`)

Ler padrões existentes. Gerar testes AAA por endpoint: contract (schema/tipos),
boundary (timeouts/erros/limites) e fuzzing (dados malformados). Configurar mocks
(Wiremock/Nock/MSW).

Esqueleto (Supertest + Jest, 3 blocos `describe`):
```typescript
import request from 'supertest';
import app from '../src/app';

describe('API Integration: <api-endpoint>', () => {
  beforeEach(() => { /* setup mocks de serviços externos */ });

  describe('Contract Testing', () => {
    test('GET retorna schema válido', async () => {
      const res = await request(app).get('/api/users').expect(200);
      expect(res.body).toMatchSchema({ /* ... */ });
    });
  });
  describe('Boundary Testing', () => {
    test('trata timeout do serviço externo', async () => {
      mockExternalService.timeout();
      const res = await request(app).get('/api/users').expect(500);
      expect(res.body.error).toBe('Service timeout');
    });
  });
  describe('Fuzzing Tests', () => {
    test('trata JSON malformado', async () => {
      for (const input of ['{"name": incompleto', '{"name": "'+'x'.repeat(10000)+'"}']) {
        await request(app).post('/api/users').send(input).expect(400);
      }
    });
  });
});
```

> Para Pact e padrões detalhados, reutilize `framework_testes.md` (seções Grey-box).

### Passo 6: Executar Testes (SE `--run`)

- **Supertest+Jest:** `npx jest <test-file>` · **+Vitest:** `npx vitest run <test-file>`
- **Pact:** `npx pact-provider-verifier` · **Postman:** `npx newman run <collection>.json`
- **Wiremock:** `java -jar wiremock.jar --port 8080` (setup) + testes

Capturar resultados, contratos validados, erros e tempo.

### Passo 7: Apresentar Resultados

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ TESTES DE INTEGRAÇÃO - <api-endpoint>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 Detecção: framework · config · runner · mock strategy
📊 API/Service: tipo · endpoints [N] · contratos [Sim/Não] · deps externas
📝 Arquivo: [✅|❌] <test-file> → contract [N] · boundary [N] · fuzzing [N]
🧪 Execução: [✅|❌|⚠️] [X/Y] passaram · contratos [X/Y] · tempo [X]s
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 Próximos: revisar/expandir · re-executar `--run` · /validate-test-strategy-create
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 📋 Exemplos de Uso

```bash
/test-integration /api/users --generate --run --contract
/test-integration payment-service --generate --boundary --framework supertest
/test-integration /api/orders --run --fuzz
/test-integration user-service --generate --contract --boundary --fuzz
/test-integration /api/products --run --mock-external false
```

## ⚠️ Validações e Regras

- `api-endpoint` vazio → ❌ ERRO; sem framework detectado e sem `--framework` → ❌
  ERRO; `--run` exige arquivo de teste ou `--generate`.
- `--framework` sobrescreve a auto-detecção; geração segue Grey-box; mock externo é
  default (`true`); `--mock-external false` testa serviços reais.

## 🔧 Suporte por Framework

| Framework | Contract | Boundary | Fuzzing | Mock Strategy |
|-----------|----------|----------|---------|---------------|
| Supertest | ✅ | ✅ | ✅ | Nock, MSW |
| Pact | ✅ | ⚠️ | ❌ | Pact Mock Service |
| Postman | ✅ | ✅ | ⚠️ | Postman Mock Server |
| Wiremock | ⚠️ | ✅ | ⚠️ | Wiremock |
| Jest/Vitest | ✅ | ✅ | ✅ | Jest/Vitest mocks |

## 🔗 Referências

- Framework de Testes (teoria Grey-box): `docs/knowledge-base/frameworks/framework_testes.md`
- Relacionados: `/test-unit` · `/test-e2e` · `/validate-test-strategy-create` · `/engineer-work`
- Personas: @test-engineer, @test-agent (`.agents/AGENTS.md`)
