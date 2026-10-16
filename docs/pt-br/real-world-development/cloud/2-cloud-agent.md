---
title: "Exercício 2: Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações, se é que existe alguma, não enfrentam dívidas técnicas. Elas podem incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para realizar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um rascunho de pull request (PR) para revisão por uma pessoa desenvolvedora. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais amplas.

Neste exercício, você vai explorar:

- como personalizar o ambiente para a geração de código;
- como garantir que as operações sejam executadas com segurança;
- a importância de definir issues com escopo claro;
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. As pessoas contratadas inicialmente para criar a primeira versão do site não documentaram o código. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. Não é necessário que a implementação esteja refinada; a equipe quer apenas algo que possa usar para testar e aceitar a experiência do usuário. Essa tarefa é um bloqueio, mas há issues de prioridade mais alta no momento.

Esses são exemplos de tarefas que podem ser adiadas facilmente e que são boas candidatas para o agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto a pessoa desenvolvedora se concentra em outras atividades e, depois, revisar o trabalho para confirmar que está correto.

## Apresentar o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa da equipe, há várias formas de solicitar trabalho, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request preliminar para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto ela está em andamento ou depois da conclusão. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica — não é possível deixar de pensar. Mesmo operações aparentemente simples podem ser complexas quando analisadas em detalhes, por isso [defina com cuidado o escopo das tarefas do agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um par de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu um exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; se não, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, observe os arquivos de instruções incluídos no repositório para perceber sua influência depois.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace em execução:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica essas instruções a arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário no pull request gerado. Eles vêm desses arquivos de instruções. Voltaremos a falar disso ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem do Copilot

A criação de código geralmente exige um ambiente específico e a execução de scripts de configuração para deixar tudo em boas condições — independentemente de quem estiver trabalhando. O mesmo vale para tarefas atribuídas ao Copilot, que atua de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow especial de configuração][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. Assim, o agente tem acesso às ferramentas de desenvolvimento e dependências necessárias. Esse arquivo já foi configurado para facilitar o laboratório e proporcionar essa experiência de aprendizado. Ele disponibiliza Node.js, as dependências do projeto, os navegadores necessários para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para as etapas. O Copilot receberá seu próprio token para as operações.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se você não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo: Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que as compilações e os testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns detalhes importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o acionador `workflow_dispatch`, que permite executar manualmente o workflow na guia **Actions** do repositório. Isso é útil para confirmar que ele funciona, sem esperar que o Copilot o execute.

## Adicionar documentação

Embora todas as pessoas reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação alguma. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade, dificultando a manutenção da base de código e complicando a integração de novas pessoas à equipe. O Copilot é muito bom em criar documentação, por isso esta é uma issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária, que você revisará em um exercício futuro.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue** para criar uma issue.
5. No campo **Title**, insira `Code lacks documentation`.
6. No campo **Description**, insira:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, que terá um título semelhante a `[WIP]: Code lacks documentation`. Se o PR ainda não aparecer na lista, aguarde alguns instantes e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e determinando a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe a sessão em tempo real para acompanhar como o Copilot resolve o problema: ele explora o código, entende o estado atual, pausa para pensar e decidir a abordagem e, por fim, cria código.

Esse trabalho provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente de nuvem e, em seguida, voltando nossa atenção à criação de funcionalidades para o aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes do jogo

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Adicionar uma seção de jogos relacionados para a equipe de design talvez não leve muito tempo, mas esse tempo poderia ser usado em outras tarefas. Vamos atribuir o trabalho ao agente de nuvem do Copilot.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. No campo **Title**, insira `Show related games on the game details page`.
6. No campo **Description**, insira:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

    - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
    - Show a "Related games" section on the game details page that uses the helper
    - Handle the case where a game has no related games
    - There should be unit tests created for the new helper
    - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora, o Copilot está trabalhando na segunda solicitação. O agente de nuvem funciona de forma semelhante a uma pessoa engenheira de software. Você não precisa monitorá-lo ativamente; basta revisar o trabalho quando estiver concluído. Vamos nos concentrar na criação e no uso de agentes personalizados.

## Resumo e próximas etapas

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com o agente de nuvem, você pode atribuir issues ao Copilot para que ele trabalhe de forma assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar recursos ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam executadas com segurança;
- compreender a importância de definir issues com escopo claro;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
