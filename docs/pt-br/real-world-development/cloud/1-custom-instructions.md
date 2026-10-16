---
title: "Exercício 1: Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma maneira específica — ou se o Copilot deve conhecer algumas informações de contexto —, esse contexto precisa estar acessível. Os [arquivos de instruções][instruction-files] fornecem essa orientação para que o Copilot entenda não apenas *o que* você quer, mas também *como* deseja que o trabalho seja feito.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções aplicáveis a caminhos específicos;
- adicionar um novo padrão geral ao arquivo `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, aqui você não executará um prompt *antes e depois*. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests produzidos pelo agente de nuvem.

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

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em **todas** as solicitações para o repositório. Ele deve conter informações de contexto relevantes para a maioria das solicitações de chat ou CLI, como a pilha de tecnologias, uma visão geral do projeto, as práticas recomendadas e orientações gerais.
- Os arquivos `.github/instructions/*.instructions.md` podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes sobre linguagens específicas, como TypeScript ou Astro, ou tarefas, como criar um componente de interface ou um conjunto de testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre como criar arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto demonstram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções de `copilot-instructions.md` focadas em orientações gerais do projeto, como a descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para fornecer orientações específicas a tipos de arquivo, como testes de unidade, componentes Astro e a camada de dados, ou a tarefas específicas.
- Escreva em linguagem natural e mantenha as orientações claras. Inclua exemplos do formato esperado para o código.

Não há uma única forma correta de criar arquivos de instruções, assim como não há uma única forma de usar IA. A experimentação ajudará você a encontrar a abordagem mais adequada ao projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto robusto de arquivos de instruções. Ao explorar os arquivos deste projeto, você poderá encontrar instruções para vários tipos de tarefa, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
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
2. Explore o arquivo e observe a breve descrição do projeto, além de seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. A seção **Code standards** contém orientações aninhadas sobre **GitHub Actions Workflows**. Essas instruções se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` na parte superior: ele define um padrão glob relativo à raiz do repositório que determina a quais arquivos as instruções se aplicam. Aqui, qualquer arquivo de teste TypeScript, como um que corresponda a `**/*.test.ts`, será incluído.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Assim, conjuntos maiores de instruções podem ser divididos em arquivos menores e reutilizáveis, e o Copilot pode ser direcionado a exemplos existentes. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicionar um novo padrão ao repositório

A única edição que você fará aqui é adicionar um padrão geral ao projeto: a documentação deve estar no código, em comentários TSDoc, e cada arquivo deve ter um cabeçalho de comentário. O agente de nuvem usará essas orientações ao trabalhar nas issues que você atribuir mais adiante neste percurso.

Antes de editar, crie uma branch para trabalhar. O agente de nuvem lerá as instruções da branch de destino da issue, mas você também fará o commit das alterações de forma organizada:

1. No terminal do codespace, crie e alterne para uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como mencionado anteriormente, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões de código do repositório para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões de código do projeto, mas ainda não exige documentação no código, por isso a função auxiliar gerada não tinha comentários.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes para instruir o Copilot a incluir cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados na raiz do repositório (`.github/copilot-instructions.md`) para oferecer orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicos. O arquivo no nível do repositório é o local adequado para padrões gerais do projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Fazer commit, enviar e integrar a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, o Copilot criará uma branch a partir de `main`. Portanto, suas instruções precisam estar em `main` para que o agente de nuvem as use.

1. Prepare as alterações e faça o commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem lerá essas instruções ao trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` durante este workshop, ignore a criação da branch e faça o commit diretamente em `main`. A etapa da branch está aqui para que o workshop reproduza o fluxo de trabalho de um projeto real.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e adicionou um novo padrão geral a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests gerados pelo agente de nuvem ao longo deste percurso.

Em seguida, você [atribuirá sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
