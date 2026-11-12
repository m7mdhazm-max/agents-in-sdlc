---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo agente de GitHub Copilot, este trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad,
- revisarás y aceptarás las ediciones de archivos propuestas,
- pedirás al agente durante la sesión que añada un modo claro junto al modo de contraste alto.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de contraste alto. Mientras trabaja, el equipo de diseño solicita además un modo claro para las personas a quienes les resulta demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea, con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta. Aparecerán lecturas y ediciones de archivos, además de los comandos de terminal que el agente quiera ejecutar.
3. Cuando el agente proponga una edición, las diferencias aparecerán en línea:
   - selecciona **Keep** para aceptar el cambio,
   - selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de contraste alto. Deberías ver un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Ahora que ya está disponible el modo de contraste alto, ampliarás la misma conversación para añadir un modo claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el método de almacenamiento local.

1. En el campo de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo, revisará los cambios anteriores y propondrá las modificaciones necesarias para el modo claro.
3. Revisa cada edición nueva a medida que se aplica, igual que con el modo de contraste alto. Utiliza **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar el área de los controles de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto al campo de chat y enviar un mensaje aclaratorio.

> [!TIP]
> Orientar una sesión en curso es más rápido que volver a empezar. Utiliza prompts de seguimiento para pedir ajustes («simplify the CSS — use CSS variables instead of duplicating selectors»), corregir el enfoque («don't add a third toggle, integrate light mode into the existing settings panel») o ampliar el alcance («also add keyboard shortcuts for both toggles»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todos los cambios propuestos por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso. Has revisado cada edición al aplicarse, aceptado o rechazado los cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección has aprendido a:

- supervisar el trabajo de un agente en tiempo real,
- aceptar y rechazar ediciones individuales,
- orientar una sesión en curso mediante prompts de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha generado el agente personalizado durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
