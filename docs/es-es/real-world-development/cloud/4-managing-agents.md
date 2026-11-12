---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas distintas para mejorar la experiencia y añadir funcionalidades. El agente trabaja de forma autónoma y asíncrona, pero sigue siendo importante supervisar las tareas.

Puedes gestionar las tareas asignadas al agente en la nube con varias herramientas, como [la página de agentes][agents-page] de GitHub.com. Desde este centro de gestión puedes ver las tareas de los agentes con solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube,
- orientarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de contraste alto, el equipo ha decidido que también convendría añadir un modo claro. Como ya se están actualizando los estilos y añadiendo controles de alternancia, parece lógico incluir esta funcionalidad. Orientarás el trabajo del agente para que añada el modo claro además del modo de contraste alto.

## Revisa las tareas del agente de Copilot en la nube

Vamos a consultar el estado de las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código,
   - generar API para modificar juegos,
   - añadir un modo de contraste alto al sitio.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot, que pueden incluir:
   - obtener el código del repositorio,
   - preparar el entorno de trabajo de Copilot,
   - configurar servidores MCP,
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque utilizado.

4. Observa también el panel de solicitudes de incorporación de cambios de la derecha. Desde allí puedes ver la PR y los archivos modificados para seguir el trabajo.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada un control para el modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes con el cuadro de diálogo para orientar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot reconoce la solicitud y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba ejecutando al recibir el mensaje.

El proceso puede tardar varios minutos. Es un buen momento para detenerte y reflexionar sobre lo que has aprendido hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro de gestión para supervisar y orientar las tareas del agente en la nube. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para ajustar el alcance o proporcionar más indicaciones.

Has aprendido a:

- utilizar mission control y la página de agentes para supervisar las tareas del agente en la nube,
- redirigir una sesión en curso para solicitar más funcionalidades.

Mientras Copilot termina de trabajar en las funcionalidades de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
