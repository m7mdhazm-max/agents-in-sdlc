---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y localiza la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal y permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. El primer paso es instalar la herramienta; por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- autenticarás la herramienta con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación rápida.
- encontrarás la incidencia de filtrado con el servidor GitHub MCP integrado.

## Escenario

Tu equipo está empezando a utilizar agentes de IA para ocuparse de una lista de trabajo cada vez mayor. Copilot CLI incorpora esa capacidad al terminal, donde ya trabajan habitualmente muchos desarrolladores. En esta lección instalarás y autenticarás la herramienta para poder utilizarla durante el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js, utilizarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y que cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Se mostrará el número de versión.

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, se te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones para autenticarte con el código de dispositivo y autorizar Copilot CLI.
3. Copilot CLI muestra este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponde a tu repositorio de Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En Codespaces, puede que ya tengas una sesión autenticada con GitHub. Si Copilot CLI se inicia sin pedirte que te autentiques, ya está listo.

## Familiarízate con la herramienta

Los comandos escritos en el prompt habitual del shell se ejecutan directamente en tu Codespace. Cuando se inicia Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, utiliza las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos de la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz a Copilot una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de contestar.
5. Escribe `/mcp list` y confirma que está disponible el servidor GitHub MCP integrado.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de realizar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama que esté activa en el terminal; no crea un worktree automáticamente. Crearás una rama de funcionalidad antes de cada cambio.

## Utiliza el acceso directo del taller

Normalmente, Copilot CLI pide permiso antes de utilizar herramientas que no estén incluidas en los permisos establecidos. En este taller, lo reiniciarás con `--yolo`, un acceso directo aprobado por ti que elimina estas solicitudes de confirmación dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita los permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como Codespaces o una máquina virtual; nunca lo conviertas en un alias predeterminado para el trabajo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas GitHub MCP de lectura y escritura que se utilizarán en las siguientes lecciones para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu ordenador local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Reinícialo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta rápida sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar las instrucciones o añadir un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de la funcionalidad.

## Resumen y pasos siguientes

¡Enhorabuena! Has instalado y autenticado GitHub Copilot CLI correctamente. Has aprendido a:

- instalar Copilot CLI con npm.
- autenticarte con tu cuenta de GitHub.
- confiar en el repositorio del taller y probar una conversación rápida.
- encontrar la incidencia de filtrado con el servidor GitHub MCP integrado.

A continuación, [iniciarás tu primer cambio específico][next-lesson] y utilizarás Copilot CLI para mostrar las valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
