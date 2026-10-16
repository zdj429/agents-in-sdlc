---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta —o Copilot necesita conocer información de contexto—, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación sobre la generación de código.

En este ejercicio:

- explorarás cómo llega a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta;
- generarás la primera parte de la capa de datos para filtrar (una función auxiliar para obtener editoriales) con las instrucciones actuales;
- añadirás un nuevo estándar general del repositorio a `.github/copilot-instructions.md`;
- volverás a enviar el mismo prompt y observarás cómo se adapta el código generado al nuevo estándar;
- confirmarás la actualización de instrucciones y los cambios de filtrado en `main` para que Copilot utilice las nuevas directrices en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir algunos de los estándares que hayas establecido. Copilot no es determinista. El objetivo de este ejercicio es observar la *tendencia* de cambio tras actualizar las instrucciones, no obtener resultados idénticos carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene una serie de directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información necesaria para realizar las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que comprenda mejor tu estilo de programación y tus requisitos. Permiten orientar a Copilot para que ofrezca sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas preferidas y los comentarios que te gusta incluir. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos, como contexto específico de una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot para **cada** solicitud del repositorio. Debe contener información general del proyecto, como la pila tecnológica, una descripción de lo que se desarrolla, buenas prácticas y orientación aplicable a la mayoría de solicitudes de chat o CLI.
- Puedes crear archivos `.github/instructions/*.instructions.md` para tareas o tipos de archivo concretos. Sirven para indicar directrices sobre lenguajes (como TypeScript o Astro) o tareas, como crear un componente de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las compleciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto el archivo de instrucciones del repositorio como los archivos `*.instructions.md` que incluyen metadatos `applyTo`.
>
> Copilot también [admite archivos de instrucciones con otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Los ejemplos del proyecto ofrecen una aproximación representativa a la creación de instrucciones:

- Mantén las instrucciones de `copilot-instructions.md` centradas en información del proyecto, su estructura y los estándares generales de programación.
- Utiliza archivos `*.instructions.md` para tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) y tareas concretas.
- Utiliza lenguaje natural y formula las directrices con claridad. Incluye ejemplos del código esperado y del que se debe evitar.

No hay una única manera de crear archivos de instrucciones ni de utilizar la IA. La experimentación te permitirá averiguar qué funciona para tu entorno y tus escenarios.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. En este proyecto verás directrices para tareas como [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz lo presenta de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección correspondiente lo explicará cuando proceda.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y echa un vistazo. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` al principio del archivo: define un patrón glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con archivos de pruebas TypeScript, por ejemplo, los que cumplan `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias para este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Verás enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y remitir a Copilot a ejemplos. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero todavía no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y las cabeceras de comentarios.

## Crear una rama para los cambios

Sigamos buenas prácticas y creemos una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un nuevo terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utilizar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían en cada ejercicio; se indicarán cuando sea necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas del editor que estén abiertas de ejercicios anteriores para que Copilot solo reciba el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el asistente.
3. En la lista desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar los cambios a los archivos.

   ![Captura de pantalla del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

5. Copilot explorará el proyecto y aplicará los cambios de código, que pueden incluir el archivo del asistente y sus pruebas.
6. Fíjate en que el asistente propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto procede de las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y la cabecera de comentarios del archivo.

> [!CAUTION]
> Como Copilot no es determinista, es posible que añada comentarios de documentación incluso sin que se lo hayas pedido. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añadir un nuevo estándar al repositorio

Como se indicó antes, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Documentemos los estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares del proyecto, pero todavía no exige documentación dentro del código; por eso al asistente generado le faltaban comentarios.
3. Añade las líneas siguientes justo debajo de los estándares existentes para pedir a Copilot que incluya cabeceras de comentarios y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla sobre comentarios de documentación que acabas de añadir.

## Volver a enviar el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el búfer.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el mismo prompt que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

4. Observa que ahora el archivo propuesto empieza con un bloque de comentarios similar a este:

   ```typescript
   /**
    * Asistentes de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc similar a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales, con su id y nombre.
    *
    * @param db - Cliente de base de datos Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Acabas de orientar a Copilot para que siga un nuevo estándar y lo aplique a código real sobre el que se trabajará en el siguiente ejercicio.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones, como cualquier otro archivo, se administran mediante el control de código fuente.

1. Abre un nuevo terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. En el terminal, confirma que estén presentes la actualización de instrucciones y los cambios del asistente ejecutando:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya cambiado para crear el asistente:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra actualizaciones complementarias (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Una vez enviada la rama, crea una solicitud de incorporación de cambios y vincúlala con la incidencia sobre los estándares de documentación del backlog. Copilot puede hacerlo por ti con las herramientas de GitHub que ya están conectadas al proyecto.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub para el espacio de trabajo, así que no tienes que preparar nada. La primera vez que Copilot utilice una, VS Code puede pedirte que inicies sesión en GitHub. Sigue las indicaciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** esté seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia sobre la actualización de los archivos de instrucciones y cree una PR desde la rama actual, describiendo las actualizaciones de instrucciones y la nueva función auxiliar para las editoriales:

   ```
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una nueva PR desde la rama actual, indica que la cierra e incluye que también hemos añadido la función auxiliar para las editoriales como base para el próximo trabajo de filtrado.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si procede.
6. Cuando se cree la PR, pide a Copilot que la combine y vuelva a `main` en el repositorio local:

   ```
   Combina la PR en main. Después, vuelve a main localmente y descarga el código más reciente para estar al día.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si procede.

¡Ya has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza el contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un nuevo estándar general a `.github/copilot-instructions.md`;
- volver a enviar el mismo prompt y observar cómo se adapta el código propuesto al estándar nuevo;
- confirmar la actualización de instrucciones y el asistente en `main` para que el siguiente ejercicio pueda utilizarlos.

A continuación, pondrás esas instrucciones en práctica en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad en varios archivos.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
