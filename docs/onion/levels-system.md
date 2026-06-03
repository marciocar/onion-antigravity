# 📚 Sistema de Níveis - Onion v4.0

> **Versão**: 4.0.0 | **Atualizado**: 2025-12-20 | **Status**: Ativo

---

## 🎯 O Que É o Sistema de Níveis?

O Sistema de Níveis organiza **comandos e agentes** em 3 categorias baseadas na **frequência de uso** e **complexidade**, facilitando o aprendizado progressivo e reduzindo tempo de onboarding.

---

## 📊 Os 3 Níveis

### 🌟 Starter (80% dos casos)

**Definição**: Comandos essenciais para o trabalho diário.

**Características**:
- Cobrem 80% das necessidades do dia a dia
- Workflows mais comuns e diretos
- Baixa curva de aprendizado
- Alta frequência de uso

**Tempo de Onboarding**: 15-30 minutos

**Exemplos**:
- **Business**: `/business-spec`, `/business-task`, `/business-estimate`
- **Technical**: `/technical-work`, `/technical-plan`, `/technical-pr`

---

### 🔧 Intermediate (15% dos casos)

**Definição**: Workflows avançados e operações especializadas.

**Características**:
- Cobrem 15% das necessidades (casos mais complexos)
- Requerem domínio dos comandos starter
- Frequência moderada de uso
- Automações e integrações

**Tempo de Onboarding**: 30-60 minutos

**Exemplos**:
- **Business**: `/business-feature`, `/business-extract-meeting`, `/business-convert-to-tasks`
- **Technical**: `/technical-start`, `/technical-pre-pr`, `/technical-build-index`

---

### ⚡ Advanced (5% dos casos)

**Definição**: Casos específicos, edge cases e automações complexas.

**Características**:
- Cobrem 5% das necessidades (casos raros mas importantes)
- Requerem domínio de starter e intermediate
- Baixa frequência mas alto impacto
- Situações especializadas

**Tempo de Onboarding**: 45-90 minutos

**Exemplos**:
- **Business**: `/business-presentation`, `/business-analyze-pain-price`
- **Technical**: `/technical-bump`, `/technical-e2e`, `/technical-release-*`

---

## 🎓 Filosofia 80/15/5

### A Regra

Em um dia típico de trabalho:
- **80%** do seu tempo → Use apenas comandos **Starter**
- **15%** do seu tempo → Use comandos **Intermediate**
- **5%** do seu tempo → Use comandos **Advanced**

### Por Que Funciona?

1. **Foco no Essencial**: Aprenda apenas o necessário para ser produtivo
2. **Redução de Sobrecarga**: Não precisa dominar todos os 56 comandos de uma vez
3. **Descoberta Natural**: Comandos sugerem próximos passos conforme necessidade
4. **Progressão Orgânica**: Avança para níveis superiores quando realmente precisar

---

## 🚀 Descoberta Progressiva

### Como Funciona

Cada comando inclui navegação para facilitar descoberta:

#### Comandos Starter
```markdown
## 🚀 Próximos Passos

1. **Comando relacionado A**
2. **Comando relacionado B**
3. **Comando de nível superior**

💡 **Dica**: Insight ou melhor prática
```

#### Comandos Intermediate/Advanced
```markdown
## 📚 Pré-requisitos

Comandos starter que você deve dominar antes.

## 🚀 Próximos Passos

Comandos relacionados ou próximo nível.
```

### Exemplo Real

```
/business-spec (starter)
  └─ 🚀 Próximos Passos
      ├─ /business-estimate (starter)
      ├─ /business-task (starter)
      └─ /technical-start (intermediate)

/business-feature (intermediate)
  ├─ 📚 Pré-requisitos
  │   ├─ /business-spec ✓
  │   ├─ /business-estimate ✓
  │   └─ /business-task ✓
  └─ 🚀 Próximos Passos
      ├─ /business-task-check
      └─ /technical-start
```

---

## 📋 Guia de Onboarding

### Para Novos Usuários

#### Dia 1 (30-60 minutos)
**Objetivo**: Conhecer comandos starter do seu contexto principal.

**Business**:
1. Execute `/business-help`
2. Pratique `/business-spec "test-feature"`
3. Teste `/business-estimate "test-feature"`
4. Experimente `/business-task "test-feature"`

**Technical**:
1. Execute `/technical-help`
2. Pratique `/technical-plan "test-feature"`
3. Teste `/technical-work "test-task"`
4. Experimente `/technical-pr`

**Resultado**: Produtivo em workflows básicos.

---

#### Semana 1 (3-5 horas)
**Objetivo**: Dominar todos os starter + experimentar intermediate.

**Checklist**:
- [ ] Usar todos os starter commands pelo menos 1x
- [ ] Experimentar 2-3 intermediate commands
- [ ] Entender quando usar cada comando
- [ ] Explorar 2-3 agentes especializados

**Resultado**: Confortável com 80% dos workflows.

---

#### Mês 1 (10-20 horas)
**Objetivo**: Dominar intermediate, conhecer advanced.

**Checklist**:
- [ ] Usar intermediate commands conforme necessidade
- [ ] Experimentar 1-2 advanced commands
- [ ] Integrar sistema no workflow diário
- [ ] Contribuir com feedback/melhorias

**Resultado**: Expert no sistema, produtividade máxima.

---

## 🎯 Por Contexto

### Business Context

| Nível | Comandos | % Total | Tempo |
|-------|----------|---------|-------|
| **Starter** | 5 | 25% | 15 min |
| **Intermediate** | 10 | 50% | 30 min |
| **Advanced** | 5 | 25% | 45 min |
| **Total** | **20** | 100% | ~90 min |

**Agentes**: 12 especializados

**Foco**: Produto, requisitos, estimativas, tasks, reuniões.

---

### Technical Context

| Nível | Comandos | % Total | Tempo |
|-------|----------|---------|-------|
| **Starter** | 8 | 22% | 20 min |
| **Intermediate** | 13 | 36% | 40 min |
| **Advanced** | 14 | 39% | 60 min |
| **Total** | **36** | 100% | ~120 min |

**Agentes**: 23 especializados

**Foco**: Desenvolvimento, git, testes, docs, releases.

---

## 💡 Melhores Práticas

### 1. Comece Pequeno
Não tente aprender todos os comandos de uma vez.
- **Dia 1**: 5-8 comandos starter
- **Semana 1**: Todos os starter + 2-3 intermediate
- **Mês 1**: Domínio completo conforme necessidade

### 2. Use a Descoberta
Deixe os comandos guiarem você.
- Leia seção "🚀 Próximos Passos"
- Siga sugestões conforme contexto
- Pergunte ao `@onion` quando em dúvida

### 3. Não Force Advanced
Comandos advanced existem para casos específicos.
- Use apenas quando realmente precisar
- Não use "porque é avançado"
- Starter/Intermediate resolvem a maioria dos casos

### 4. Respeite Pré-requisitos
Comandos intermediate/advanced assumem familiaridade.
- Domine starter antes de intermediate
- Domine intermediate antes de advanced
- Leia seção "📚 Pré-requisitos"

### 5. Consulte o Help
Sistema de help é seu melhor amigo.
```bash
/business-help                    # Ver comandos por nível
/technical-help --level=starter   # Focar em starter
/help                             # Visão geral do sistema
```

---

## 🔄 Progressão Típica

### Semana 1-2: Starter Phase
```
Usuário novo
  ├─ Aprende 5-8 starter commands
  ├─ Usa diariamente
  └─ 80% produtivo
```

### Semana 3-4: Intermediate Exploration
```
Usuário confortável
  ├─ Domina todos starter
  ├─ Experimenta intermediate conforme necessidade
  └─ 95% produtivo
```

### Mês 2+: Advanced Mastery
```
Usuário expert
  ├─ Usa intermediate fluentemente
  ├─ Conhece quando usar advanced
  └─ 100% produtivo + ajuda outros
```

---

## 📊 Comparação: Antes vs Depois

### Antes do Sistema de Níveis (Onion v3)
- ❌ 90 comandos sem organização clara
- ❌ Tempo de onboarding: 4-8 horas
- ❌ Curva de aprendizado íngreme
- ❌ Usuários se perdiam facilmente
- ❌ Comandos raramente usados poluíam descoberta

### Depois do Sistema de Níveis (Onion v4)
- ✅ 56 comandos organizados em 3 níveis claros
- ✅ Tempo de onboarding: 15-30 minutos (90% redução!)
- ✅ Curva de aprendizado suave
- ✅ Descoberta progressiva guia naturalmente
- ✅ Foco no que importa (80% dos casos)

---

## 🎯 Estatísticas de Uso (Esperado)

Baseado em análise de workflows típicos:

### Business Context
- **Starter**: ~70% dos comandos executados diariamente
- **Intermediate**: ~25% dos comandos executados
- **Advanced**: ~5% dos comandos executados

### Technical Context
- **Starter**: ~75% dos comandos executados diariamente
- **Intermediate**: ~20% dos comandos executados
- **Advanced**: ~5% dos comandos executados

**Validação**: Filosofia 80/15/5 alinha com uso real!

---

## 🆘 FAQ

### "Devo aprender todos os comandos?"
Não! Comece com starter (80% dos casos). Explore intermediate/advanced conforme necessidade.

### "Quando passar para intermediate?"
Quando se sentir confortável com todos os starter e encontrar um caso de uso que starter não resolve.

### "Advanced é melhor que starter?"
Não! Advanced é **diferente**, não melhor. Use starter para 80% das tarefas.

### "Como saber qual nível usar?"
1. Consulte `/help` do seu contexto
2. Leia descrições de cada comando
3. Veja "🚀 Próximos Passos" para sugestões
4. Pergunte ao `@onion`

### "Posso pular níveis?"
Tecnicamente sim, mas não recomendado. Pré-requisitos existem por uma razão - comandos advanced assumem familiaridade com conceitos de starter/intermediate.

---

## 📚 Recursos Relacionados

### Documentação
- **Visão Geral**: `docs/knowledge-base/frameworks/onion-multi-context-orchestrator-vision.md`
- **Ciclo Completo**: `docs/knowledge-base/frameworks/onion-complete-cycle-understanding.md`
- **READMEs**: `.agents/workflows/` (workflows organizados por prefixo de categoria)

### Comandos Help
- `/help` - Visão geral do sistema
- `/business-help` - Comandos business por nível
- `/technical-help` - Comandos technical por nível

### Agentes
- `@onion` - Orquestrador principal, ajuda com navegação
- `@product-agent` - Business workflows
- `@react-developer` - Technical development

---

## 🔄 Evolução do Sistema

### v4.0 (Atual)
- ✅ Sistema de 3 níveis implementado
- ✅ Descoberta progressiva em 28 comandos
- ✅ Help inteligente por contexto
- ✅ Filosofia 80/15/5 estabelecida

### v4.1 (Futuro)
- 🔮 Analytics de uso real para validar 80/15/5
- 🔮 Expansão de descoberta para todos os 56 comandos
- 🔮 Sistema de badges/achievements por nível
- 🔮 Onboarding interativo guiado

---

**Sistema Onion v4.0** - Multi-Context Development Orchestrator 🧅

**Filosofia**: *Aprenda menos, produza mais.*

