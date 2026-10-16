---
title: "Exercício 3: Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme se ela realmente funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP estendem o Copilot com novas ferramentas;
- adicionar o servidor MCP do Playwright ao espaço de trabalho;
- pedir ao Copilot que use o servidor para testar manualmente a funcionalidade de filtragem no navegador;
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite que o Copilot faça isso — é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento interno do modo de agente e de como ele interage com contexto, LLMs e ferramentas — incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Com o MCP, os agentes podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos. O servidor MCP gerencia a comunicação entre o agente de IA e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP oferece ao agente de IA um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. O agente de IA pode usá-lo para criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador usando Playwright. O agente de IA pode usá-lo para acessar páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que oferecem acesso a diferentes ferramentas e recursos. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores MCP confiáveis e tenha cautela ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você deverá encontrar um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

A entrada `github` vem com o modelo do projeto. Por isso, o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalá-lo localmente: o VS Code se conecta por HTTP, e você se autentica no GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicionar o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] oferece ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, de modo que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia no computador executando `npx @playwright/mcp@latest`. A opção `--headless` instrui o Playwright a executar o navegador sem uma janela visível, o que é necessário em um codespace, que não tem uma área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que falta um navegador, peça para executar `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code solicitar a confirmação de confiança no servidor, revise a configuração e confirme que confia nele para permitir sua inicialização.

Iniciá-lo manualmente é opcional. Se você ignorar essa etapa, o VS Code iniciará o servidor e solicitará sua confiança na primeira vez que o Copilot precisar das ferramentas no próximo passo.

## Testar a funcionalidade de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de navegar pelo aplicativo para verificar seu trabalho, o agente pode abrir uma página, navegar, aplicar filtros e ler os resultados — e depois resumir o que observou. É uma forma rápida de confirmar que o recurso se comporta como esperado sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, e não capturas de tela. Assim, o agente raciocina sobre elementos estruturados e rotulados (botões, links, itens de lista), como faz a tecnologia assistiva. Dessa forma, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a funcionalidade de filtragem que você acabou de criar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, executará cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas complementares ou peça ao Copilot que corrija o código antes de abrir um pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para este teste. Se você tiver interrompido o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que falta um navegador, peça para executar `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Agora que confirmou que o recurso funciona, você pode abrir um pull request (PR) para que a equipe o revise. Primeiro, publique a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando (CLI) do GitHub. Como você já está trabalhando com o GitHub Copilot, peça que ele crie o PR. Ele pode localizar a issue relevante e associá-la ao PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado no seletor de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações necessárias. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja solicitada sua autenticação no GitHub. Siga as instruções.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria o PR.
6. Selecione o link criado pelo Copilot para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP estendem o Copilot com novas ferramentas;
- adicionou o servidor MCP do Playwright ao espaço de trabalho;
- usou o servidor para testar manualmente sua funcionalidade de filtragem no navegador antes de publicá-la;
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das formas de estender o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para simplificar tarefas direcionadas, como revisões de acessibilidade.

## Recursos

- [O que é o MCP e por que todo mundo está falando dele?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
