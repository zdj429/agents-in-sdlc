---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes en archivos Markdown de la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio para que realice determinados tipos de trabajo con mayor eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, uno de accesibilidad que garantice el cumplimiento de [WCAG][wcag], otro de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con, al menos, los campos `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y aptitudes de agente

Hay cierto solapamiento entre los agentes personalizados y las [aptitudes de agente][agent-skills-concept]. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar operaciones. La forma más sencilla de distinguirlos es que el **agente personalizado** es quien trabaja y las **aptitudes** son sus herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar aptitudes e incluso otros agentes como parte de su trabajo. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las pautas de accesibilidad; para ello podría utilizar aptitudes como una para gestionar solicitudes de incorporación de cambios o para ejecutar y administrar pruebas.

> [!NOTE]
> No existe una única forma «correcta» de crear un agente personalizado. Como con cualquier herramienta de IA, prueba e itera para encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, con independencia de sus capacidades o preferencias visuales. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para responder a esta necesidad de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta funcionalidad lo antes posible. Utilizarás un agente personalizado para generarla.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve al Codespace, abre un terminal y crea una rama nueva a partir de `main` para el trabajo de accesibilidad. Así mantendrás separada la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los metadatos YAML con los campos `name` y `description`.

> [!CAUTION]
> Los metadatos con `name` y `description` son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que destacan:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las buenas prácticas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - una lista de errores habituales.

## Utilizar el agente personalizado en Copilot Chat

VS Code muestra en la lista de agentes de la parte inferior de Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para limitar una sesión de chat a las instrucciones y herramientas de ese agente.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían en cada ejercicio; se indicarán cuando sea necesario.

1. Si todavía no está seleccionado, elige **Agent** en la lista desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abre la lista de agentes situada en la parte inferior de la vista de chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Añade un modo de alto contraste al sitio. Debe haber un control para activar y desactivar el alto contraste, y la configuración debe mantenerse entre cargas de página mediante el almacenamiento local del navegador.
    ```

5. Copilot Chat explorará el código, propondrá cambios y los aplicará a los archivos del proyecto. Cada cambio aparecerá en el chat con la ruta del archivo y una diferencia que podrás revisar.
6. A medida que se apliquen los cambios, se actualizará el indicador **Files changed** con los archivos modificados por el agente.

> [!NOTE]
> Este proceso puede tardar varios minutos. Copilot realizará cambios reales en el repositorio: editará archivos existentes, como componentes Astro, CSS y las pruebas relacionadas.

En la siguiente lección revisarás y guiarás el trabajo en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados para tareas y ámbitos concretos. Con ellos puedes convertir los conocimientos y estándares de tu equipo en agentes reutilizables que guíen a Copilot para realizar tipos de trabajo específicos con mayor eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo utilizar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se produzcan y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
