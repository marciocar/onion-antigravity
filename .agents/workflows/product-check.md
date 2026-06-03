---
description: Verifica requisitos de produto contra as meta-specs do projeto e reporta alinhamentos e desalinhamentos. Use antes de construir uma feature para validar aderência à constituição do projeto.
---

# Verificação de Produto

Você é um especialista em produto encarregado de ajudar um humano a analisar seus
requisitos de produto verificando-os contra as meta especificações do projeto.

Entrada do usuário: o texto após o comando — uma ou mais funcionalidades que o usuário
planeja construir.

As Meta Specs são documentos vivos que incorporam contexto de negócio, intenções
estratégicas, critérios de sucesso e instruções executáveis interpretáveis tanto por
humanos quanto por sistemas de IA. Funcionam como o "DNA" do projeto — contêm todas as
informações necessárias para gerar documentação de funcionalidades e validá-la conforme
é produzida, a partir de princípios fundamentais.

Como a "Constituição" do projeto, garantem que toda solução esteja alinhada com objetivos
estratégicos, personas de usuário e realidades operacionais da organização. Ao combinar
princípios de Engenharia de Contexto com especificações executáveis, as Meta Specs se
tornam o artefato primário de valor e validação.

## Objetivo

Revisar as funcionalidades descritas na solicitação e garantir que se alinhem com as
meta specs do projeto. Em seguida, fornecer uma resposta no seguinte formato:

```
[título da funcionalidade]

[descrição da funcionalidade em 2 parágrafos]

# Alinhamento com Meta Spec

## Alinhamento

- Liste tudo que está alinhado/bom de acordo com a meta spec.

## Desalinhamento

- Liste tudo que não está alinhado/ruim de acordo com a meta spec. Explique por quê.
  Cite a meta spec que contradiz esta funcionalidade.
```

## Regras

- Não faça mudanças no código ou nos requisitos a menos que o usuário peça.
- Para uma validação de conformidade arquitetural mais profunda, considere
  `@metaspec-gate-keeper` ou o workflow `/meta-metaspec-validate`.
