---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios del agente de Copilot en la nube, prepara todo lo necesario. Crearás tu propia copia del repositorio Tailspin Toys e iniciarás un [Codespace][codespaces] que podrás utilizar para editar archivos de instrucciones y revisar el trabajo del agente.

## Configurar el repositorio del laboratorio

Para crear una copia del repositorio en la que trabajarás, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia incluirá todos los archivos necesarios para el laboratorio y la utilizarás durante los ejercicios.

1. En una nueva ventana del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu propia copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del laboratorio. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones del equipo mentor. De lo contrario, puedes crear el nuevo repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante en el laboratorio.

## Crear un Codespace

A continuación, utilizarás un Codespace para completar los ejercicios del laboratorio.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo con compatibilidad para varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Selecciona el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, a continuación, el botón **+** para crear un nuevo Codespace.

    ![Crea un nuevo Codespace](../../../_images/ex0-create-codespace.png)

La creación del Codespace tardará varios minutos, aunque seguirá siendo mucho más rápida que instalar todos los servicios manualmente. Mientras tanto, puedes explorar otras características de GitHub Copilot, que veremos a continuación.

> [!CAUTION]
> Volverás al Codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está preparado para ejecutarse en un Codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen todos los requisitos previos para que la experiencia sea fluida. Si prefieres trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando aparezca la opción; VS Code creará el mismo contenedor de desarrollo que utiliza Codespaces.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del laboratorio. También has iniciado la creación del Codespace que utilizarás mientras trabajas con el agente de Copilot en la nube.

## Siguiente paso

Añadamos instrucciones personalizadas que seguirá el agente en la nube. Continúa con el [ejercicio 1: instrucciones personalizadas][next-lesson].

## Recursos

- [Descripción general de GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
