---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Cuando activas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como todo ocurre en tiempo real en el equipo, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones de archivos a medida que se producen. Puedes revisar las diferencias insertadas en el chat, aceptar o rechazar cambios individuales y guiar la conversación con prompts de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás los cambios propuestos en los archivos;
- guiarás al agente durante la sesión para añadir un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un control de modo claro para las personas a quienes les resulta demasiado oscuro el tema predeterminado. En vez de empezar una conversación desde cero, guiarás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerá la diferencia en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos con los que estés de acuerdo.

5. Espera a que el agente termine de implementar el modo de alto contraste. Verás un mensaje final con un resumen de los cambios.

## Guiar al agente para añadir el modo claro

Ahora que el modo de alto contraste está implementado, amplía la misma conversación para añadir un control de modo claro. Como continúas en la misma sesión, el agente conserva el contexto anterior: los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. En el cuadro de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    Buen trabajo. Ahora añade un control de modo claro junto al control de alto contraste. El modo claro debe seguir el mismo patrón: persistir mediante almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el alto contraste y el modo claro de forma independiente.
    ```

2. El agente continuará a partir de su trabajo anterior, revisará los cambios existentes y propondrá las modificaciones para el modo claro.
3. Revisa cada cambio nuevo a medida que aparezca, como hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** en cada diferencia.
4. Si el agente toma una dirección que no quieres (por ejemplo, si intenta reorganizar la interfaz de los controles), deténlo con el botón **Stop** junto al cuadro de chat y envía un mensaje aclaratorio.

> [!TIP]
> Guiar una sesión en curso es más rápido que volver a empezar. Utiliza mensajes de seguimiento para pedir ajustes («simplifica el CSS y utiliza variables CSS en vez de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa brevemente todos los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberían aparecer componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado en tiempo real el trabajo de un agente;
- has aceptado y rechazado ediciones individuales;
- has guiado una sesión en curso mediante prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios del agente personalizado local a lo largo de este laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
