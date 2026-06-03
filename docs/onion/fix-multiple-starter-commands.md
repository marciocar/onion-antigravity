> ⚠️ **Documento histórico.** Refere-se ao CLI/wizard e à estrutura `.onion/` / `.claude/commands/`, **abandonados em 2026-05-18**. O Sistema Onion migrou para o **Google Antigravity** (`.agents/workflows/`) em 2026-06 — ver [ADR-001](../analysis/onion-antigravity-migration-adr-2026-06.md). Mantido apenas como registro histórico.

# 🎯 Correção - Múltiplos Comandos Starter

## ❌ Problema Identificado

O wizard estava criando **apenas o comando `help.md`** para cada contexto, fazendo com que o Claude Code reconhecesse poucos comandos.

## ✅ Solução Implementada

### Comandos Criados Por Contexto

#### **Business Context** (`/.claude/commands/business/`)
1. **`help.md`** - Ajuda do contexto
2. **`spec.md`** - Criar especificação de feature
3. **`task.md`** - Criar task no task manager

#### **Technical Context** (`/.claude/commands/technical/`)
1. **`help.md`** - Ajuda do contexto
2. **`plan.md`** - Planejar implementação técnica
3. **`work.md`** - Iniciar desenvolvimento

#### **Customer Success Context** (`/.claude/commands/customer-success/`)
1. **`help.md`** - Ajuda do contexto
2. **`guide.md`** - Criar guia para cliente

### Código Implementado

```javascript
async createStarterCommands(context, commandsPath) {
  const starterCommands = {
    business: [
      { name: 'help', ...},
      { name: 'spec', ...},
      { name: 'task', ...}
    ],
    technical: [
      { name: 'help', ...},
      { name: 'plan', ...},
      { name: 'work', ...}
    ],
    'customer-success': [
      { name: 'help', ...},
      { name: 'guide', ...}
    ]
  };
  
  // Gera cada comando com conteúdo completo
  for (const cmd of commands) {
    await fs.writeFile(...);
  }
}
```

## 📊 Resultado Esperado

Após executar `onion init`, o Claude Code vai reconhecer:

### Business Context
- ✅ `/business/help`
- ✅ `/business/spec "feature-name"`
- ✅ `/business/task "task-title"`

### Technical Context
- ✅ `/technical/help`
- ✅ `/technical/plan "feature-name"`
- ✅ `/technical/work "feature-name"`

## 🚀 Teste Agora

```bash
cd ~/openspec
rm -rf .claude .onion .onion-config.yml docs/
onion init
# Seguir wizard: Monorepo → business,technical → Claude Code → None,None

# Verificar comandos criados
ls -la .claude/commands/business/
ls -la .claude/commands/technical/

# Testar no Claude Code
/business/help
/business/spec "minha-feature"
/technical/plan "minha-feature"
```

---

**Status**: ✅ CORRIGIDO  
**Próximo**: Testar no Claude Code 🎉

