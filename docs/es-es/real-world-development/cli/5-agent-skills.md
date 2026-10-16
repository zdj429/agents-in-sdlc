---
title: "Lección 5 - Personalizar y utilizar la aptitud quality-checks"
description: "Explora la aptitud quality-checks existente, personaliza el formato del informe y úsala para validar los filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar implica algo más que escribir código. Ya hemos validado manualmente que funciona y hemos utilizado archivos de instrucciones para asegurarnos de que sigue nuestros estándares. ¿Y qué ocurre con las pruebas, lint y los demás componentes de la integración continua (CI)?

Para tareas como estas, las **aptitudes de agente** son ideales. Ayudan a Copilot a entender cómo ejecutar correctamente operaciones como las comprobaciones.

En esta lección:

- explorarás la aptitud existente `quality-checks`;
- personalizarás el formato de sus resultados;
- volverás a cargar y ejecutar la aptitud.

## Escenario

Tailspin Toys utiliza la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que resulte más fácil de leer.

## Instrucciones, scripts y recursos

Las aptitudes de agente agrupan instrucciones de tareas reutilizables, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. Una aptitud es, principalmente, una carpeta con su nombre que contiene un archivo Markdown llamado `SKILL.md`. Este archivo incluye metadatos con un nombre y una descripción, una introducción a la aptitud e instrucciones sobre cuándo utilizarla. También puede contener subcarpetas con scripts y recursos que la aptitud utiliza.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La aptitud `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, ya que utiliza los comandos existentes del proyecto.

Las aptitudes pueden estar en la carpeta `.github/skills` del proyecto para que el equipo las comparta y reutilice o, de forma global, en la carpeta de aptitudes del usuario `~/.copilot/skills`.

## Explorar la aptitud

Exploremos la aptitud `quality-checks` que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve al Codespace. En el editor, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee los campos `name` y `description` del principio. La descripción ayuda a Copilot a decidir cuándo utilizar la aptitud.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y lint.
4. Fíjate en que todavía no hay una sección **Results output formatting**.

## Ejecutar la aptitud antes de cambiarla

Puedes llamar directamente a las aptitudes desde Copilot CLI o describir la tarea con lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Ejecuta la aptitud quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Lee el informe final.

## Personalizar el informe

Queremos un informe mejor, que indique qué se ha ejecutado, si ha funcionado y qué resultados han obtenido realmente las herramientas. Añadamos ese formato a la aptitud.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la aptitud actualizada

Veamos el resultado del cambio. Copilot CLI puede volver a cargar las aptitudes modificadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo prompt que antes:

   ```plaintext
   Ejecuta la aptitud quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una aptitud de agente existente. En esta lección:

- has explorado la aptitud `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos;
- has personalizado el formato de sus resultados;
- has vuelto a cargar y ejecutado la aptitud.

El cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás a Copilot interactuar directamente con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de aptitudes

Estos ejemplos de la comunidad sirven como referencias; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
