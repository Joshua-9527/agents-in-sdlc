---
title: "Ejercicio 1: instrucciones personalizadas para cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Siguiente lección: agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información previa, asegúrate de que tenga acceso a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda tanto qué debe hacer como cómo hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas;
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no compararás prompts antes y después. El agente en la nube de Copilot trabaja de forma asíncrona en incidencias de GitHub, así que es más difícil demostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones cuando revises los pull requests que genere el agente en los ejercicios posteriores.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc (documentación de TypeScript).
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Usarás archivos de instrucciones para dar a Copilot la información necesaria y ayudarle a respetar estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot el contexto y las preferencias necesarios para entender mejor tu estilo de programación. Puedes indicar las convenciones, bibliotecas y comentarios que prefieres, ya sea para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información general pertinente, como la pila tecnológica, una descripción del proyecto, prácticas recomendadas y directrices globales.
- `.github/instructions/*.instructions.md`: archivos para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un entorno de desarrollo integrado (IDE), los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni para sugerir las siguientes ediciones.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md.

### Procedimientos recomendados para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto: descripción, estructura y estándares globales de programación.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo —pruebas unitarias, componentes de Astro o capa de datos— o tareas.
- Usa lenguaje natural, redacta instrucciones claras e incluye ejemplos de cómo debería ser el código.

No hay una única forma correcta de crear archivos de instrucciones o de usar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para cada proyecto.

> [!TIP]
> Los proyectos con GitHub Copilot pueden beneficiarse de una colección sólida de archivos de instrucciones. En este proyecto hay instrucciones para [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions], entre otras tareas.
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno presenta esta opción de una forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente describe la opción aplicable.
>
> Para buscar plantillas o ideas, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones incluidos en el repositorio: el archivo principal `copilot-instructions.md` y los archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y examínala. Incluye instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros aspectos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` del principio define un glob relativo a la raíz del repositorio. Coincide con los archivos de pruebas TypeScript, por ejemplo, los que siguen el patrón `**/*.test.ts`.
5. Examina las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate al final. Observa los enlaces a otros archivos de instrucciones —como `unit-tests.instructions.md`— y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Añade un estándar para todo el repositorio

Añadirás una regla que exija comentarios TSDoc y un encabezado de comentarios por archivo. El agente en la nube aplicará esta orientación a las incidencias que le asignes más adelante.

1. En el terminal de tu Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Busca la sección **Code formatting requirements**, cerca de la línea 27. Observa que todavía no incluye la documentación dentro del código.
4. Debajo de los estándares existentes, añade estas líneas Markdown:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones específicas se pueden guardar en el repositorio (`.github/copilot-instructions.md`) para que sean globales, o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. Los estándares generales del proyecto, como la regla de comentarios que acabas de añadir, corresponden al archivo del repositorio.

## Confirma, sube y combina la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama de destino de la incidencia. En el ejercicio siguiente, Copilot partirá de `main`, así que tus cambios deben llegar a `main`.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Sube la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre un pull request desde `custom-instructions-cloud` a `main` y combínalo. El agente en la nube leerá estas instrucciones cuando trabaje en incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar el cambio directamente en `main`. La rama permite practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has añadido un estándar para todo el repositorio en `.github/copilot-instructions.md`. En este ejercicio:

- exploraste cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas;
- añadiste un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

Verás cómo se aplica ese estándar en los pull requests que genere el agente.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Procedimientos recomendados para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
