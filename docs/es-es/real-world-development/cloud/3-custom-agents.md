---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes en archivos Markdown de la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio para que realice determinados tipos de trabajo con mayor eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que garantice el cumplimiento de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones de prueba coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con, al menos, los campos `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y aptitudes de agente

Hay cierto solapamiento entre los agentes personalizados y las [aptitudes de agente][agent-skills-concept]. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar operaciones. La forma más sencilla de distinguirlos es que el **agente personalizado** es quien trabaja y las **aptitudes** son sus herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar aptitudes e incluso otros agentes como parte de su trabajo. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las pautas de accesibilidad; para ello podría utilizar aptitudes como una para gestionar solicitudes de incorporación de cambios o para ejecutar y administrar pruebas.

> [!NOTE]
> No existe una única forma «correcta» de crear un agente personalizado. Como con cualquier herramienta de IA, prueba e itera para encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados explorarás:

- cómo se definen los agentes personalizados;
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, con independencia de sus capacidades o preferencias visuales. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para responder a esta necesidad de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta funcionalidad lo antes posible. Utilizarás un agente personalizado para generarla.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve al Codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los metadatos YAML con los campos `name` y `description`.

> [!CAUTION]
> Los metadatos con `name` y `description` son obligatorios para los agentes personalizados.

3. A continuación, lee las secciones siguientes, que destacan:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las buenas prácticas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - una lista de errores habituales.

## Crear y asignar una incidencia

El centro de control es el lugar principal para trabajar con los agentes del entorno. Desde allí puedes asignar tareas al agente de Copilot en la nube, supervisarlas y redirigir el trabajo o proporcionar indicaciones adicionales. Empecemos por asignar a Copilot una tarea para crear el modo de alto contraste.

1. Ve al repositorio.
2. Selecciona la pestaña de incidencias.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. En **Title**, escribe `Add high contrast mode to website`.
6. En **Description**, escribe:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura de pantalla de la asignación del agente en la nube, con el agente personalizado y Accessibility resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados para tareas y ámbitos concretos. Con ellos puedes convertir los conocimientos y estándares de tu equipo en agentes reutilizables que guíen a Copilot para realizar tipos de trabajo específicos con mayor eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo asignarles una tarea.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y guiar la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar el uso de agentes personalizados en tu organización][org-custom-agents]
- [Preparar el uso de agentes personalizados en tu empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
