---
title: "Exercício 6: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
| :-- |

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário. Usou o modo de agente para adicionar filtragem no cliente e no servidor, o servidor MCP do Playwright para testar manualmente esse trabalho no navegador, e depois um agente personalizado para implementar recursos de acessibilidade — seletores dos modos de alto contraste e claro — e orientou a sessão em andamento para ampliar o trabalho. Agora é hora de publicar esse trabalho local e revisá-lo como sua equipe faria.

### Cenário

Os fundamentos de design de software e DevOps não mudam com a chegada da IA generativa. Você ainda precisa revisar de verdade tudo o que o Copilot produz. Com isso em mente, vamos enviar as alterações de acessibilidade do codespace, abrir um pull request e examinar o diff antes de envolver o restante da equipe.

## Publicar os recursos de acessibilidade

Os seletores dos modos de alto contraste e claro implementados com o agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5] estão salvos como alterações com commit no codespace. Vamos enviá-los para uma branch e abrir um pull request para que o restante da equipe possa revisar.

1. Volte ao codespace.
2. Abra a visualização **Source Control** no VS Code.
3. Confirme que as alterações de acessibilidade estão com commit. Se houver alterações não registradas em commit do Exercício 5, prepare-as e faça commit agora com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** (ou use o menu **...** → **Push**).
5. O VS Code oferecerá a opção de abrir a nova branch no github.com. Aceite o aviso ou acesse o repositório manualmente e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro (por exemplo, `Add high-contrast and light-mode toggles`) e uma descrição curta que resuma o que foi feito e por quê.
7. Selecione **Create pull request**.
8. Quando o PR estiver aberto, selecione a guia **Files changed** para revisar todo o trabalho. Preste atenção especial a:
   - componentes de interface para alternar entre os modos;
   - uso do armazenamento local para manter as preferências do usuário;
   - alterações de CSS ou estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.);
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.

9. Volte à guia **Conversation**.
10. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos workflows. Se tudo correr bem, eles serão aprovados.

> [!TIP]
> Quer uma segunda opinião sobre o trabalho de acessibilidade? Mencione `@copilot` em um comentário no PR e peça, por exemplo, "revise este PR em busca de outros problemas de WCAG" ou "sugira melhorias na navegação pelo teclado". O Copilot iniciará uma nova sessão para responder ao comentário.

## Exercício opcional: continuar explorando localmente

Trabalhar de forma iterativa com um agente no IDE é uma habilidade que só se desenvolve com a prática. Algumas ideias para sessões adicionais no VS Code:

- Adicione um formulário de interesse para apoiadores na página de detalhes do jogo.
- Implemente paginação na lista de jogos.
- Adicione validação de entrada e tratamento de erros aos auxiliares de acesso a dados em `src/lib/`.
- Amplie o escopo do agente de acessibilidade — por exemplo, audite a ordem de foco do teclado em todo o site.

## Resumo

Parabéns — você concluiu o percurso do VS Code. Ao longo deste laboratório, você:

- **Usou o MCP do Playwright para testar manualmente o recurso.** Adicionou o servidor MCP do Playwright e deixou o Copilot controlar um navegador para verificar a funcionalidade de filtragem antes de abrir um pull request.
- **Conduziu o modo de agente por alterações coordenadas em toda a pilha.** Adicionou um recurso de filtragem que envolveu o cliente, o servidor e os testes em uma única sessão.
- **Usou um agente personalizado.** Selecionou o agente personalizado de acessibilidade no seletor de agentes e acompanhou a implementação do modo de alto contraste no repositório.
- **Gerenciou e orientou uma sessão de agente.** Revisou as alterações propostas em linha, aceitou as desejadas e ampliou a sessão com uma solicitação de modo claro.
- **Concluiu o ciclo com um pull request.** Publicou o trabalho local e o revisou de ponta a ponta como sua equipe faria.

## Revisão e próximas etapas

Este é o fim do percurso obrigatório do VS Code. Você pode parar aqui e considerar o workshop concluído.

Se quiser explorar outros recursos de agentes do Copilot, os outros percursos abordam cenários relacionados em interfaces diferentes:

- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente personalizado de QA, MCP do Playwright, Agent Merge e transferências de contexto.
- ☁️ **[Percurso do agente de nuvem](../../cloud/)** — concentre-se em atribuir issues ao agente de nuvem, monitorar sessões pela página de agentes e iterar de forma assíncrona em pull requests.

Você também pode continuar desenvolvendo o que começou aqui. O [awesome-copilot][awesome-copilot] é uma ótima fonte de arquivos de instruções, agentes personalizados e skills que você pode adaptar aos próprios projetos.

Como extensão opcional, [Opcional: Incorporar o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar o Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
