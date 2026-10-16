---
title: "Ejercicio 2 - Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso una actualización sencilla de una aplicación suele requerir cambios en varios archivos y tareas, como ejecutar pruebas. El flujo de trabajo habitual incluye localizar todos los archivos necesarios, hacer cambios, ejecutar pruebas, depurar y averiguar qué archivo falta actualizar. La lista continúa.

El modo de agente de Copilot ayuda a resolver este problema.

El modo de agente trabaja con mayor autonomía en el IDE. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentre. Veamos cómo utilizarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres son parecidos, el modo de agente y el agente en la nube están diseñados para experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción. El agente en la nube trabaja de forma asíncrona, como una persona más del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo de agente de Copilot para explorar el proyecto, identificar archivos pertinentes y realizar cambios coordinados;
- implementar funcionalidades que afecten a la interfaz y a la capa de datos;
- revisar los cambios y las pruebas que genere Copilot antes de combinarlos en el código.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrar por categoría y editorial. En el ejercicio anterior ya añadiste una función auxiliar para consultar las editoriales; ahora completarás el trabajo pendiente de la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web para entender su funcionalidad actual.

Tailspin Toys es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una aplicación Astro única que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM desde los metadatos de la página; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los asistentes reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que utilizan las API DOM estándar.

### Iniciar el sitio web

El comando `npm run dev` inicia el único servidor de desarrollo de Astro. Ejecútalo en GitHub Codespaces:

1. Vuelve al Codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te ofrece abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) mientras seleccionas la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un Codespace, al seleccionar un enlace localhost en el terminal, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, podrás explorar sus funcionalidades principales:

- **Página principal**: muestra una lista de juegos de mesa, con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían en cada ejercicio; se indicarán cuando sea necesario.

La implementación inicial funciona, pero queremos ampliarla. Empecemos revisando el backlog. Al crear el proyecto desde la plantilla, se generó automáticamente un conjunto de incidencias; pide a GitHub Copilot que las muestre.

1. Selecciona **Agent** en la lista desplegable de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal e invoca herramientas de forma autónoma.

   ![Captura de pantalla del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos.

> [!CAUTION]
> Las personas autoras de este taller no recomiendan un modelo concreto frente a otro. Al crear el taller utilizaron Claude Sonnet 4.5 y lo incluyen en las instrucciones para que las sugerencias sean relativamente coherentes. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias pueden diferir de los ejemplos. Es algo normal y esperado.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje grandes, Copilot podría utilizar otro comando MCP, pero debería poder completar la tarea igualmente.

3. Pide a Copilot que muestre el backlog con este prompt:

   ```plaintext
   Muéstrame el backlog de elementos de mi repositorio de GitHub. Ayúdame a priorizarlos según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Fíjate en que Copilot también ha priorizado las tareas según las que considera más útiles para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporciona contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el Codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidad:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a instrucciones específicas para distintos tipos de archivo, incluidos Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar información, dividir conjuntos grandes de directrices en archivos más manejables y proporcionar ejemplos y plantillas.

## Implementar los filtros

Para completar la funcionalidad hacen falta, como mínimo, tres cambios independientes en la aplicación:

- añadir o ajustar la lógica de filtrado restante en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo de agente de Copilot puede realizar estas tareas.

1. Continúa en la conversación actual de Copilot o selecciona **New Chat** para empezar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en la lista de agentes.

   ![Captura de pantalla del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia correspondiente del backlog:

   ```plaintext
   Actualiza el sitio para incluir filtros por editorial y categoría conforme a los requisitos de la incidencia correspondiente de GitHub en el backlog. Ya existe una función auxiliar para las editoriales del ejercicio anterior; consérvala y mejórala si es necesario al completar la capa de datos, la interfaz y las pruebas pendientes. Asegúrate de que se superen todas las pruebas. El servidor ya está en ejecución, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad, incluidos los asistentes de la capa de datos, la interfaz y las pruebas. A continuación, modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar unos minutos. Durante el proceso, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, podrás conservar o deshacer todos los cambios; no hace falta que los selecciones mientras el agente trabaja.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel Copilot Chat que solicita permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alternará entre la generación de código y la ejecución de pruebas hasta completar la tarea sin detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Chat en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot y cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has utilizado el modo de agente de GitHub Copilot para añadir capacidades al sitio web de Tailspin Toys. Has aprendido que:

- el modo de agente puede implementar funcionalidades en la interfaz y en la capa de datos;
- puede explorar el proyecto, identificar archivos pertinentes y realizar cambios coordinados;
- es necesario revisar los cambios y las pruebas antes de combinar el código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional opcional: implementar paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio y pide a Copilot que actualice el sitio:

- sigue las buenas prácticas existentes y utiliza los archivos de instrucciones;
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario puede elegir el tamaño de página o si será fijo;
- al crear el prompt, proporciona a Copilot las directrices necesarias para que la implementación se ajuste a lo que quieres;
- quizá tengas que iterar con GitHub Copilot, solicitando cambios y proporcionando contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
