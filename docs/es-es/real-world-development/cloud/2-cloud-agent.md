---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que haya pocas organizaciones que no tengan problemas de deuda técnica. Puede tratarse de incidencias de seguridad sin resolver, código heredado que necesita actualizaciones o solicitudes de funcionalidad que llevan tiempo en el backlog porque no ha habido ocasión de implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidad de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios para que lo revise una persona. Así puedes delegar tareas tediosas y acelerar el desarrollo, dejando tiempo para que los desarrolladores se centren en cuestiones de mayor alcance.

En este ejercicio explorarás:

- cómo personalizar el entorno de generación de código;
- cómo garantizar que las operaciones se realizan de forma segura;
- por qué es importante que las incidencias tengan un alcance claro;
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado; de hecho, comprobarás que no hay ninguna. El primer paso consiste en añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. En la página de detalles de cada juego quiere mostrar otros títulos relacionados de la misma categoría, para que las personas puedan seguir explorando. Todavía no necesita una implementación pulida; quiere algo que sirva para probar la experiencia de usuario y comprobar su aceptación. Esto bloquea el trabajo, aunque actualmente hay otras incidencias con mayor prioridad.

Estas tareas suelen aplazarse y son buenas candidatas para asignarlas al agente de Copilot en la nube. Mientras el agente trabaja de forma asíncrona, el desarrollador puede ocuparse de otros asuntos y volver más tarde para revisar el resultado.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma parecida a un desarrollador. Al igual que cuando se trabaja con una persona, hay distintas formas de asignarle trabajo, por ejemplo, [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea un borrador de solicitud de incorporación de cambios para hacer seguimiento del progreso, prepara un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue en curso o después de que haya terminado. Cuando la solución esté lista para revisarla, Copilot te mencionará en la solicitud.

## La importancia de definir bien las instrucciones

GitHub Copilot no hace magia: no permite saltarse el razonamiento. Incluso las operaciones que parecen sencillas pueden ser complejas cuando se examinan en detalle. Por eso, [define cuidadosamente el alcance de las tareas para el agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como un programador en pareja con IA: divide el trabajo en fases, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has aprendido cómo orientan a Copilot las instrucciones personalizadas. Si ya has completado un ejercicio práctico sobre ellas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa en modo de solo lectura los archivos de instrucciones del repositorio para poder observar su efecto más adelante.

Abre los archivos siguientes en la interfaz web de GitHub o en un Codespace en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el campo `applyTo` de los metadatos, que aplica estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la siguiente sección, fíjate en si la solicitud resultante incluye comentarios TSDoc, convenciones de TypeScript y cabeceras de comentarios. Estos elementos proceden de los archivos de instrucciones; volveremos a ellos al revisar las PR en otro ejercicio.

## Configurar el entorno de desarrollo del agente en la nube

Crear código suele requerir un entorno concreto y la ejecución de scripts de configuración para que todo esté preparado. Lo mismo ocurre cuando asignas tareas a Copilot, que las lleva a cabo de manera parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de empezar. Así, el agente puede acceder a las herramientas de desarrollo y dependencias necesarias. La plantilla del laboratorio ya incluye esta configuración: proporciona a Copilot Node.js, las dependencias del proyecto, los binarios de navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y preparada para la aplicación Astro única:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña «Actions» del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en los pasos de configuración, Copilot lo hará automáticamente al completarlos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y de la aplicación: Node.js (ahora toda la aplicación utiliza Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que la compilación y las pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero hay dos detalles importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Este trabajo se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todos sabemos que la documentación es importante, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica suele quedar sin resolver, reduce la productividad y dificulta mantener la base de código o incorporar a nuevos desarrolladores. Copilot es especialmente útil para generar documentación, así que esta incidencia es ideal para asignarla al agente en la nube. Trabajará en segundo plano y podrás revisar el resultado en un ejercicio posterior.

1. Abre el repositorio en github.com en una pestaña nueva.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Nuestra organización exige que las funciones y los métodos incluyan comentarios TSDoc cuando resulten útiles. Lamentablemente, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para incluir comentarios de documentación cuando aclaren el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la solicitud. Copilot está preparando el entorno, ejecutando el flujo de trabajo descrito antes, analizando el proyecto y definiendo cómo resolver la tarea.

13. Revisa la lista de tareas que va a completar.
14. Desplázate por la cronología de la solicitud y busca una actualización que indique que Copilot ha empezado a trabajar.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Quizá tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve la tarea: explora el código y su estado, se detiene a analizar y decidir el enfoque y, finalmente, crea código.

Es probable que tarde varios minutos. Una de las principales ventajas del agente de Copilot en la nube es que puede trabajar de forma asíncrona, mientras tú te ocupas de otras tareas. Aprovecharemos esa característica para asignarle otro trabajo y, a continuación, añadiremos funcionalidad a la aplicación.

## Añadir una sección de juegos relacionados a los detalles

Una de las principales ventajas del agente en la nube es que permite dividir el trabajo: puedes ocuparte de una tarea mientras Copilot realiza otra. Aunque añadir juegos relacionados quizá no lleve mucho tiempo, podrías aprovechar ese tiempo para trabajar en otra cosa. Asignemos la tarea al agente en la nube.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    Queremos ayudar a descubrir más juegos mostrando títulos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia y hacer pruebas de aceptación. Requisitos:

   - Añadir una función auxiliar de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego determinado, sin incluir el propio juego.
   - Mostrar una sección «Related games» en la página de detalles que utilice la función auxiliar.
   - Gestionar el caso en que un juego no tenga juegos relacionados.
   - Crear pruebas unitarias para el nuevo asistente.
   - Antes de crear la PR, confirmar que se superan todas las pruebas.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás el emoji 👀 en el primer comentario de la incidencia; indica que Copilot ya está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube funciona de forma parecida a un ingeniero de software: no hace falta supervisarlo continuamente; puedes revisar el resultado cuando termine. Ahora nos centraremos en crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignar incidencias a Copilot para que las resuelva de forma asíncrona. Puede ayudarte con la deuda técnica, nuevas funcionalidades o la migración de código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno para generar código;
- garantizar que las operaciones se realizan de forma segura;
- definir claramente el alcance de las incidencias;
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, puedes crear y utilizar agentes personalizados. El agente en la nube también puede utilizar [servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya hemos explorado en otros módulos.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas del flujo de configuración del agente de Copilot en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
