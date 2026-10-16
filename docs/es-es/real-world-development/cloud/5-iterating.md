---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidad. Le has pedido que documente el código, cree una funcionalidad de juegos relacionados para que el equipo de diseño pueda seguir iterando y añada funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisemos los cambios de código y, si es necesario, demos indicaciones a Copilot para mejorar su trabajo.

### Escenario

Como se ha destacado en varias ocasiones, los fundamentos del diseño de software y DevOps no cambian por incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso DevOps habitual. Con esto en mente, revisemos las sugerencias de GitHub Copilot para la documentación, la funcionalidad de juegos relacionados y las mejoras de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para su código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos por explorar la primera solicitud de incorporación de cambios (PR) que ha generado el agente de Copilot en la nube: añadir documentación al código. Para ello utilizarás la interfaz habitual de las solicitudes de incorporación de cambios de GitHub.com.

> [!NOTE]
> Al explorar la solicitud, quizá veas un aviso que indica que un cortafuegos bloquea GitHub Copilot. **Es lo esperado**, ya que Copilot tiene un acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o desactivar el cortafuegos del agente de Copilot en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de incorporación de cambios.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**, o una similar.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud incluirá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar un momento o reflexionar sobre lo aprendido hasta ahora.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y cabeceras de comentarios. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)
9. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto se superarán para la aplicación Astro única. Puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de incorporación de cambios no es un proceso unidireccional. También puedes mencionar a Copilot en comentarios —como harías con otras personas del equipo— tanto en la solicitud como en comentarios sobre líneas concretas del código. Copilot verá estos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos proporcionar un texto concreto que debas utilizar. Algunas ideas para pedir a Copilot que actualice el código:

- Añadir cabeceras de comentarios al principio de cada archivo de código con una breve descripción de su propósito.
- Añadir comentarios TSDoc a los archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Utiliza una de las ideas anteriores u otra sugerencia sobre la documentación del código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una nueva sesión para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado los cambios, aparecerá una nueva confirmación en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés satisfecho. Cuando lo estés, puedes marcar la solicitud como lista para revisión y combinarla con la rama principal.

![Marcar la solicitud como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Volvamos a la solicitud que ha generado Copilot para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)
8. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto se superarán para la aplicación Astro única. Puede tardar unos minutos.
9. **Opcional:** También puedes cambiar a esta rama desde el Codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar con el agente en la nube para pedir actualizaciones, como modificar el número de juegos relacionados o añadir cabeceras de comentarios y comentarios TSDoc (recuerda que esta incidencia se asignó **antes** de actualizar las instrucciones personalizadas). Para solicitar cambios, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una nueva sesión.

## Revisar las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas mediante el agente personalizado de accesibilidad. Esta solicitud debería incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado en el centro de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud incluirá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para alternar entre modos;
   - el uso del almacenamiento local para guardar las preferencias;
   - los cambios de CSS o estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.);
   - cualquier código JavaScript o TypeScript que gestione el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)
9. Los flujos de trabajo deberían aparecer en cola en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto se superarán para la aplicación Astro única. Puede tardar unos minutos.
10. **Opcional:** Puedes cambiar a esta rama desde tu Codespace para probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que se mantienen después de volver a cargar la página.

Fíjate en cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo buenas prácticas de accesibilidad. Si detectas algún problema o mejora de accesibilidad, puedes mencionar a **@copilot** en un comentario para solicitar cambios, como hiciste con las solicitudes anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube es más útil cuando puedes asignarle elementos reales del backlog y dedicarte a otras tareas. Para adquirir el hábito, registra algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario de interés para los patrocinadores en la página de detalles del juego.
- Implementar la paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas incluidos en el repositorio** para observar más adelante su efecto en los resultados del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y has observado cómo prepara su entorno, planifica y ejecuta el trabajo de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para la accesibilidad y has añadido modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las solicitudes del agente en la nube e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal mediante los modos Plan y Autopilot, aptitudes de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asignar al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crear un agente personalizado para otro ámbito.
- Configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas del flujo de configuración del agente de Copilot en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
