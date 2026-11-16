---
title: "Ejercicio 2: añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso un cambio sencillo en una aplicación suele requerir modificar varios archivos y ejecutar tareas, como las pruebas. Hay que localizar los archivos, hacer los cambios, ejecutar las pruebas, depurar, descubrir qué archivo se ha pasado por alto y volver a modificarlo. El modo agente de Copilot puede coordinar este trabajo.

El modo agente trabaja con mayor autonomía en el entorno de desarrollo integrado (IDE). Explora la estructura del proyecto, realiza cambios en varios archivos, ejecuta tareas como las pruebas y corrige los problemas que encuentra. En este ejercicio lo usarás para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en tu IDE y permite ciclos de respuesta rápidos. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, mediante incidencias y pull requests.

En este ejercicio aprenderás a:

- dejar que el modo agente explore el proyecto y coordine los cambios en los archivos pertinentes;
- implementar funcionalidad en la interfaz de usuario (UI) y la capa de datos;
- revisar los cambios y las pruebas generados por Copilot antes de combinar el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir el filtrado por categoría y editorial. En el ejercicio anterior creaste un ayudante de editoriales; ahora completarás con el modo agente el resto del trabajo de la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys y su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz del lado cliente. Los ayudantes reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con scripts `<script>` de Astro acotados que usan las API estándar del Modelo de objetos del documento (DOM).

### Inicia el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro. Inicia el sitio en tu Codespace:

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de Astro esté listo, aparecerá un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost en el terminal, se redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al servidor web que se ejecuta en tu Codespace.

### Explora el sitio web

Con el sitio en marcha, explora sus funciones principales:

- **Página principal**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, accedes a su página de detalles, que muestra el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indica cuando corresponde.

La implementación inicial del sitio funciona, pero quieres añadir capacidades. El proyecto creó un backlog de incidencias automáticamente a partir de la plantilla. Pide a GitHub Copilot que lo muestre.

1. Selecciona **Agent** en el menú de agentes de Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no recomiendan un modelo concreto. Durante la creación del taller se usó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes, pero las respuestas pueden variar porque los modelos de lenguaje grandes (LLM) son probabilísticos. Es normal.

> [!NOTE]
> Por su naturaleza probabilística, Copilot puede usar un comando distinto de Model Context Protocol (MCP) y aun así completar la tarea.

3. Envía este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que Copilot enumere las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot prioriza las incidencias según la utilidad que podrían tener para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporciona contexto. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Examina las directrices, que incluyen:
   - una descripción de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones de Astro, Tailwind CSS (hojas de estilo en cascada) y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar orientación, dividirla en partes más manejables e indicar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que:

- añadir o ajustar la lógica restante en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de listado de juegos con la interfaz de filtrado.

Las pruebas deben ejecutarse correctamente antes de combinar los cambios. Pide al modo agente de Copilot que realice estas tareas.

1. Continúa la conversación actual o selecciona **New Chat** para empezar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú de agentes de Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Envía este prompt para que Copilot implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explora, modifica archivos y ejecuta pruebas; el proceso puede durar unos minutos. Mientras trabaja, pueden aparecer **Keep** y **Undo** en el editor. No tienes que seleccionarlos hasta que Copilot termine y puedas revisar todos los cambios.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que solicita confirmación para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y algunas pruebas pueden fallar durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

Has explorado cómo usar el modo agente de GitHub Copilot para añadir funcionalidad al sitio de Tailspin Toys. En concreto:

- dejaste que explorara el proyecto, identificara los archivos pertinentes y coordinara los cambios;
- usaste el modo agente para implementar cambios en la interfaz y la capa de datos;
- revisaste los cambios y las pruebas antes de combinar el código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás un pull request.

### Ejercicio de exploración adicional: implementa la paginación

A medida que crezca la lista de juegos, será necesario paginarla. Usa lo aprendido para pedir a Copilot que añada paginación. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes y usa los archivos de instrucciones;
- decide si el usuario podrá elegir el tamaño de página o si este será fijo;
- incluye en el prompt las indicaciones necesarias para conseguir el resultado deseado;
- es posible que tengas que iterar, pedir cambios y proporcionar contexto; es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
