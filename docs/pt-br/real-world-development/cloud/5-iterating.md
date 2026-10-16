---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse um recurso de jogos relacionados para a equipe de design explorar e implementasse recursos de acessibilidade, incluindo seletores dos modos de alto contraste e claro. Vamos explorar as alterações no código e, se necessário, enviar comentários ao Copilot para melhorar o trabalho antes de habilitar a revisão pelo restante da equipe.

### Cenário

Como já foi destacado várias vezes, os fundamentos de design de software e DevOps não mudam com a chegada da IA generativa. Sempre queremos revisar o código gerado e seguir nosso processo habitual de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão pela equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há algumas restrições de segurança para manter o processo seguro. Entre elas:

- O Copilot tem acesso somente de leitura ao repositório e acesso de gravação **somente** à branch usada para o código.
- O agente de nuvem é executado no GitHub Actions, onde cria um ambiente separado e efêmero para trabalhar.
- Os workflows do GitHub Actions precisam ser aprovados por uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revisar a documentação gerada

Vamos começar explorando o primeiro pull request (PR) gerado pelo agente de nuvem do GitHub Copilot: a adição de documentação ao código. Para isso, usaremos a interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao explorar o PR, você poderá ver um aviso de que o firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o acesso do Copilot a recursos externos, incluindo chamadas a servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation** ou mais detalhado.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui comentários TSDoc e outra documentação. As alterações exatas podem variar.

   Ao revisar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles resultam dos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode mencionar o Copilot em comentários do pull request ou comentários em linha no código, como faria com outras pessoas da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não há um texto específico que possamos recomendar. Algumas ideias do que pedir ao Copilot:

- Adicione cabeçalhos de comentário no início de cada arquivo de código com uma descrição breve de sua finalidade.
- Adicione comentários TSDoc a arquivos TypeScript e Astro.
- Crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como o Copilot inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, você verá um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e integrá-lo à branch principal.

![Marcar PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Vamos voltar ao PR que o Copilot gerou para resolver a issue sobre mostrar jogos relacionados na página de detalhes de um jogo.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou mais detalhado.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** Você pode alternar para essa branch no Codespace e testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode trabalhar de forma iterativa com o agente de nuvem do Copilot para solicitar atualizações. Por exemplo, você pode pedir que o Copilot ajuste o número de jogos relacionados exibidos ou adicione cabeçalhos de comentário e comentários TSDoc (lembre-se: essa tarefa foi atribuída **antes** de você atualizar as instruções personalizadas). Como antes, faça essas solicitações adicionando um comentário na guia **Conversation**. O Copilot verá o comentário e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Esse PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no centro de controle no Exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website** ou mais detalhado.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de interface para alternar entre os modos;
   - uso do armazenamento local para manter as preferências do usuário;
   - alterações de CSS ou estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.);
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** Você pode alternar para essa branch no Codespace para testar os recursos de acessibilidade manualmente. Acesse o Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os seletores dos modos de alto contraste e claro no navegador para confirmar que funcionam e mantêm as preferências após recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot na implementação dos recursos conforme as práticas recomendadas de acessibilidade. Se encontrar problemas de acessibilidade ou oportunidades de melhoria, você pode mencionar **@copilot** em um comentário para solicitar atualizações, como fez nos PRs anteriores.

## Exercício opcional: continuar delegando

O agente de nuvem funciona melhor quando você pode atribuir a ele itens reais do backlog e cuidar de outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Crie um formulário de interesse de apoiadores na página de detalhes do jogo.
- Implemente paginação na lista de jogos.
- Adicione validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos no repositório** para observar como eles influenciam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar seletores dos modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como centro de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximas etapas

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma integração opcional `/delegate` com o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
