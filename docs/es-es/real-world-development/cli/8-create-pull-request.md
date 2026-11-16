---
title: "Lección 8: crea y combina el pull request de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la habilidad, el perfil de control de calidad (QA) y las pruebas; después crea un pull request y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Ahora los revisarás juntos y abrirás un pull request. Combinaste personalmente el pull request (PR) de valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la habilidad y el agente de QA en varios PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación;
- crearás el PR de filtrado;
- habilitarás Agent Merge después de la revisión y confirmarás que el PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante del PR sin quitar al desarrollador el control sobre la autorización de la combinación.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar un pull request. Cuando lo habilitas, Copilot resuelve los bloqueos del PR —corrige errores de integración continua (CI), responde a comentarios de revisión y vuelve a aplicar los cambios cuando es necesario— y, después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de habilitarlo.

## Usa Agent Merge para gestionar el PR

Revisa el código, crea el PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el diálogo de agentes con `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea un PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua del PR. Cuando todo termine correctamente, realizará la combinación.
7. Confirma que recibes un mensaje de Copilot similar a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varios aspectos del desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso del pull request. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionaste el cambio completo de la funcionalidad y las pruebas de verificación;
- creaste el PR de filtrado;
- habilitaste Agent Merge después de la revisión y confirmaste que el PR se combinó.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a cloud agent.

## Recursos

- [Gestiona pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
