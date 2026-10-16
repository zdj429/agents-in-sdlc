---
title: "Lección 7 - Crear y utilizar un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la aptitud quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la aptitud `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás estas capacidades en un agente personalizado con un proceso de QA claramente definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, aptitudes y herramientas MCP;
- crearás y revisarás un perfil de control de calidad reutilizable;
- seleccionarás el agente de QA y revisarás sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de las pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot, definida en un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller, definirás un rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones utilizadas hasta ahora tienen distintas funciones. Las instrucciones del repositorio describen los estándares del equipo. La aptitud `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo combinar estas capacidades para evaluar los requisitos e informar de los resultados. No sustituye a las demás personalizaciones ni requiere iniciar otra conversación.

## Crear el perfil de QA

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve al Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades según sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la aptitud quality-checks, utilizar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe indicar para cada requisito si se supera, falla o está bloqueado, y aportar pruebas que respalden el resultado. Debe pedir permiso antes de cambiar el código de implementación y no debe confirmar cambios ni abrir solicitudes de incorporación de cambios.

   Por ahora, crea solo el perfil para que pueda revisarlo.
   ```

## Inspeccionar el perfil

Antes de utilizar el nuevo agente, revisa su perfil para confirmar que Copilot ha incluido el flujo de QA previsto y sus límites. Así evitarás que un agente incompleto o demasiado amplio cambie la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee los metadatos. `description` es obligatorio; `name` es opcional, pero incluirlo proporciona al agente un nombre claro en la interfaz.
3. Lee las instrucciones del perfil y confirma que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la aptitud `quality-checks` y utiliza Playwright MCP.
4. Confirma que QA aporta pruebas, pide permiso antes de cambiar el código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si al perfil generado le falta alguna responsabilidad o límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecutar QA para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, reanuda la misma conversación de filtrado y selecciona QA para que pueda utilizar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. Aun así, el comando anterior lo activará.

2. Utiliza este prompt para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones del plan. ¿Está lista para una PR?
   ```

3. El agente de QA empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un perfil especializado reutilizable al flujo de trabajo y has revisado su trabajo. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, aptitudes y herramientas MCP;
- has creado e inspeccionado un perfil de control de calidad reutilizable;
- has seleccionado el agente de QA y revisado sus conclusiones sobre la incidencia de filtrado.

Ya tienes la implementación, la actualización de la aptitud, el perfil de QA, las pruebas y el informe de verificación listos para revisar. A continuación, [los reunirás en una PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
