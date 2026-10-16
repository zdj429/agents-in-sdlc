---
title: "Exercício 2: Adicionar funcionalidade com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e tarefas como executar testes. Como pessoa desenvolvedora, seu fluxo geralmente envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando, fazer outra atualização... E assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para agir com mais autonomia no IDE. Ele se comporta de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar novas funcionalidades ao site.

> [!NOTE]
> Apesar dos nomes semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa as tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem atua como um par de programação, trabalhando de forma assíncrona como uma pessoa da equipe e interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- o modo de agente do GitHub Copilot pode implementar recursos na interface e na camada de dados;
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e distribuidora. Você já adicionou um auxiliar para distribuidoras no exercício anterior; agora concluirá com o modo de agente do Copilot o restante do trabalho na camada de dados, na interface e nos testes.

## Executar o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza as páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o ORM Drizzle — não há uma API de back-end separada nem uma estrutura de interface do lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um pequeno `<script>` Astro específico, usando APIs padrão do DOM.

### Iniciar o site

Para simplificar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Siga estas etapas para executá-la no GitHub Codespace:

1. Volte ao codespace. Você deverá estar na branch `main` após o exercício anterior.
2. Abra um terminal no codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e alterne para uma nova branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um banner com o URL, semelhante ao seguinte:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> No codespace, selecionar o link do URL localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para seu codespace, que agora hospeda o servidor Web.

### Explorar o site

Com o site em execução, explore as funcionalidades. Os principais recursos são:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página de detalhes com mais informações, como título, descrição, distribuidora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção do modo e do modelo varia por exercício — cada etapa indica os detalhes relevantes.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar examinando o backlog. Ao configurar o projeto a partir do modelo, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** no seletor de agentes da visualização Chat. O agente planeja e implementa alterações autonomamente em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> As pessoas autoras deste workshop não estão indicando preferência por um modelo. Durante a criação do workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções. Esperamos que as sugestões de código recebidas sejam relativamente consistentes para proporcionar uma boa experiência. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das descritas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Envie ao Copilot o prompt a seguir para perguntar sobre o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando e listar todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot também priorizou os itens com base nos que considera mais úteis para o usuário.

## Revisar os arquivos de instruções

Antes de iniciar o agente para gerar o código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a adição de funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais sobre como adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura;
   - princípios de design de componentes, capacidade de teste e acessibilidade;
   - links para arquivos de instruções específicos de tipos de arquivo, incluindo:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções permitem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso facilita a reutilização, a divisão de conjuntos complexos em partes menores e mais fáceis de gerenciar e a inclusão de exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, será necessário fazer pelo menos três atualizações separadas no aplicativo:

- adicionar ou refinar a lógica de filtragem restante na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento da filtragem;
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de integrar as alterações à base de código. O modo de agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no seletor de agentes da visualização Chat.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada no backlog usando este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe enquanto o Copilot explora o projeto e localiza os arquivos relacionados à funcionalidade. Ele deverá encontrar os auxiliares da camada de dados, a interface e os testes. Em seguida, começará a modificar os arquivos e executar os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Você verá o Copilot executar várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor de código. Quando o Copilot terminar, haverá um botão **Keep** ou **Undo** para cada alteração; não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando o Copilot solicitar, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez haja pausas e até falhas em alguns testes durante o processo. Tudo bem. O Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Sessão concluída do modo de agente do Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar novas funcionalidades ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar recursos na interface e na camada de dados;
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de integrá-los à base de código.

Agora vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus: implementar paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Use o que aprendeu neste exercício e peça ao Copilot que atualize o site para implementar a paginação. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções;
- decidir como implementar a paginação: permitir que o usuário selecione o tamanho da página ou defini-lo no código;
- fornecer ao Copilot orientações suficientes no prompt para obter a implementação desejada;
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
