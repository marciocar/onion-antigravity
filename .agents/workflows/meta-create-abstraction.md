---
description: Cria uma camada de abstração agnóstica de provedor (Task Manager, Notification, Storage) seguindo o padrão SDAAL.
---

# 🏗️ Criar Abstraction Layer (SDAAL)

Gerador de camadas de abstração seguindo o padrão **Specification-Driven AI Abstraction Layer**.

Entrada do usuário: o texto após o comando — `abstraction_name` (kebab-case,
obrigatório) e, opcionalmente, `interface_name`, `providers` (lista separada por
vírgula) e uma descrição breve do propósito.

## 🎯 Objetivo

Criar estrutura completa de abstração agnóstica de provedor, permitindo trocar implementações sem modificar workflows ou personas/subagents.

## 📚 Base Conceitual (NÃO duplicar aqui)

Este workflow **orquestra** a criação. O conhecimento de fundo já existe nas KBs — leia-as antes de gerar:

- **Padrão SDAAL** (fundamentos, arquitetura, design patterns, anti-patterns, quando usar): `docs/knowledge-base/concepts/specification-driven-ai-abstraction-layer.md`
- **Implementação de referência real** (Task Manager): `docs/knowledge-base/concepts/task-manager-abstraction.md` e a regra `task-manager-routing`
- **Templates completos de geração** (README, interface, types, detector, factory, adapters, none, .env): `docs/knowledge-base/patterns/sdaal-examples.md`

## 📐 Estrutura Gerada

```
.agents/abstractions/{{abstraction_name}}/
├── README.md           # Visão geral e uso rápido
├── interface.md        # Interface/Contrato principal
├── types.md            # Tipos de entrada e saída
├── factory.md          # Criação de instâncias
├── detector.md         # Detecção de contexto/provedor
└── adapters/
    ├── {{provider}}.md  # Um por provedor
    └── none.md          # Fallback (Null Object Pattern)
```

## ⚡ Fluxo de Execução

### Passo 1: Validação de Entrada

- Verificar se a abstração já existe na pasta de abstrações — se existir, **abortar** com erro.
- Validar formato kebab-case do nome (`^[a-z][a-z0-9]*(-[a-z0-9]+)*$`).

**Checklist de Validação:**
- [ ] Nome único (não existe ainda)
- [ ] Nome em kebab-case válido
- [ ] Pelo menos 1 provedor definido (ou usar fallback only)

### Passo 2: Determinar Valores

Derivar automaticamente os placeholders usados pelos templates:

| Input | Derivação |
|-------|-----------|
| `{{abstraction_name}}` | `notification-manager` (input) |
| `{{interface_name}}` | `INotificationManager` (auto: `I` + PascalCase) |
| `{{providers}}` | `slack,discord,email` ou `none` se vazio |
| `{{env_prefix}}` | `NOTIFICATION_MANAGER` (auto: UPPER_SNAKE) |

> Convenção: `interface_name` sem o prefixo `I` (ex.: `INotificationManager` → `NotificationManager`) é usado em provider types e funções factory `get<Nome>()`.

### Passo 3: Criar Estrutura de Diretórios

Criar a pasta da abstração e a subpasta `adapters/`.

### Passo 4: Gerar Arquivos a partir dos Templates

Para cada arquivo, **use o template correspondente** em `docs/knowledge-base/patterns/sdaal-examples.md`, substituindo os placeholders pelos valores derivados no Passo 2:

| Arquivo gerado | Seção do template em `sdaal-examples.md` |
|----------------|-------------------------------------------|
| `README.md` | 1. README.md |
| `interface.md` | 2. interface.md |
| `types.md` | 3. types.md |
| `detector.md` | 4. detector.md |
| `factory.md` | 5. factory.md |
| `adapters/{{provider}}.md` (um por provedor) | 6. adapters/{{provider}}.md |
| `adapters/none.md` | 7. adapters/none.md |

**Regras ao gerar:**
- Cada provedor em `{{providers}}` gera um adapter em estado **stub** (métodos com `TODO`).
- O `none.md` é sempre gerado e é **funcional** (degradação graciosa).
- Manter emojis nos headers e separadores ASCII (otimização para parsing por IA).
- Cada arquivo deve ter < 400 linhas.

### Passo 5: Atualizar .env.example

Acrescentar ao `.env.example` o bloco de configuração (template seção 8 em `sdaal-examples.md`):

```bash
{{env_prefix}}_PROVIDER=none  # <providers> | none
# + uma seção <PROVIDER>_TOKEN / _WORKSPACE por provedor
```

## 🧪 Exemplos de Invocação

```bash
# Abstração de notificações com 3 provedores
/meta-create-abstraction notification-manager --providers slack,discord,email

# Abstração de storage, interface explícita
/meta-create-abstraction file-storage --interface_name IFileStorage --providers s3,gcs

# Apenas fallback (sem provedor externo)
/meta-create-abstraction local-cache
```

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ ABSTRACTION LAYER CRIADA (SDAAL)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📁 Estrutura criada com README, interface, types, factory, detector
   e adapters (um por provedor + none).

📋 Detalhes:
∟ Interface: {{interface_name}}
∟ Provedores: <lista>
∟ Env Prefix: {{env_prefix}}_PROVIDER

🔧 Próximos Passos:
1. Definir métodos em interface.md
2. Adicionar tipos em types.md
3. Implementar adapters em adapters/
4. Configurar .env com {{env_prefix}}_PROVIDER

📚 Documentação:
∟ Pattern: docs/knowledge-base/concepts/specification-driven-ai-abstraction-layer.md
∟ Templates: docs/knowledge-base/patterns/sdaal-examples.md
∟ Exemplo real: regra task-manager-routing
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- SDAAL Pattern: `docs/knowledge-base/concepts/specification-driven-ai-abstraction-layer.md`
- Templates SDAAL: `docs/knowledge-base/patterns/sdaal-examples.md`
- Referência real: regra `task-manager-routing`
- Persona: `@onion`

## ⚠️ Notas

- Cada arquivo deve ter < 400 linhas.
- Interface deve ser extensível (Open/Closed).
- Sempre incluir `NoProviderAdapter` (fallback funcional).
- Documentar variáveis de ambiente necessárias.
- Usar emojis e separadores ASCII para facilitar parsing por IA.
