---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar los filtros en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos destacado, programar implica más que escribir código. Hay que trabajar con datos y servicios externos, e incluso habilitar automatizaciones adicionales para Copilot. Para eso están los servidores MCP: permiten que Copilot vaya más allá de las funciones integradas en la CLI y tenga acceso a más herramientas y servicios.

En esta lección:

- entenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- añadirás el servidor Playwright MCP si todavía no está disponible;
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar los cambios en la interfaz hay que interactuar con ella. Quieres permitir que Copilot utilice el sitio web como lo haría un usuario y automatice las comprobaciones de los cambios, para tener más confianza en que funcionan como se espera.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite a los agentes de IA comunicarse con herramientas y servicios externos. Gracias a MCP, los agentes de IA pueden acceder a información actualizada y realizar acciones en tu nombre.

Estas herramientas y recursos se ofrecen a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta las implicaciones de seguridad.

## Añadir el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa en la sección siguiente.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo para añadir un servidor MCP escribiendo este comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor sea **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el nuevo servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pedir a Copilot que explore la funcionalidad con Playwright

Antes has comprobado manualmente que la funcionalidad se comporta como se espera. Ahora utilicemos el servidor Playwright MCP disponible en Copilot para hacer la misma comprobación.

1. Usa este prompt para pedir a Copilot que valide la funcionalidad con el servidor Playwright MCP:

   ```plaintext
   Inicia la aplicación y utiliza Playwright MCP para comprobar los filtros según la incidencia y nuestro plan. Dime qué funciona y qué no, sin hacer cambios. Cuando termines, detén el servidor que has iniciado.
   ```

> [!NOTE]
> No es necesario pedirle a Copilot que utilice el servidor MCP, porque normalmente lo detecta por su cuenta. Sin embargo, como sabes qué debe utilizar, indicárselo puede ofrecer resultados más coherentes y ahorrar tokens.

2. Observa los pasos que realiza Copilot en el navegador para confirmar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como se espera.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio, detendrá el servidor y te proporcionará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has utilizado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- has añadido el servidor Playwright MCP si aún no estaba disponible;
- has pedido al agente que controlara un navegador y explorara la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combine la aptitud y las herramientas del navegador en un perfil especializado.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de él][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
