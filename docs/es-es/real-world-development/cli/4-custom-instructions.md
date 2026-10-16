---
title: "Lección 4 - Orientar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, asegúrate de que Copilot disponga de esa orientación. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado los filtros, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta;
- actualizarás el archivo de instrucciones para asegurar que se siguen los estándares de programación;
- observarás el impacto de los archivos de instrucciones en el código.

## Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene una serie de directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no resulten evidentes, en vez de repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con TSDoc o JSDoc, incluido el argumento `db` inyectable cuando lo haya.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información necesaria para realizar las tareas conforme a estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que comprenda mejor tu estilo de programación y tus requisitos. Esta potente característica permite orientar a Copilot para que ofrezca sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación y las bibliotecas preferidas, e incluso los comentarios que te gusta incluir. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos, como contexto específico de una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot para **cada** solicitud del repositorio. Debe contener información general del proyecto pertinente para la mayoría de solicitudes.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas de este proyecto

El proyecto inicial incluye varios archivos de instrucciones. Exploremos lo que ya está disponible antes de hacer un cambio para ver su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Verás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` al principio del archivo: define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y remitir a Copilot a ejemplos.

## Actualizar los archivos de instrucciones conforme a las directrices del equipo

Los archivos existentes son un buen punto de partida, pero todavía falta algo. Modifiquemos el archivo principal `copilot-instructions.md` para que se añadan comentarios TSDoc al código TypeScript nuevo.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar alrededor de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Usar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones actualizadas, sal de la conversación de filtrado y reanúdala para que Copilot comience con las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda automáticamente las conversaciones.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   Acabamos de actualizar las instrucciones y las directrices de código. ¿Puedes actualizar el código generado para que las cumpla?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que describen correctamente el código.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI utiliza el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta;
- has actualizado el archivo de instrucciones para asegurar que se siguen los estándares de programación;
- has observado el impacto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la aptitud reutilizable `quality-checks`][next-lesson] para garantizar que las pruebas y lint se ejecutan de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
