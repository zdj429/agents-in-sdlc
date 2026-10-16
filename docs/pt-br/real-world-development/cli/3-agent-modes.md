---
title: "Lição 3: Modos de agente: Plan e Autopilot"
description: "Use Plan para definir a abordagem, Autopilot para desenvolver a filtragem a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. No entanto, alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo de trabalho existente da organização e ajudar a desenvolver as funcionalidades certas da maneira correta. Esta é a primeira de várias lições nas quais você seguirá um processo típico de desenvolvimento orientado por agentes: começará com uma issue para criar um recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, fará o merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch durante todo o fluxo de trabalho do recurso. Normalmente, seriam usadas branches ou PRs diferentes para os vários tipos de arquivo, mas faremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub;
- definir requisitos no modo Plan;
- implementar o novo recurso com o modo Autopilot;
- revisar o código;
- validar o recurso manualmente em um navegador encaminhado.

Enquanto desenvolve o recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e as pessoas precisam filtrar jogos por categoria e distribuidora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de começar a programar. Você usará o modo Plan para tomar essas decisões e, em seguida, autorizará uma implementação com escopo definido usando o Autopilot.

## Contexto

Incluir agentes de programação com IA no fluxo de trabalho não altera os fundamentos — na verdade, torna-os ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Começar por uma issue registrada que descreve o que precisa ser feito.
2. Criar um plano do que será desenvolvido.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, fazer o merge.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos fluxos segue esse padrão geral.

Seguir essa abordagem padrão garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua resposta antes de prosseguir.
- **Plan**: o agente cria primeiro um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pela issue

Antes de começar o trabalho de filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR de avaliação por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` ou um número semelhante. O caractere `#` permite adicionar ao contexto uma issue ou um pull request (PR) do GitHub.

## Planejar a funcionalidade de filtragem

O planejamento permite definir a abordagem para implementar um recurso ou realizar tarefas antes de encaminhá-las ao Copilot. Para qualquer tarefa complexa, é recomendável reservar algum tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, insira o seguinte prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a criar o plano. Primeiro, explorará o projeto e depois determinará a melhor abordagem.

3. Durante o processo, o Copilot poderá fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências. Não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a página para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise qualquer parte do plano que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar usando o modo Autopilot.

O Autopilot permite que o Copilot itere sobre o problema até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com nome semelhante na versão instalada.
2. Confirme que o indicador de modo abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, então revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Assim que o código for gerado, revise-o antes do merge, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva eventuais falhas.

## Explorar a nova funcionalidade

O código parece correto — mas será que funciona? Vamos iniciar o aplicativo como fizemos antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria e distribuidora, incluindo as combinações definidas no plano.
4. Confirme que a redefinição e o comportamento sem resultados correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que interrompa o servidor de desenvolvimento iniciado.

## Resumo e próximas etapas

Você usou diferentes modos de conversa para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub;
- definiu os requisitos no modo Plan;
- implementou o recurso com o modo Autopilot;
- revisou o código;
- validou o recurso manualmente em um navegador encaminhado.

Em seguida, vamos explorar mais a fundo como o código é gerado e garantir que siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
