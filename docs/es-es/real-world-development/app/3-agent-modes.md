---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: acuerda un enfoque con Plan, desarrolla el filtrado a partir de una incidencia con Autopilot y revisa y comprueba el resultado en Interactive."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más completo. La aplicación GitHub Copilot está diseñada para adaptarse al flujo de trabajo de una organización y ayudarnos a desarrollar lo adecuado de la manera correcta. En esta y las siguientes lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que se comporta como esperas y, finalmente, la integrarás en el proyecto.

> [!NOTE]
> A medida que avances en el flujo de trabajo de esta funcionalidad, utilizarás la misma sesión. Normalmente se usarían sesiones o PR distintas para los diferentes tipos de archivo, pero haremos una excepción para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente a partir de una incidencia de GitHub,
- definirás los requisitos en el modo **Plan**,
- implementarás la funcionalidad en el modo **Autopilot**,
- revisarás el código,
- comprobarás manualmente la funcionalidad en un lienzo del navegador.

Más adelante actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás la validación MCP, crearás un agente QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y quienes lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay que acordar algunos detalles, como la combinación de categorías, antes de programarla. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Abrir una incidencia registrada que describa el trabajo.
2. Planificar lo que hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Comprobar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen alguna variante de este modelo.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla la autonomía del agente. Puedes elegirlo en el menú desplegable situado debajo del campo de prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que empiece a ejecutarlo.
- **Autopilot**: el agente trabaja con plena autonomía: escribe código, ejecuta pruebas e itera sin esperar a que le indiques cada paso.

Empieza en el modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que se ha combinado la PR de valoraciones por estrellas y que tu `main` local está actualizado.

1. En la barra lateral, dentro de **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior. Abre **Allow users to filter games by category and publisher**. Es una incidencia incluida en la plantilla; no tienes que crear otra.
2. Selecciona **New session** y crea un **new working tree** a partir de `main` actualizado.

    ![Vista de la incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modos.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal, pero no en la sesión, revisa la configuración de `PATH` o del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones fallidas; no reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene el contexto de la solicitud. Envía este prompt:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye filtros por categoría y editorial, controles accesibles, cambios en el acceso a datos y pruebas. Aclara cualquier comportamiento dudoso, por ejemplo, cómo se combinan varias categorías o qué ocurre cuando no hay juegos que coincidan.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobaciones de tipos mediante las herramientas existentes en el proyecto. Céntrate en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Pide que se modifique el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de dar por hecho que la aprobación cambia el modo. Confirma que el indicador muestra **Autopilot**.

Copilot empezará a implementar la funcionalidad. Seguirá el plan, generará código y ejecutará pruebas de forma iterativa.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones. Detén solo los servidores que hayas iniciado tú.

## Revisa y comprueba la implementación

Como cualquier otro código, el código generado debe revisarse antes de combinarlo. Revisarás los cambios y ejecutarás el sitio para confirmar que todo funciona.

1. Abre **Changes** e inspecciona la implementación del filtrado y sus pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios siguen las instrucciones del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias y E2E y las comprobaciones de tipos. Una comprobación omitida no cuenta como superada.
4. Corrige los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local. Asegúrate de que el servidor probado pertenece a este worktree y no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿funciona? Inicia la aplicación como antes y abre el sitio en un lienzo del navegador.

1. Utiliza este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y confirma que los resultados corresponden a ella.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo de la sesión y cierre el lienzo del navegador:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, worktree y rama para las lecciones 4 a 8. En la siguiente lección aplicarás instrucciones personalizadas al código de filtrado y, después, añadirás la habilidad y las mejoras de QA antes de crear y combinar la PR conjunta en la Lección 8. Puedes dejar la sesión inactiva después de detener su servidor de desarrollo y retomarla en la siguiente lección.

## Resumen y pasos siguientes

Has utilizado distintos modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- iniciaste una sesión de agente a partir de una incidencia de GitHub,
- definiste los requisitos en el modo **Plan**,
- implementaste la funcionalidad en el modo **Autopilot**,
- revisaste el código,
- comprobaste manualmente la funcionalidad en un lienzo del navegador.

A continuación, profundizarás en cómo generar código que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
