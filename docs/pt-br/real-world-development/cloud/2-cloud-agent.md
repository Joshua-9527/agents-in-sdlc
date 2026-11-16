---
title: "Exercício 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É comum que as organizações enfrentem dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que permanecem no backlog por falta de tempo. O GitHub Copilot cloud agent pode atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria uma pull request (PR) de rascunho para revisão de um desenvolvedor. Assim, você pode delegar tarefas repetitivas e se concentrar em questões mais abrangentes.

Neste exercício, você explorará:

- a personalização do ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de issues bem delimitadas.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver parte da dívida técnica. Os prestadores de serviço que criaram a primeira versão do site não documentaram o código. Como primeiro passo, a equipe quer adicionar comentários TSDoc às funções exportadas do aplicativo.

Além disso, a equipe de design quer aprimorar a experiência do usuário (UX) na descoberta de jogos. Ela quer mostrar jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo. Por enquanto, precisa de uma implementação simples para testes de aceitação de UX. Embora isso seja um bloqueio, há issues mais prioritárias.

Esses são exemplos de tarefas que podem perder prioridade e ser delegadas ao GitHub Copilot cloud agent. Ele pode trabalhar de forma assíncrona enquanto você se dedica a outras tarefas e, depois, você revisa o trabalho.

## Conheça o GitHub Copilot cloud agent

O [GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano, de forma semelhante a um desenvolvedor. Você pode, por exemplo, [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria uma pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode inspecionar a sessão durante a execução ou depois da conclusão. Quando a solução estiver pronta para revisão, o Copilot marcará você na pull request.

## A importância de instruções bem delimitadas

O GitHub Copilot não é mágico: não elimina a necessidade de pensar. Até tarefas aparentemente simples têm complexidades; por isso, [defina o escopo das tarefas com cuidado][cloud-agent-best-practices]. Trate o agente como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software continuam os mesmos.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas. Se você já fez a atividade prática, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao cloud agent, leia os arquivos de instruções sem alterá-los para observar seu efeito mais tarde.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, especialmente as convenções de TypeScript e as expectativas para comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Quando atribuir a issue `Code lacks documentation` ao cloud agent na próxima seção, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários na pull request. Essas práticas vêm dos arquivos de instruções e serão retomadas na revisão posterior.

## Configure o ambiente de desenvolvimento do cloud agent

A criação de código normalmente exige um ambiente específico e scripts de configuração. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de modo semelhante a um engenheiro de software.

O cloud agent usa [GitHub Actions][github-actions] como ambiente de execução. Você pode personalizá-lo com um [fluxo de trabalho de configuração especial][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho e capaz de instalar ferramentas e dependências. Esse arquivo já está configurado no projeto para o laboratório: disponibiliza Node.js, dependências, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido do aplicativo Astro.

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar na pull request.
- O gatilho `workflow_dispatch` permite executar manualmente o fluxo de trabalho na guia **Actions** do repositório. Isso é útil para testá-lo sem esperar que o Copilot o execute.

## Adicione a documentação ao código

Muitos projetos têm documentação desatualizada ou inexistente, o que reduz a produtividade e dificulta a manutenção e a integração de desenvolvedores. O Copilot pode ajudar a criar documentação, por isso esta é uma issue adequada para atribuir ao cloud agent. Mais adiante, você revisará o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra a nova pull request, intitulada de modo semelhante a `[WIP]: Code lacks documentation`. Se ela ainda não aparecer, aguarde um momento e atualize a página.
12. Após alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista pode demorar alguns minutos para aparecer. O Copilot está configurando o ambiente com o fluxo de trabalho descrito anteriormente, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo da pull request e procure uma atualização que informe que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Acompanhe a sessão em tempo real: o Copilot explora o código e o estado do projeto, planeja o trabalho e cria código.

Isso pode levar alguns minutos. O cloud agent permite trabalhar de forma assíncrona e se dedicar a outras tarefas. Vamos aproveitar esse recurso atribuindo outra tarefa e, em seguida, criando uma funcionalidade no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma vantagem do GitHub Copilot cloud agent é poder dividir o trabalho. Enquanto ele implementa uma funcionalidade de jogos relacionados para a equipe de design, você pode cuidar de outras tarefas.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues** e, em seguida, **New issue**.
3. Selecione **Blank issue**.
4. Defina **Title** como `Show related games on the game details page`.
5. Defina **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecione **Create** para criar a issue.
7. À direita, selecione **Assign to Copilot**.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

8. Selecione **Assign**.

Em pouco tempo, o primeiro comentário da issue deverá exibir o emoji 👀, indicando que o Copilot está trabalhando. O agente agora está cuidando da segunda solicitação. Como o cloud agent trabalha de maneira assíncrona, você não precisa acompanhá-lo continuamente; revise o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o [GitHub Copilot cloud agent][copilot-agents]. Você pode atribuir issues ao Copilot para execução assíncrona e usá-lo para resolver dívida técnica, criar funcionalidades ou ajudar a migrar código entre frameworks.

Você explorou:

- a personalização do ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de issues bem delimitadas.
- como atribuir issues ao Copilot.

Enquanto o cloud agent trabalha, vamos criar e usar agentes personalizados. O [cloud agent também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e as instruções personalizadas que você explorou nos módulos anteriores.

## Recursos

- [Sobre o Copilot cloud agent][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de trabalho de configuração do cloud agent][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
