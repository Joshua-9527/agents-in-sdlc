---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos exercícios anteriores, você pediu ao GitHub Copilot cloud agent que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o cloud agent trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao cloud agent, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você vê todas as tarefas do agente que têm pull requests (PRs) abertas. Também pode inspecionar as operações executadas e direcionar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do cloud agent.
- direcionar uma sessão em andamento para solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe percebeu que também seria útil adicionar um modo claro. Como o trabalho já inclui alterações de estilo e controles para alternar modos, você orientará a sessão para incluir o modo claro junto com o alto contraste.

## Revise as tarefas do GitHub Copilot cloud agent

Vamos conferir o estado das tarefas atribuídas ao GitHub Copilot cloud agent.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas, incluindo:
   - atualizar a documentação do código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em execução e revise as operações do Copilot, que podem incluir:
   - obter o código do repositório.
   - configurar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas listadas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request à direita, que permite acompanhar a PR e os arquivos alterados.

## Direcione o cloud agent

Agora que você conhece as tarefas ativas, peça ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato varia conforme o nome dado pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do cloud agent na página de agentes com a caixa de diálogo de orientação destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento e inclui a solicitação no fluxo de trabalho.

## Deixe o Copilot trabalhar

O Copilot incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento no momento do envio. Isso levará alguns minutos; aproveite a pausa para refletir sobre o que você aprendeu até aqui.

## Resumo e próximos passos

Este exercício explorou a página de agentes do Copilot, seu painel central para acompanhar e orientar tarefas do GitHub Copilot cloud agent. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento.

Você explorou:

- a página de agentes e o painel central para acompanhar tarefas do cloud agent.
- como redirecionar uma sessão em andamento e solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho de acessibilidade, vamos [iterar sobre as pull requests criadas pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
