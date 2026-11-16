---
title: "Ejercicio 5: iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia y añadir funcionalidad. Le pediste que documentara el código, creara una funcionalidad de juegos relacionados y añadiera funciones de accesibilidad, incluidos los modos de alto contraste y claro. Ahora revisarás los cambios y, si es necesario, pedirás a Copilot que los mejore.

### Escenario

Los principios de diseño de software y DevOps no cambian con la IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual. Revisarás las propuestas de documentación, juegos relacionados y accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, hay varias restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para el código.
- El agente en la nube se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores de Model Context Protocol (MCP).

## Revisa la documentación generada

Empieza por el primer pull request (PR) que generó el agente en la nube: el que añade documentación al código. Usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que aparezca una advertencia que indique que el firewall ha bloqueado a GitHub Copilot. Es lo esperado: por defecto, Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre el PR cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, el PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Cuando el PR esté listo, selecciona **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que incluirá comentarios TSDoc (documentación de TypeScript) y otra documentación. Los cambios concretos pueden variar. Busca comentarios TSDoc, convenciones TypeScript y encabezados de comentarios: proceden de los archivos de instrucciones que examinaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el indicador de flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones del PR. Si todo va bien, las comprobaciones del proyecto deberían completarse correctamente para la aplicación Astro. Puede tardar varios minutos.

## Pide cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios del PR, igual que a otros miembros del equipo, o añadir comentarios en línea al código. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay un prompt único. Puedes pedir, por ejemplo:

- añadir encabezados de comentarios al principio de cada archivo de código con una descripción breve;
- añadir comentarios TSDoc a archivos TypeScript y Astro;
- crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**. Puedes usar una de las ideas anteriores u otra petición relacionada con la documentación.
2. Selecciona **View Session** para observar cómo Copilot inicia una sesión nueva y aplica los cambios.
3. Selecciona **Back to pull request** para volver al PR.

   ![Volver al pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, busca la confirmación nueva en el PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar el PR como listo para revisión y combinarlo en la rama principal.

![Marcar el PR como listo para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve al PR que Copilot generó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre el PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
6. Busca el indicador de flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones del PR y que las comprobaciones del proyecto se completan correctamente. Puede tardar varios minutos.
9. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la funcionalidad de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes iterar pidiendo, por ejemplo, que ajuste el número de juegos relacionados o que añada encabezados de comentarios y comentarios TSDoc. Recuerda que esta incidencia se asignó **antes** de actualizar las instrucciones personalizadas. Añade un comentario en la pestaña **Conversation** para que Copilot inicie otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisarás el PR implementado con el agente personalizado de accesibilidad. Debe incluir tanto el modo de alto contraste solicitado en el ejercicio 3 como el modo claro que pediste en mission control durante el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre el PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, el PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar varios minutos.

4. Selecciona **Files changed** y revisa el código generado.
5. Revisa la implementación y presta atención a:
   - los controles de interfaz para cambiar de modo;
   - el uso del almacenamiento local para guardar las preferencias;
   - los cambios de hojas de estilo en cascada (CSS) o de estilos para los modos de alto contraste y claro;
   - los atributos de aplicaciones de Internet enriquecidas y accesibles (ARIA), la navegación por teclado y otros aspectos;
   - el código JavaScript/TypeScript que gestiona los cambios de modo.
6. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
7. Busca el indicador de flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones del PR y que las comprobaciones del proyecto se completan correctamente. Puede tardar varios minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionan y que la configuración se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones según las prácticas recomendadas. Si detectas problemas de accesibilidad o mejoras posibles, menciona a **@copilot** en un comentario para pedir cambios, igual que en los otros PR.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle tareas reales del backlog y ocuparte de otra cosa. Para practicar, crea más incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para mostrar interés en un juego en la página de detalles;
- implementar la paginación en la página de la lista de juegos;
- añadir validación de entradas y gestión de errores a los ayudantes de acceso a datos.

## Resumen

Has completado el entorno de agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizados del repositorio** para observar su efecto en el trabajo del agente.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir controles de alto contraste y modo claro.
- **Usaste la página de agentes de Copilot como mission control** para supervisar y dirigir la sesión de accesibilidad.
- **Revisaste e iteraste en los pull requests del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has terminado el entorno de agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP desde el entorno de desarrollo integrado (IDE).
- 💻 **[Entorno de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con Plan, Autopilot, habilidades de agente, un agente de QA, Playwright MCP, Agent Merge y la opción `/delegate` para volver a cloud agent.

En tu propio repositorio, también puedes:

- asignar al agente una incidencia de refactorización o cobertura de pruebas;
- crear un agente personalizado para otro ámbito;
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignación de incidencias de GitHub a Copilot][assign-issue]
- [Procedimientos recomendados para configurar el agente en la nube][cloud-agent-best-practices]
- [Configuración del firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
