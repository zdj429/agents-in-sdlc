---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta —o Copilot necesita conocer información de contexto—, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio:

- explorarás cómo llega a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta;
- añadirás un nuevo estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no harás una comparación «antes/después»: el agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar su impacto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las solicitudes de incorporación de cambios que genere el agente.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene una serie de directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que comprenda mejor tu estilo de programación y tus requisitos. Esta potente característica permite orientar a Copilot para que ofrezca sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación y las bibliotecas preferidas, e incluso los comentarios que te gusta incluir. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos, como contexto específico de una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot para **cada** solicitud del repositorio. Debe contener información general del proyecto, pertinente para la mayoría de solicitudes de chat o CLI: la pila tecnológica, una descripción de lo que se desarrolla, buenas prácticas y orientación general.
- Puedes crear archivos `.github/instructions/*.instructions.md` para tareas o tipos de archivo concretos. Sirven para indicar directrices sobre lenguajes específicos (como TypeScript o Astro) o tareas como crear un componente de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las compleciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto el archivo de instrucciones del repositorio como los archivos `*.instructions.md` que incluyen metadatos `applyTo`.
>
> Además, Copilot [admite archivos de instrucciones con otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices del proyecto, como una descripción de lo que se desarrolla, la estructura del proyecto y los estándares globales de programación.
- Utiliza archivos `*.instructions.md` para tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o tareas concretas.
- Utiliza lenguaje natural y formula las directrices con claridad. Incluye ejemplos de cómo debe ser el código y de cómo no debe serlo.

No hay una única forma de crear archivos de instrucciones, del mismo modo que tampoco hay una única manera de utilizar la IA. La experimentación te permitirá averiguar qué funciona para tu entorno y tus escenarios.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, quizá veas archivos para numerosas tareas, como [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz lo presenta de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección correspondiente lo explicará cuando proceda.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Son aplicables a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y echa un vistazo. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` al principio del archivo: define un patrón glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, uno que cumpla `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias para este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Verás enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones extensos en archivos más pequeños y reutilizables, y remitir a Copilot a ejemplos. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y las cabeceras de comentarios de los archivos.

## Añadir un nuevo estándar al repositorio

El siguiente paso es la única edición que harás en este ejercicio: añadir una regla general para que la documentación del código se escriba mediante comentarios TSDoc y una cabecera de comentarios a nivel de archivo. El agente en la nube la tendrá en cuenta al trabajar con las incidencias que le asignes más adelante en este recorrido.

Antes de editar, crea una rama de trabajo (el agente leerá las instrucciones de la rama a la que se dirija la incidencia, pero de todos modos confirmarás los cambios):

1. En el terminal del Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se indicó antes, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Documentemos los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación del proyecto, pero aún no incluye reglas para documentar el código; por eso el asistente generado no tenía comentarios de documentación.
3. Añade las siguientes líneas Markdown justo debajo de los estándares existentes para indicar a Copilot que añada cabeceras de comentarios y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices generales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas específicas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre comentarios de documentación que acabas de añadir.

## Confirmar, enviar y combinar la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio asignarás una incidencia a Copilot, que partirá de `main`; por tanto, los cambios de instrucciones deben llegar a `main` para que el agente pueda utilizarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. Así el agente en la nube podrá leer estas instrucciones al trabajar en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir los pasos de la rama y confirmar los cambios en `main`. Utilizar una rama permite que el taller se parezca a la forma de trabajar en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza el contexto de los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto de este recorrido.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
