---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque está diseñado para trabajar de forma autónoma y asíncrona, también es importante supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. Puedes explorar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- guiarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo la funcionalidad de alternancia, parecía lógico incluirlo. Guiarás el trabajo del agente para asegurarte de que añada tanto el modo claro como el de alto contraste.

## Revisar las tareas del agente de Copilot en la nube

Veamos el estado de las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Fíjate en la lista de tareas del panel principal y del panel izquierdo. Deberían aparecer las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código;
    - generar API para modificar juegos;
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones realizadas por Copilot. Pueden incluir:
    - clonar el código del repositorio;
    - preparar el entorno de trabajo de Copilot;
    - configurar servidores MCP;
    - llevar a cabo varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos de la lista dependerán del estado del trabajo de Copilot y del enfoque que haya elegido.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Permite ver la solicitud y los archivos modificados para supervisar el trabajo.

## Guiar al agente en la nube

Ahora que has visto las tareas activas, pidamos a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente a la incorporación del modo de alto contraste. El título concreto dependerá del nombre que Copilot haya utilizado y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empieces a ver mensajes similares a los siguientes.
3. En el cuadro **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. También debe haber un selector para este modo que permita a los usuarios elegir el modo de visualización que prefieran.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro para guiar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma el prompt y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot se ocupará de la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Al igual que antes, tardará varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y guiar las tareas del agente de Copilot en la nube. Desde allí puedes hacer seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o proporcionar indicaciones.

Has explorado estos conceptos:

- utilizar el centro de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, pasemos a [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
