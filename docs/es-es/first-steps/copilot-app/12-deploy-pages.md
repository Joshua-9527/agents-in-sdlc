---
title: "Lección 12: publica el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su pull request y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz en GitHub Pages para que todo el mundo pueda probarlo desde una URL pública. Copilot puede preparar el flujo de trabajo y guiarte durante el despliegue, pero tú debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure un despliegue en GitHub Pages;
- revisarás el flujo de trabajo y el pull request antes de combinarlo;
- seguirás el despliegue y comprobarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero aún hace falta una forma sencilla de probarlo. Publicar el sitio, que consta de un solo archivo, con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni una dependencia de runtime.

## Pide a Copilot que prepare el despliegue

> [!WARNING]
> Crear un pull request puede confirmar y subir código. Revisa la configuración de aprobación y el repositorio de destino antes de enviar este prompt. Con **Approve all**, no esperes una solicitud de confirmación independiente.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revisa el flujo de trabajo y el pull request

1. Confirma que el README solo se añade si el repositorio todavía no tiene uno.
2. Revisa los cambios del pull request, incluidos los pasos de compilación y despliegue del flujo de trabajo, los desencadenadores y los permisos.
3. Confirma que el flujo de trabajo despliega desde la rama predeterminada, no desde un pull request sin combinar.
4. Solicita las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias del pull request antes de autorizar la combinación.

## Autoriza la combinación y el despliegue

> [!WARNING]
> El prompt siguiente autoriza al agente a combinar el pull request revisado, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de terminar la revisión y confirmar el repositorio y el pull request. La configuración de aprobación puede permitir estas acciones sin otra solicitud de confirmación.

Sustituye `<PR_NUMBER>` por el número del pull request que has revisado y envía este prompt por separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres fases: un pull request de space-quiz combinado; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz disponible en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Comprueba el despliegue

1. Sigue la ejecución de despliegue de GitHub Actions hasta que se complete correctamente.
2. Abre la URL indicada y confirma que Space Quiz se carga.
3. Si el agente no puede habilitar Pages o combinar el pull request, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que el despliegue se complete y el sitio esté disponible.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio activo y has comprobado el despliegue en lugar de confiar únicamente en el informe del agente. En esta lección:

- pediste a Copilot que configurara un despliegue en GitHub Pages;
- revisaste el flujo de trabajo y el pull request antes de combinarlo;
- seguiste el despliegue y comprobaste el sitio publicado.

Continúa con [la lección 13: revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
