---
title: "Lección 10 - Resumen y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y los recursos adicionales."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo en Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado del backlog;
- has añadido valoraciones con estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado los cambios con Autopilot y revisado el código en modo Interactive;
- has orientado al agente con instrucciones personalizadas, personalizado la aptitud `quality-checks` y la has utilizado para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar los filtros en un navegador real;
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la aptitud y las comprobaciones en el navegador;
- has revisado todos los cambios de filtrado y autorizado Agent Merge para la PR de la funcionalidad;
- has explorado comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente en la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones con estrellas:** mostrar `starRating` y un estado explícito de «sin valoración» en las tarjetas de juego.
2. **Flujo de filtrado y calidad:** implementar los filtros, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la creación de su PR, has utilizado la misma conversación y rama. Para agilizar el taller, combinamos ese trabajo en una sola PR.

## Distintos tipos de verificación

Has comprobado el código de varias formas: pruebas automatizadas, inspección propia en el navegador y exploración del navegador por parte de Copilot mediante MCP. La aptitud `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha generado un informe con el nuevo formato. QA ha combinado esos resultados con una revisión de los requisitos y de la cobertura de las pruebas antes de abrir la PR.

Las pruebas añadidas deben cubrir carencias reales; es posible que no haga falta añadir ninguna prueba nueva durante una ejecución de QA. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas pertinentes después de cualquier cambio.

## Buenas prácticas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado las instrucciones, personalizado una aptitud, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades de tu equipo. Las instrucciones establecen estándares, las aptitudes describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para analizar un enfoque antes de implementarlo, **Interactive** para participar de forma continua en cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y otro más potente para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres desarrollar, *por qué* y *cómo* puede cambiar significativamente el resultado.

## Más ideas para explorar

Has completado el flujo de trabajo principal. Puedes investigar estas otras funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar una tarea;
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo aprendido con tu equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
