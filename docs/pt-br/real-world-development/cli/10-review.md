---
title: "Lição 10: Encerramento e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo de trabalho contínuo na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem já incluída no backlog;
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e fez manualmente o merge do primeiro pull request (PR);
- começou pela issue de filtragem, definiu a abordagem no modo Plan, desenvolveu no modo Autopilot e revisou no modo Interactive;
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e usou-a para executar testes de unidade, lint e verificações de tipo;
- adicionou o servidor do Model Context Protocol (MCP) do Playwright e usou-o para explorar a filtragem em um navegador real;
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador;
- revisou a alteração completa de filtragem e autorizou o Agent Merge para o PR de filtragem;
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizada:

1. **Avaliações por estrelas:** exiba o `starRating` existente e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implemente a filtragem, atualize as instruções e aplique-as ao recurso, personalize o relatório `quality-checks`, crie um perfil de QA e inclua os testes relacionados.

Do planejamento da filtragem à abertura do PR, você usou a mesma conversa e branch. Combinamos esse trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, inspeção própria no navegador e exploração do navegador pelo Copilot usando MCP. A skill `quality-checks` executou testes de unidade, lint e verificações de tipo e relatou os resultados no novo formato. O QA reuniu esses resultados com uma revisão dos requisitos e da cobertura de testes antes do PR.

Os testes adicionados devem corrigir lacunas reais; uma execução de QA que não precise de novos testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas após qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot moldam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as conforme as necessidades da equipe. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações e os resultados reais das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** conforme a tarefa. Use **Plan** para definir uma abordagem antes de implementar, **Interactive** para acompanhar alterações direcionadas e **Autopilot** em tarefas com escopo definido. Escolha um modelo mais rápido para edições rotineiras e um modelo mais capaz para trabalhos complexos.

O contexto continua tão importante quanto a infraestrutura. Descrever claramente *o que* você quer desenvolver, *por que* e *como* altera significativamente o resultado.

## O que mais explorar

Você abordou o fluxo principal. Há mais alguns recursos da CLI que vale a pena conhecer:

- `/review` para pedir ao agente de revisão de código que analise as alterações;
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva;
- `/fleet` para coordenar tarefas independentes em paralelo;
- `/worktree` para isolar uma tarefa separada;
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximas etapas

A melhor forma de aprimorar o uso de qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você pensa em criar há anos. Compartilhe o que aprendeu com sua equipe e aprenda com as outras pessoas. E, como sempre, explore a documentação.

Se quiser ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop de VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
