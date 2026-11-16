---
title: "Ejercicio 6: itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lección anterior: supervisión y gestión de agentes][previous-lesson] |
| :-- |

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario. Usaste el modo agente para añadir filtrado en el cliente y el servidor, el servidor Playwright MCP para probarlo manualmente en un navegador y, después, un agente personalizado para implementar funciones de accesibilidad —controles de alto contraste y modo claro— y dirigir la sesión para ampliar el trabajo. Ahora publicarás esos cambios locales y los revisarás como lo haría tu equipo.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Aun así, hay que revisar el trabajo de Copilot. Enviarás desde tu Codespace los cambios de accesibilidad, abrirás un pull request y revisarás el diff antes de involucrar al resto del equipo.

## Publica las funciones de accesibilidad

Los controles de alto contraste y modo claro que implementaste con el agente personalizado de accesibilidad en el [ejercicio 4][exercise-4] y el [ejercicio 5][exercise-5] están confirmados en tu Codespace. Los subirás a una rama y abrirás un pull request (PR) para que el equipo pueda revisarlos.

1. Vuelve a tu Codespace.
2. Abre la vista **Source Control** de VS Code.
3. Confirma que los cambios de accesibilidad están confirmados. Si tienes cambios sin confirmar del ejercicio 5, prepáralos y confírmalos ahora con un mensaje descriptivo, como `Add high-contrast and light-mode toggles`.
4. Publica la rama con **Publish Branch** (o abre el menú **...** y selecciona **Push**).
5. VS Code ofrecerá abrir la nueva rama en github.com. Acepta la propuesta o ve manualmente al repositorio y selecciona **Compare & pull request** en el aviso de la rama.
6. Escribe un título claro, por ejemplo `Add high-contrast and light-mode toggles`, y una descripción breve de los cambios y su propósito.
7. Selecciona **Create pull request**.
8. Cuando se abra el PR, selecciona **Files changed** para revisar todo el trabajo. Presta atención a:
   - los controles de interfaz para cambiar de modo;
   - el uso del almacenamiento local para conservar las preferencias del usuario;
   - los cambios de hojas de estilo en cascada (CSS) o de estilos para los modos de alto contraste y claro;
   - los atributos de aplicaciones de Internet enriquecidas y accesibles (ARIA), la navegación por teclado y otros aspectos;
   - el código JavaScript/TypeScript que gestiona los cambios de modo.
9. Vuelve a la pestaña **Conversation**.
10. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

11. Espera a que terminen los flujos de trabajo y comprueba que se completan correctamente.

> [!TIP]
> ¿Quieres una segunda opinión sobre el trabajo de accesibilidad? Menciona a `@copilot` en un comentario del PR y pídele, por ejemplo, que revise si hay otros problemas de las Pautas de Accesibilidad para el Contenido Web (WCAG) o sugiera mejoras para la navegación por teclado. Copilot iniciará una sesión para responder al comentario.

## Ejercicio opcional: sigue explorando localmente

Trabajar de forma iterativa con un agente en el entorno de desarrollo integrado (IDE) es una habilidad que se desarrolla con la práctica. Puedes probar estas ideas desde VS Code:

- añadir un formulario de interés para patrocinadores en la página de detalles del juego;
- implementar la paginación en la página de la lista de juegos;
- añadir validación de entradas y gestión de errores a los ayudantes de acceso a datos de `src/lib/`;
- ampliar el alcance del agente de accesibilidad, por ejemplo, auditando el orden del foco del teclado en todo el sitio.

## Resumen

¡Enhorabuena! Has completado el entorno de VS Code. En este taller:

- **Usaste Playwright MCP para probar manualmente la funcionalidad.** Añadiste el servidor Playwright MCP y dejaste que Copilot usara un navegador para verificar el filtrado antes de abrir un pull request.
- **Dirigiste el modo agente para coordinar cambios en varias capas.** Añadiste una funcionalidad de filtrado que afectó al cliente, al servidor y a las pruebas en una sola sesión.
- **Usaste un agente personalizado.** Seleccionaste el agente de accesibilidad en el menú de agentes y observaste cómo implementaba el modo de alto contraste en el repositorio.
- **Gestionaste y dirigiste una sesión de agente.** Revisaste los cambios propuestos en línea, aceptaste los que querías y ampliaste la sesión con una solicitud de seguimiento para el modo claro.
- **Cerraste el proceso con un pull request.** Publicaste el trabajo local y lo revisaste de principio a fin, como lo haría el equipo.

## Revisión y siguientes pasos

Has terminado el entorno obligatorio de VS Code. Puedes detenerte aquí y dar por completado el taller.

Si quieres ampliar tu experiencia con agentes de Copilot, los demás entornos exploran situaciones relacionadas mediante otras interfaces:

- 💻 **[Entorno de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y transferencias contextuales.
- ☁️ **[Entorno del agente en la nube](../../cloud/)**: asigna incidencias al agente en la nube, supervisa las sesiones desde la página de agentes e itera de forma asíncrona en los pull requests.

También puedes seguir desarrollando lo que has empezado. [awesome-copilot][awesome-copilot] ofrece archivos de instrucciones, agentes personalizados y habilidades de agente que puedes adaptar a tus proyectos.

Como extensión opcional, [Opcional: incorpora Foundry][exercise-7] usa VS Code y Microsoft Foundry Toolkit para preparar un modelo, implementar Backer Concierge y conectarlo al sitio.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: gestión de agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
