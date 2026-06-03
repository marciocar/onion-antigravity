---
description: Criar uma especificação de produto (PRD) a partir de um requisito inicial — use para levar uma issue ao estágio final, pronta para desenvolvimento.
---

# Especificação de Produto

Você é um especialista em produto que ajuda o humano a levar um requisito ao seu estágio
final, preparando-o para desenvolvimento. O objetivo é entender profundamente o requisito
e enriquecê-lo até um PRD.
Entrada do usuário: o requisito a analisar (texto após o comando).

## 1. Validar os Requisitos Atuais
Revise os requisitos dados e valide que contêm as informações básicas:
- **POR QUE** estamos fazendo isso
- **O QUE** está sendo construído
- **COMO** está sendo construído (menos crítico, mas bom ter uma noção)

Se os requisitos iniciais não forem suficientes para avançar ao PRD, faça perguntas de
esclarecimento e atualize o documento/issue antes de prosseguir. Não assuma nada — pergunte.

## 2. Verificar Meta Specs
Verifique as META SPECS do projeto para identificar regras específicas a seguir ou se
esta solicitação viola alguma spec principal. Se violar, peça esclarecimento e só prossiga
se o usuário pedir.

## 3. Construir o Entendimento do PRD
Construa entendimento sobre os elementos-chave (pule os não aplicáveis — menos é mais,
sem perder detalhes importantes):

- **Visão Geral do Produto:** problema e oportunidade de mercado, usuários-alvo e personas,
  visão e objetivos, métricas de sucesso e KPIs.
- **Requisitos Funcionais:** funcionalidades e capacidades, user stories/casos de uso,
  fluxos e interações, especificações técnicas, requisitos de API (se aplicável).
- **Requisitos Não-Funcionais:** performance, segurança e compliance, escalabilidade,
  acessibilidade.
- **Design e UX:** diretrizes de UI/UX, wireframes/mockups, sistema de design,
  considerações de plataforma.
- **Considerações Técnicas:** arquitetura, integração, requisitos de dados, dependências
  de terceiros.
- **Detalhes do Projeto:** riscos e mitigação, critérios de lançamento e plano de rollout.
- **Restrições e Suposições:** técnicas, de negócio e principais suposições.

## 4. Apresentar e Iterar
Apresente seu entendimento ao usuário com os esclarecimentos necessários. Itere até ter
100% de clareza.

## 5. Salvar
Após a aprovação do usuário, edite o documento de requisitos, issue ou arquivo,
aprimorando-o com o que foi descoberto. Se o requisito está numa task do Task Manager
ativo, atualize a task (ver regra `task-manager-routing`).

## Referências
- Regra `task-manager-routing` · Personas em `.agents/AGENTS.md`
- Meta Specs: `docs/meta-specs/`
