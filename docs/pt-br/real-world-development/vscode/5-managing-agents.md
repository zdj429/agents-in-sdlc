---
title: "Exercício 5: Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma — explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo à medida que ocorrem. Você pode revisar cada diff em linha, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts adicionais para aprimorar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições de arquivo propostas;
- orientar o agente durante a sessão para adicionar um seletor do modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade que você usou no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez uma solicitação adicional: também gostaria de um seletor do modo claro para pessoas que consideram o tema padrão escuro demais. Em vez de iniciar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A visualização Copilot Chat mostra cada etapa executada pelo agente: arquivos explorados, comandos executados e edições propostas. Cada edição aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos, além de comandos de terminal que ele solicitará executar.
3. Quando o agente propuser uma edição, o diff aparecerá em linha:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazer a alteração e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão, por exemplo, para executar um comando de terminal, o agente pausará e mostrará a solicitação **Allow**. Aprove somente os comandos que considerar seguros.

5. Aguarde o agente concluir a implementação do modo de alto contraste. Você verá uma mensagem final resumindo as alterações feitas.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, você ampliará a mesma conversa para adicionar um seletor do modo claro. Como continuará na mesma sessão de chat, o agente manterá o contexto do trabalho anterior — incluindo os componentes alterados, a abordagem de estilo usada e o padrão de armazenamento local configurado.

1. No campo de entrada do Copilot Chat, envie este prompt complementar:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as alterações feitas e propondo complementos para o modo claro.
3. Revise cada nova edição à medida que aparecer, como fez para o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, se tentar refatorar a interface dos seletores de uma forma que você não queira —, interrompa-o com o botão **Stop** ao lado do campo de entrada do chat e envie uma mensagem complementar para esclarecer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para solicitar ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro seletor; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois seletores").

## Revisar os arquivos alterados

Antes de fazer commit do trabalho, revise rapidamente todos os arquivos alterados pelo agente.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações em componentes Astro, estilos e testes relacionados, se houver.
3. Abra alguns dos arquivos alterados e examine os diffs. Confirme que eles refletem os padrões de acessibilidade do agente personalizado: atributos ARIA, navegação pelo teclado, HTML semântico e persistência por meio do armazenamento local.
4. Quando estiver satisfeito, prepare as alterações e faça commit no painel Source Control. Você as publicará [na próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você explorou:

- como monitorar o trabalho de um agente em tempo real;
- como aceitar e rejeitar edições de arquivos individualmente;
- como orientar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará seu trabalho de acessibilidade como pull request e revisará as alterações produzidas pelo agente personalizado local ao longo deste laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
