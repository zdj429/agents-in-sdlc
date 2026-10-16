---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa los filtros, las instrucciones, la actualización de la aptitud, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación de los filtros, las actualizaciones de instrucciones, la actualización de la aptitud, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Combinaste manualmente la solicitud de incorporación de cambios (PR) de las valoraciones con estrellas; esta vez, dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la aptitud y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sola conversación y rama, y enviarás todo ese trabajo en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionarás los cambios completos de la funcionalidad y las pruebas de verificación;
- crearás la PR de filtrado;
- habilitarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener el control de la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los bloqueos de la PR —corrige errores en las comprobaciones de integración continua (CI), responde a los comentarios de revisión y vuelve a basar la rama cuando es necesario— y, después, habilita la combinación automática de GitHub cuando el repositorio lo permite.

Hasta ahora, has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Utilizar Agent Merge para gestionar la PR

Con todo el código generado, vamos a revisarlo, crear la PR y dejar que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la combinará.
7. Confirma que Copilot muestra un mensaje similar a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si no puede continuar, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo, desde la generación, prueba y validación del código hasta la gestión de la solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- has inspeccionado los cambios completos de la funcionalidad y las pruebas de verificación;
- has creado la PR de filtrado;
- has habilitado Agent Merge solo después de revisar el trabajo y has confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
