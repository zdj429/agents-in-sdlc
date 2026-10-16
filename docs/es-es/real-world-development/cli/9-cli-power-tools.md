---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier CLI completa, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Veamos otros comandos útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usarás `/model` para explorar los modelos disponibles;
- aprenderás cómo `/share` puede exportar o compartir una sesión;
- explorarás comandos opcionales para el trabajo en paralelo, los árboles de trabajo y la delegación al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de CLI. Ahora explorarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente al [agente de Copilot en la nube][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puedes acercarte al límite de la ventana de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta y permite inspeccionarla o compactarla manualmente con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es breve, quizá no haya un cambio significativo.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Utiliza `/compact` para elegir el momento. Utiliza `/clear` o `/new` cuando cambies a una tarea sin relación y quieras empezar una conversación nueva.

## Elegir un modelo

Los modelos tienen distintas capacidades y cada desarrollador puede tener preferencias distintas. Copilot CLI permite enumerar los modelos disponibles y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Colaborar y compartir conocimientos ayuda a todo el equipo a mejorar su uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir la sesión, elige el destino que mejor se ajuste a tus necesidades, como `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que tu equipo no tenga previsto compartir.

## Opcional: ampliar o delegar el trabajo

Has completado el taller principal. Copilot CLI también proporciona comandos para tareas de mayor alcance:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo;
- `/worktree` puede crear un árbol de trabajo Git aislado para una tarea independiente;
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y podría abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o tareas remotas. Antes de probarlos, inicia una tarea nueva y bien definida, y revisa el resultado mediante el flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y ver lo que ocurre internamente. En esta lección:

- has utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- has usado `/model` para explorar los modelos disponibles;
- has aprendido cómo `/share` puede exportar o compartir una sesión;
- has explorado comandos opcionales para el trabajo en paralelo, los árboles de trabajo y la delegación al agente en la nube.

Copilot CLI ofrece muchos otros comandos de barra y capacidades. Terminemos el recorrido [repasando lo que hemos aprendido][next-lesson] y algunos pasos para seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
