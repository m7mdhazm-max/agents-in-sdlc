---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El proceso habitual de desarrollo consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto, volver a modificarlo… y así sucesivamente.

Aquí es donde entra en juego el modo agente de Copilot.

El modo agente de Copilot puede trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres sean parecidos, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con interacciones y ciclos de comentarios rápidos. El agente en la nube está diseñado para trabajar de forma asíncrona como un compañero del equipo e interactuar contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo el modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- cómo puede implementar nuevas funcionalidades en la interfaz de usuario y la capa de datos.
- cómo revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código del proyecto.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales; ahora completarás con el modo agente el resto del trabajo de la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio de Tailspin Toys para conocer sus funcionalidades actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con una temática de desarrollo. Permite consultar juegos y ver sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local directamente desde sus metadatos mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con scripts `<script>` de Astro pequeños y específicos que utilizan las API estándar del DOM.

### Inicia el sitio

Para facilitar la ejecución del sitio, se ha definido la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en tu GitHub Codespace así:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje con la URL parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En Codespaces, al seleccionar un vínculo a una URL de localhost desde el terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio

Cuando el sitio esté en marcha, podrás explorar sus funcionalidades principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los siguientes ejercicios, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea importante.

La implementación inicial del sitio funciona, pero queremos añadir más funcionalidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias; pide a GitHub Copilot que te las muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo concreto. Al preparar el taller utilizamos Claude Sonnet 4.5 y lo mencionamos en las instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia sea satisfactoria. Sin embargo, como los modelos de lenguaje de gran tamaño (LLM) son probabilísticos, las sugerencias pueden diferir de los ejemplos. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede utilizar un comando MCP diferente y aun así completar la tarea.

3. Pide a Copilot que muestre las incidencias del backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según los que considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En tu Codespace, ve a `.github/instructions/ui.instructions.md`.
2. Lee las directrices generales para añadir funcionalidades. Incluyen:
   - una descripción general de la arquitectura,
   - principios de diseño de componentes, capacidad de prueba y accesibilidad,
   - vínculos a instrucciones específicas para varios tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar ejemplos y plantillas y dividir conjuntos de instrucciones complejos en partes más pequeñas.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios en la aplicación:

- añadir o ajustar la lógica de filtrado restante en la capa de datos (`src/lib/`),
- añadir o actualizar las pruebas del comportamiento del filtrado,
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y superar todas las comprobaciones antes de combinar los cambios. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz.](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog con este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar los helpers de la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar varios minutos. Mientras trabaja, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, habrá una opción **Keep** o **Undo** para cada cambio; no es necesario seleccionarlas mientras siga trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo en el panel Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que Copilot se detenga varias veces e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión de Chat completa con el modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde está abierto el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys. Has visto cómo:

- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz y la capa de datos,
- Copilot puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios,
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código del proyecto.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que añada paginación al sitio. Ten en cuenta lo siguiente:

- sigue los procedimientos recomendados existentes, incluidos los archivos de instrucciones,
- decide cómo quieres implementar la paginación y si quieres permitir que el usuario seleccione el tamaño de página o fijarlo en el código,
- al redactar el prompt, proporciona a Copilot el contexto necesario para que implemente la funcionalidad como prefieras,
- puede que tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto; es el flujo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
