---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo habitual de un desarrollador consiste en localizar todos los archivos necesarios, hacer cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto y volver a modificarlo. Y así sucesivamente.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Descubre cómo utilizar el modo agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias diferentes. El modo agente trabaja en tu IDE, lo que permite ciclos de comentarios rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot explora el proyecto, identifica los archivos pertinentes y realiza cambios coordinados.
- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generadas por el modo agente de Copilot antes de combinarlos en el código base.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un ayudante de editoriales; ahora completarás con el modo agente de Copilot el resto del trabajo de la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo de software. Permite mostrar juegos y sus detalles. Se trata de una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local en su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los ayudantes reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que utilizan las API estándar del DOM.

### Inicia el sitio web

Para facilitar la ejecución del sitio, se ha preparado la tarea `npm run dev`, que inicia el único servidor de desarrollo Astro. Puedes ejecutarla en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal nuevo en Codespaces pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella para trabajar en el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje similar a este con la URL:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te ofrece abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace de localhost en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Una vez iniciado el sitio, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre su página de detalles con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección de modo y modelo varía según el ejercicio; cada paso lo especificará cuando sea pertinente.

La primera versión del sitio funciona, pero queremos añadir nuevas capacidades. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias; pide a GitHub Copilot que te lo muestre.

1. En la lista desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla que muestra el selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan preferencia por un modelo concreto. Durante su desarrollo utilizamos Claude Sonnet 4.5 y por eso lo indicamos en estas instrucciones. Esperamos que así las sugerencias sean relativamente coherentes y la experiencia resulte satisfactoria. Sin embargo, como los modelos de lenguaje grandes (LLM, por sus siglas en inglés) son probabilísticos, las sugerencias pueden diferir de las que aparecen aquí. Es normal y previsible.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía a Copilot esta instrucción para consultar el backlog de incidencias:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según las que considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que describe cómo añadir funcionalidades al sitio.

1. En tu codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidades:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones específicas para varios tipos de archivo, entre ellos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar instrucciones, dividir conjuntos complejos en fragmentos más manejables e incluir ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado hay que hacer al menos tres cambios distintos en la aplicación:

- añadir o perfeccionar el resto de la lógica de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de combinar los cambios en el código base. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa la conversación actual de Copilot o inicia una nueva seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla que muestra cómo seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en la lista desplegable de agentes de la vista Chat.

   ![Captura de pantalla que muestra el selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad, incluidos los ayudantes de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, aparecerá uno de ellos para cada cambio; no hace falta que los selecciones mientras trabaja.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo de Copilot Chat que pregunta si quieres ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas o que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre la generación de código y las pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en marcha y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has utilizado el modo agente de GitHub Copilot para añadir capacidades al sitio web de Tailspin Toys. Has aprendido cómo:

- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos.
- el modo agente de Copilot explora el proyecto, identifica los archivos pertinentes y realiza cambios coordinados.
- revisar los cambios y las pruebas generadas por el modo agente de Copilot antes de combinarlos en el código base.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de ampliación: implementa la paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Utiliza lo aprendido en este ejercicio para pedir a Copilot que añada esta función. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- decide cómo debe funcionar la paginación: por ejemplo, si el usuario puede seleccionar el tamaño de página o si debe estar predefinido.
- al redactar la instrucción, proporciona a Copilot las directrices necesarias para crear la implementación que quieres.
- quizá tengas que iterar con GitHub Copilot, solicitar cambios y aportar contexto. Es el flujo normal de trabajo con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
