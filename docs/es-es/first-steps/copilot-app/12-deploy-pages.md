---
title: "Lección 12 - Implementa el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dale un hogar público a Space Quiz implementándolo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiar la implementación, pero sigues siendo responsable de revisar los cambios y confirmar que el sitio está publicado.

En esta lección:

- pedirás a Copilot que configure una implementación en GitHub Pages;
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla;
- seguirás la implementación y comprobarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero la gente todavía necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública, sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que implemente el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Habilita GitHub Pages para este repositorio. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si es necesario y lo implemente en GitHub Pages. Si todavía no existe un README, añade uno que describa la aplicación y cómo ejecutarla localmente. Crea una solicitud de incorporación de cambios con estos cambios y combínala cuando se superen las comprobaciones obligatorias. Supervisa la implementación hasta que se complete correctamente y, después, indica la URL definitiva del sitio publicado. Si no tienes permiso para habilitar Pages o combinar la solicitud, dime exactamente qué debo aprobar o hacer; no informes de que se ha completado correctamente hasta que el sitio esté publicado.
```

![Diagrama de tres fases: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de GitHub Actions llamada Deploy website, con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y combine una solicitud de incorporación de cambios y modifique la configuración de Pages del repositorio. Revisa la diferencia, los permisos de implementación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y verifica la implementación

1. Confirma que el archivo README solo se añade si el repositorio todavía no tiene uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación e implementación del flujo de trabajo.
3. Combínala solo cuando se superen las comprobaciones obligatorias de la solicitud de incorporación de cambios.
4. Sigue la ejecución de implementación de GitHub Actions hasta que se complete correctamente.
5. Abre la URL indicada y confirma que se carga Space Quiz.

Si el agente no puede habilitar Pages o combinar la solicitud de incorporación de cambios, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la implementación se complete correctamente y el sitio publicado se cargue.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio publicado y has verificado la implementación, en vez de limitarte a confiar en el informe del agente. Continúa con [Lección 13: revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
