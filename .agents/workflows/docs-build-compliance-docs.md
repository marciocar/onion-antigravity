---
description: Gerar arquitetura de documentação de compliance multi-framework (ISO 27001, ISO 22301, SOC2, PMBOK) em docs/compliance-context/.
---

# 📋 Gerador de Documentação de Compliance

Criar documentação de conformidade para auditorias e certificações.

Entrada do usuário: o texto após o comando. Parâmetros aceitos (como prosa): `frameworks` (opcional — ex.: `iso27001,soc2` ou `all`) e `due_diligence` (opcional — caminho para checklist de DD).

## 🎯 Objetivo

Gerar arquitetura completa de docs de compliance multi-framework.

## 🔧 Modos de Execução

- **Modo Seletivo**: `/docs-build-compliance-docs frameworks="iso27001,soc2"`
- **Modo Due Diligence**: `/docs-build-compliance-docs due_diligence="path/to/checklist.md"`
- **Modo Auto** (analisa projeto): `/docs-build-compliance-docs`
- **Modo Completo**: `/docs-build-compliance-docs frameworks="all"`

## ⚡ Fluxo de Execução

### Passo 1: Detectar Modo

- SE `frameworks` informado → Modo Seletivo
- SE `due_diligence` informado → Modo DD (analisar checklist)
- SENÃO → Modo Auto (analisar projeto)

### Passo 2: Selecionar Frameworks

| Framework | Foco | Quando Usar |
|-----------|------|-------------|
| ISO 27001 | Segurança da Info | Certificação, DD |
| ISO 22301 | Continuidade | DR, BCP |
| SOC2 | Trust Services | Clientes enterprise |
| PMBOK | Governança | Projetos |

### Passo 3: Delegar para Especialistas

Para cada framework selecionado:
- `iso27001` → `@iso-27001-specialist`
- `iso22301` → `@iso-22301-specialist`
- `soc2` → `@soc2-specialist`
- `pmbok` → `@pmbok-specialist`

Coordenação via `@security-information-master`.

### Passo 4: Gerar Documentação

Estrutura de saída:
```
docs/compliance-context/
├── index.md
├── iso27001/
│   ├── policy.md
│   ├── risk-assessment.md
│   └── controls.md
├── soc2/
│   ├── trust-services.md
│   └── evidence.md
└── reports/
    └── summary.md
```

### Passo 5: Validar e Entregar

## 📤 Output Esperado

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ DOCS DE COMPLIANCE GERADOS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 Frameworks:
∟ ISO 27001: ✅ 12 documentos
∟ SOC2: ✅ 8 documentos

📁 Estrutura:
∟ docs/compliance-context/index.md
∟ docs/compliance-context/iso27001/ (12)
∟ docs/compliance-context/soc2/ (8)

📋 Cobertura:
∟ Políticas: 100%
∟ Controles: 85%
∟ Evidências: Template

🚀 Próximo: Revisar e customizar
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## 🔗 Referências

- Orquestrador: `@security-information-master`
- ISO 27001: `@iso-27001-specialist`
- SOC2: `@soc2-specialist`

## ⚠️ Notas

- Docs gerados são templates base
- Customizar para contexto específico
- Revisar antes de auditorias
