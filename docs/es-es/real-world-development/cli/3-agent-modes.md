---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios de mayor alcance requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con el flujo existente de una organización y ayudarnos a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: empezarás con una incidencia para generar una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, finalmente, la combinarás con el proyecto.

> [!NOTE]
> Seguirás utilizando la misma conversación y rama durante el flujo de trabajo de la funcionalidad. Normalmente utilizarías distintas ramas o PR para los diferentes tipos de archivo, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad con el modo Autopilot;
- revisarás el código;
- validarás manualmente la funcionalidad en un navegador reenviado.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la aptitud existente `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles, como la combinación de categorías. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La incorporación de agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores sigue un proceso similar a este:

1. Empezar con una incidencia registrada que describa el trabajo.
2. Crear un plan para lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, combinarlo.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen este patrón general.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el nivel de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: tú y el agente trabajáis conjuntamente. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente prepara primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta que termina la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empezar desde la incidencia

Antes de empezar a trabajar en los filtros, vuelve al Codespace y comprueba que el repositorio y el terminal estén preparados.

1. Vuelve al Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para los filtros:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa dos veces <kbd>Tab</kbd> para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora comienza con `#7` (o un número parecido). El carácter `#` permite incluir en el contexto una incidencia o solicitud de incorporación de cambios (PR) de GitHub.

## Planificar la funcionalidad de filtrado

La planificación permite definir el enfoque de una funcionalidad o tarea antes de asignársela a Copilot. Siempre es buena idea dedicar tiempo a planificar los trabajos complejos. Cambiemos al modo Plan y pidamos a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador bajo el prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe este prompt:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot creará el plan: primero explorará el proyecto y después determinará el mejor enfoque.

3. Durante el proceso, Copilot puede hacer preguntas sobre el funcionamiento de los filtros. Respóndelas según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que revise las partes que no reflejen tus decisiones.

## Aprobar Autopilot

Una vez redactado y revisado el plan, es hora de implementarlo. Dejemos que Copilot se ocupe de la tarea con el modo Autopilot.

Autopilot permite a Copilot iterar sobre el problema hasta que considera que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o una opción con un nombre parecido en la versión instalada.
2. Confirma que el indicador bajo el prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de considerar que las comprobaciones están completas.

## Revisar y verificar la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, como cualquier otro código. Revisemos los cambios y ejecutemos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación de los filtros y sus pruebas.
3. Compara el resultado con la incidencia y con las decisiones tomadas durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los errores.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio en el puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar los filtros en el navegador. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones acordadas en el plan.
4. Comprueba que el comportamiento al restablecer los filtros y cuando no hay resultados coincide con la incidencia y tus decisiones.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub;
- has definido los requisitos en el modo Plan;
- has implementado la funcionalidad con el modo Autopilot;
- has revisado el código;
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, aprenderás a generar código siguiendo prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
