---
title: "Ejercicio 4: supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste al agente en la nube de Copilot que se ocupara de tres tareas para mejorar la experiencia y añadir funcionalidad. Aunque trabaja de forma asíncrona y autónoma, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas de agentes con pull requests abiertos, examinar las operaciones realizadas y orientar las sesiones en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decidió añadir también un modo claro. Como ya estaba actualizando los estilos y creando controles, parecía lógico incluir esta funcionalidad. Orientarás al agente para que añada ambos modos.

## Revisa las tareas del agente en la nube

Consulta el estado de las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Examina las tareas que aparecen en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, como:
   - actualizar la documentación del código;
   - generar interfaces de programación de aplicaciones (API) para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - preparar el entorno de trabajo;
   - configurar servidores de Model Context Protocol (MCP);
   - realizar pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Examina también el panel de pull requests de la derecha. Desde allí puedes consultar el PR y sus archivos modificados.

## Dirige el agente en la nube

Ahora que has visto las tareas activas, pedirás a Copilot que incluya un control del modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión para añadir el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos a que la sesión indique que terminó la configuración y empezó a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube con el diálogo para dirigir la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al terminar el paso que estaba ejecutando cuando enviaste el mensaje.

La tarea puede tardar varios minutos. Aprovecha este tiempo para repasar lo que has aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro para supervisar y dirigir tareas del agente en la nube de GitHub Copilot. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o añadir indicaciones.

En este ejercicio:

- exploraste mission control y la página de agentes para supervisar las tareas del agente en la nube;
- dirigiste una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina la funcionalidad de accesibilidad, pasarás a [iterar en los pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: agentes personalizados][previous-lesson] | [Siguiente lección: iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
