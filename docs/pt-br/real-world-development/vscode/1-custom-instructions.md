---
title: "Exercício 1: Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma maneira específica — ou se o Copilot deve conhecer algumas informações de contexto —, esse contexto precisa estar acessível. Há várias maneiras de compartilhar contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções aplicáveis a caminhos específicos;
- gerar a primeira parte da camada de dados para a filtragem (um auxiliar de distribuidoras) com as instruções *atuais*;
- adicionar um novo padrão geral ao arquivo `.github/copilot-instructions.md`;
- enviar novamente o mesmo prompt e observar o código gerado incorporar o novo padrão;
- fazer commit das instruções atualizadas e da primeira parte da filtragem em `main`, para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode divergir de alguns dos padrões definidos. O Copilot é não determinístico. O objetivo do exercício é observar a *tendência* de mudança no comportamento após atualizar as instruções, e não obter exatamente a mesma saída, caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa ter testes de unidade.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para realizar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso ajuda a orientar o Copilot para que suas sugestões e seus trechos de código sejam mais relevantes. Você pode especificar convenções de código, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo e contexto de tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em **todas** as solicitações para o repositório. Ele deve conter informações gerais do projeto relevantes para a maioria das solicitações de chat ou CLI, como a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e outras orientações globais.
- Os arquivos `.github/instructions/*.instructions.md` podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes sobre linguagens específicas, como TypeScript ou Astro, ou tarefas, como criar um componente de interface ou um conjunto de testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre como criar arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos incluídos no projeto demonstram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções de `copilot-instructions.md` focadas em orientações gerais do projeto, como a descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para fornecer orientações específicas a tipos de arquivo, como testes de unidade, componentes Astro e a camada de dados, ou a tarefas específicas.
- Escreva em linguagem natural e mantenha as orientações claras. Inclua exemplos do formato esperado para o código.

Não há uma única forma correta de criar arquivos de instruções, assim como não há uma única forma de usar IA. Você descobrirá por meio de experimentação o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto robusto de arquivos de instruções. Ao explorar os arquivos deste projeto, você poderá encontrar instruções para várias tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece esse recurso de uma maneira (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição da interface que você estiver usando indicará quando isso for relevante.
>
> Procura modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos neste repositório: há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` na parte superior: ele define um padrão glob relativo à raiz do repositório que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript, como um que corresponda a `**/*.test.ts`, será incluído.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos que o Copilot deve seguir. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Criar uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e alterne para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção do modo e do modelo varia por exercício — cada etapa indica os detalhes relevantes.

Para observar o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para comparar os resultados.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot use somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para mostrar ao Copilot onde o auxiliar deve ficar.
3. Selecione **Agent** no seletor de agentes da visualização Chat para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela do seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt a seguir:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações ao código, geralmente incluindo o arquivo auxiliar e os testes.
6. Observe que a função auxiliar proposta tem tipos, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de distribuidoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, existe a possibilidade de ele adicionar comentários TSDoc mesmo sem essa orientação. Se isso acontecer, tudo bem — o objetivo é observar a melhoria de *consistência* após atualizar as instruções.

## Adicionar um novo padrão ao repositório

Como mencionado anteriormente, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões de código do projeto, mas ainda não exige documentação no código, por isso o auxiliar gerado não tinha comentários.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes para instruir o Copilot a incluir cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados na raiz do repositório (`.github/copilot-instructions.md`) para fornecer orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicos. O arquivo no nível do repositório é o local adequado para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Enviar novamente o prompt e observar a alteração

1. Volte ao Copilot Chat e selecione **New Chat** para limpar a conversa.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie **o mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão e aplicá-lo ao código real que será usado no próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são ativos do repositório como qualquer outro arquivo e são gerenciados pelo controle de versão. Vamos fazer commit e enviar a branch ao repositório.

1. Abra uma nova janela do terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as atualizações das instruções e do auxiliar estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções atualizadas e de todos os arquivos alterados pelo Copilot para a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar atualizações de suporte adicionais (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de um pull request

Com a branch enviada, vamos criar um pull request associado à issue de padrões de documentação do backlog. Em vez de fazer isso manualmente, peça ao Copilot que localize a issue, crie o PR para fechá-la e depois faça o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub em seu nome. O modelo do projeto já configura as ferramentas do GitHub no espaço de trabalho, então não é necessário configurar nada. Na primeira vez que o Copilot usar uma ferramenta, o VS Code poderá solicitar que você entre no GitHub. Siga as instruções. Em um exercício posterior, você aprenderá como essa conexão funciona (Model Context Protocol).

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que localize a issue sobre a atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo as atualizações e a base do auxiliar de distribuidoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando o Copilot solicitar permissão para executar ações do GitHub, revise o comando e selecione **Allow** conforme apropriado.
6. Quando o PR for criado, peça ao Copilot que faça o merge e volte para `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando o Copilot solicitar permissão para executar ações do GitHub e comandos de shell, revise o comando e selecione **Allow** conforme apropriado.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado usando as instruções *existentes*;
- adicionar um novo padrão geral a `.github/copilot-instructions.md`;
- enviar novamente o mesmo prompt e observar o código proposto incorporar o novo padrão;
- fazer commit das instruções e da base do auxiliar em `main` para que o próximo exercício possa usá-las.

Em seguida, você aplicará essas instruções no [modo de agente][next-lesson] enquanto o Copilot adiciona um novo recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
