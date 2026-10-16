---
title: "Exercício 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados e adaptados a tarefas ou domínios específicos do fluxo de trabalho de desenvolvimento. Ao definir agentes por meio de arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções direcionadas, práticas recomendadas, padrões de código e conhecimento específico do domínio para ajudá-lo a executar determinados tipos de trabalho com mais eficiência. As equipes podem transformar sua experiência em agentes reutilizáveis — um agente de acessibilidade que aplica as diretrizes de [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes de teste.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguido por um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Há uma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente por arquivos Markdown e orientam uma IA sobre como realizar operações. A forma mais simples de diferenciá-los é: um **agente personalizado** é quem executa o trabalho; as **skills** são ferramentas.

Os agentes personalizados têm sua própria janela de contexto e são criados para orquestrar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site conforme as diretrizes de acessibilidade; como parte desse trabalho, ele poderia chamar skills como uma skill de fluxo de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não existe uma única maneira "correta" de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para descobrir o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai explorar:

- como os agentes personalizados são definidos;
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys tem o compromisso de garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicam que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é fundamental, queremos implementar esse recurso o mais rápido possível. Você usará um agente personalizado para gerar a funcionalidade.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e revise o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível;
   - práticas recomendadas de acessibilidade;
   - exemplos de código HTML, CSS e JavaScript;
   - erros e armadilhas comuns.

## Criar e atribuir uma issue

O centro de controle é o local principal para trabalhar com todos os agentes do seu ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorá-las e até redirecionar e fornecer orientações adicionais. Vamos começar atribuindo ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue** para criar uma nova issue.
5. No campo **Title**, insira `Add high contrast mode to website`.
6. No campo **Description**, insira:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Captura de tela da atribuição do agente de nuvem, com o agente personalizado Accessibility destacado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximas etapas

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados e adaptados a tarefas e domínios específicos. Com agentes personalizados, você pode codificar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos;
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot trabalha na implementação do modo de alto contraste, vamos [monitorar e orientar a sessão do agente][next-lesson] pelo centro de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar-se para usar agentes personalizados na organização][org-custom-agents]
- [Preparar-se para usar agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
