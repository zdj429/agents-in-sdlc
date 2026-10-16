---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear los filtros con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, confirma que funcionan en el navegador. En vez de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** y dejarás que Copilot controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con herramientas;
- añadirás el servidor Playwright MCP al espacio de trabajo;
- pedirás a Copilot que lo utilice para probar manualmente los filtros en un navegador;
- publicarás la rama y abrirás una solicitud de incorporación de cambios para los filtros.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. MCP permite a Copilot hacerlo: es un estándar que el agente utiliza para comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las herramientas proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Estas herramientas y recursos se proporcionan a través de un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. El servidor MCP gestiona la comunicación con herramientas externas, como API existentes o herramientas locales —por ejemplo, paquetes de NPM—. Cada servidor MCP representa un conjunto de herramientas y recursos al que puede acceder el agente.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a varias API para administrar repositorios de GitHub. El agente de IA puede, por ejemplo, crear y actualizar repositorios, y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador mediante Playwright. El agente puede navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> En cuanto a la seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Utiliza solo servidores de confianza y ten cuidado al conceder acceso a operaciones o recursos confidenciales.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Debería aparecer un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Esta entrada `github` viene incluida en la plantilla del proyecto; por eso Copilot pudo leer las incidencias del backlog en el ejercicio anterior. Utiliza el [servidor remoto GitHub MCP][remote-github-mcp-server], así que no hay que instalar nada localmente. VS Code se conecta mediante HTTP y la primera vez que Copilot utiliza una herramienta te autenticas con GitHub.

## Añadir el servidor Playwright MCP

Ahora añadirás un segundo servidor. El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`, para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en el equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que inicie el navegador sin ventana visible, algo necesario en un Codespace, que no dispone de un escritorio para mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya estará instalado. Si Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar y autorizar el servidor

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot requiere las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá la opción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pide confirmar que confías en el servidor, revisa la configuración y autorízalo.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code iniciará el servidor (y te pedirá que confíes en él) la primera vez que Copilot necesite sus herramientas.

## Probar la funcionalidad de filtrado

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador real. En vez de que tú recorras el sitio para comprobar el trabajo, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados para después resumir lo que ha visto. Así puedes confirmar rápidamente que la funcionalidad se comporta como esperas sin salir de la conversación.

Internamente, el servidor Playwright MCP utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. El agente trabaja con elementos estructurados y etiquetados (botones, enlaces y elementos de listas), como lo hacen las tecnologías de asistencia. Así, una comprobación funcional rápida también sirve como verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe los filtros:

```text
Utiliza el servidor Playwright MCP, abre un navegador en la aplicación que está en ejecución en http://localhost:4321 y verifica la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos juegos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra juegos de esa categoría.
3. Quítalo, aplica un filtro de editorial y confirma que la lista se actualiza para mostrar esa editorial.
4. Combina un filtro de categoría y otro de editorial, y confirma que los resultados cumplen ambos.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot iniciará un navegador a través del servidor Playwright MCP, seguirá los pasos y te comunicará los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas o pide a Copilot que corrija el código antes de abrir una PR.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para realizar esta prueba. Si has detenido el servidor de desarrollo, reinícialo antes de enviar el prompt. La primera vez que Copilot utilice el servidor Playwright MCP quizá tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Una vez confirmado que la funcionalidad funciona, abre una solicitud de incorporación de cambios (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios realizados por Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, por ejemplo, desde github.com o mediante la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, deja que la cree. Puede localizar la incidencia correspondiente y vincularla con la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** esté seleccionado en la lista de agentes para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con los filtros por categoría y editorial. Crea una nueva solicitud de incorporación de cambios para la rama actual y vincúlala con la incidencia correcta.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir a Copilot recopilar información y realizar las operaciones. La primera vez que utilice una herramienta de GitHub, quizá tengas que autenticarte; sigue las indicaciones.
5. Observa cómo Copilot busca las incidencias, encuentra la pertinente y crea la PR.
6. Selecciona el enlace que genere Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con herramientas;
- has añadido el servidor Playwright MCP al espacio de trabajo;
- lo has utilizado para probar manualmente la funcionalidad en un navegador antes de publicarla;
- has publicado la rama y abierto una PR para los filtros.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de él][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
