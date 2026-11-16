---
slug: es-es/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** en VS Code integra GitHub Copilot en el editor que ya usas. En Visual Studio Code y GitHub Codespaces, controlarás Copilot Chat en modo agente, te conectarás a herramientas externas mediante Model Context Protocol (MCP) y usarás agentes personalizados sin salir del entorno de desarrollo integrado (IDE), donde Copilot puede acceder a tus archivos, al terminal y a los problemas.

Empezarás añadiendo instrucciones personalizadas y comprobando cómo las sigue Copilot. Después, usarás el modo agente para crear una funcionalidad de filtrado que abarque la interfaz, la capa de datos y las pruebas. Conectarás el servidor Playwright MCP y dejarás que Copilot use un navegador para probar la funcionalidad antes de abrir un pull request. Por último, revisarás y usarás un agente personalizado para accesibilidad, y supervisarás, dirigirás e iterarás sobre los cambios de Copilot sin salir del editor.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y el Codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade y comprueba instrucciones personalizadas en VS Code |
| [2. Modo agente][ex2] | Generación de código | Crea una funcionalidad de filtrado con el modo agente |
| [3. MCP con Playwright][ex3] | Herramientas externas | Prueba la funcionalidad en un navegador con el servidor Playwright MCP |
| [4. Agentes personalizados][ex4] | Agentes especializados | Revisa y usa agentes personalizados |
| [5. Gestión de agentes][ex5] | Supervisión | Supervisa y dirige sesiones del agente |
| [6. Iteración][ex6] | Revisión | Revisa localmente el trabajo de Copilot y decide los siguientes pasos |
| [Opcional: incorpora Foundry][foundry-toolkit] | Agentes de IA | Prepara un modelo, implementa un agente y conéctalo al sitio con VS Code y Foundry Toolkit |

## Requisitos previos

Antes de participar en este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Acceso a GitHub Codespaces

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y Copilot CLI que se usan en este taller; puedes completar todos los entornos con él.

[callout-student-plan-education]: https://github.com/education/students

## Empieza

**[Empieza con el ejercicio 0: requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
