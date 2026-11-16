---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em tarefas para aprimorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse uma funcionalidade de jogos relacionados para a equipe de design e implementasse recursos de acessibilidade, incluindo controles de alto contraste e modo claro. Agora vamos revisar as alterações e, se necessário, pedir melhorias ao Copilot.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a IA generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e a acessibilidade antes de habilitar a revisão do restante da equipe.

## Segurança e GitHub Copilot cloud agent

Como o Copilot cloud agent executa tarefas de forma assíncrona e sem supervisão contínua, existem limites de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** ao branch que usará para o código.
- O cloud agent é executado no GitHub Actions, em um ambiente separado e temporário.
- Os fluxos de trabalho do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar pela primeira pull request (PR) criada pelo GitHub Copilot cloud agent, que adiciona documentação ao código. Você usará a interface padrão de PR do GitHub.com.

> [!NOTE]
> A PR pode exibir um aviso de que o Copilot foi bloqueado por um firewall. Isso é esperado, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo servidores MCP. Se quiser, você pode [personalizar ou desabilitar o firewall do Copilot cloud agent][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra a PR intitulada algo como **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, a PR terá o indicador **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Quando a PR estiver pronta, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui comentários TSDoc e outra documentação. As alterações exatas variam. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários, provenientes dos arquivos de instruções personalizados vistos no início do Exercício 2.
6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho aparecerão na seção de verificações da PR. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

Você não precisa apenas aceitar o trabalho do Copilot. Também pode mencioná-lo em comentários da PR ou em comentários inline no código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. As sugestões possíveis incluem:

- adicionar cabeçalhos breves aos arquivos de código explicando sua finalidade.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot** como faria com qualquer pessoa da equipe. Use uma das sugestões acima ou peça outra melhoria relacionada à documentação.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Ele iniciará uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar à pull request.

   ![Voltar à pull request](../../../_images/ex7-back-to-pr.png)

4. Depois que o Copilot concluir as alterações, deverá aparecer um novo commit na pull request.
5. Selecione **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar a PR como pronta para revisão e mesclá-la ao branch principal.

![Marcar a PR como pronta para revisão](../../../_images/ex7-ready-for-review.png)

## Revise a funcionalidade de jogos relacionados

Agora, volte à PR criada pelo Copilot para mostrar jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** e revise o código gerado.
5. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
6. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Aguarde os fluxos de trabalho na seção de verificações da PR. As verificações do projeto para o aplicativo Astro devem ser aprovadas; isso pode levar alguns minutos.
9. **Opcional:** Para testar manualmente a funcionalidade, mude para o branch no Codespace. Substitua `<branch-name>` pelo nome criado pelo Copilot, como `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou a funcionalidade de jogos relacionados. Você pode solicitar atualizações iterativamente, por exemplo, para ajustar a quantidade de jogos exibidos ou adicionar cabeçalhos de comentários e comentários TSDoc. Observe que esta issue foi atribuída **antes** da atualização das instruções personalizadas. Para solicitar alterações, adicione um comentário na guia **Conversation**; o Copilot iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. A PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR intitulada algo como **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, a PR terá o indicador **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Selecione **Files changed** para revisar o código gerado.
5. Revise a implementação, com atenção especial a:
   - componentes de interface para alternar os modos.
   - armazenamento local usado para persistir as preferências.
   - alterações de CSS e estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a alternância de modos.
6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Aguarde os fluxos de trabalho na seção de verificações da PR. As verificações do projeto para o aplicativo Astro devem ser aprovadas; isso pode levar alguns minutos.
10. **Opcional:** Para testar manualmente os recursos de acessibilidade, mude para o branch no Codespace:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles de alto contraste e modo claro no navegador. Confirme se funcionam e se as preferências persistem após recarregar a página.

O agente personalizado de acessibilidade ajudou a orientar a implementação segundo práticas recomendadas. Se encontrar problemas ou melhorias, mencione **@copilot** em um comentário para solicitar alterações, como nas outras PRs.

## Exercício opcional — continue delegando

O cloud agent é mais útil quando recebe itens reais do backlog e você pode se dedicar a outras tarefas. Para praticar, crie mais issues no repositório e atribua-as ao Copilot. Por exemplo:

- criar um formulário de interesse de apoiadores na página de detalhes dos jogos.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros às funções auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do cloud agent. Nestas lições, você:

- **Inspecionou os arquivos de instruções personalizados do repositório** para observar mais tarde seu efeito na saída do cloud agent.
- **Atribuiu issues ao GitHub Copilot cloud agent** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para acompanhar e direcionar a sessão de acessibilidade.
- **Revisou e iterou sobre pull requests do cloud agent**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do cloud agent. Para continuar explorando, os outros percursos complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas no terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e a delegação opcional `/delegate` ao cloud agent.

No seu próprio repositório, experimente:

- atribuir ao cloud agent uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de trabalho de configuração do cloud agent][cloud-agent-best-practices]
- [Configurar o firewall do Copilot cloud agent][agent-firewall]

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
