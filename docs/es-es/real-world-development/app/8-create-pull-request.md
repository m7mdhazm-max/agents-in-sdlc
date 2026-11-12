---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil QA y las pruebas; después, autoriza Agent Merge solo tras la revisión y confirma que se ha combinado la PR."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Combinaste personalmente la PR de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería dividir la funcionalidad, las instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación,
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación,
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión lee la solicitud y resuelve lo que la bloquea: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la rama cuando es necesario. Después, la combina en cuanto GitHub lo permite. El proceso se ejecuta en segundo plano, continúa aunque reinicies la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar todavía requiere tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Deja que Agent Merge gestione la PR

Ya has creado y revisado todo el código. Ahora permitirás que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge. Hará lo siguiente:

- crear la PR con un título y una descripción,
- si has iniciado la sesión desde una incidencia, mencionarla en el cuerpo de la descripción,
- reorganizar la rama o gestionar posibles conflictos con la rama de destino,
- supervisar la integración continua para comprobar que se superan todas las comprobaciones,
- supervisar los comentarios de otros desarrolladores y de la revisión de código de Copilot y hacer cambios para resolverlos,
- combinar la PR automáticamente cuando todo se haya completado correctamente, si así lo autorizas.

Dejaremos que Agent Merge combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el proceso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR es **Merged**; activar el permiso de combinación o dejar que el agente trabaje no significa que la PR ya se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora la gestión de solicitudes de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de combinación,
- inspeccionaste la PR completa de la funcionalidad y las pruebas de verificación,
- autorizaste Agent Merge solo después de la revisión y confirmaste que la PR se había combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
