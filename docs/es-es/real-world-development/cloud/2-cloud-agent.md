---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muy pocas organizaciones, si es que hay alguna, no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que un desarrollador la revise. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

Con el agente de GitHub Copilot en la nube explorarás cómo:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver parte de su deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente; de hecho, comprobarás que no hay documentación. Como primer paso, quieren añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere que la página de detalles de cada juego muestre otros juegos relacionados, es decir, títulos de la misma categoría, para que los usuarios puedan seguir explorando. Todavía no necesita una implementación pulida; quiere algo con lo que probar la experiencia de usuario. Por ahora es un bloqueo, pero hay otras incidencias de mayor prioridad.

Ambas son tareas que pueden quedar relegadas y que resulta adecuado asignar al agente de GitHub Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, más tarde, revisar que el resultado sea correcto.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma similar a un desarrollador. Al igual que con un compañero de equipo, puedes trabajar con él de varias maneras, entre ellas, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea una PR en borrador para seguir el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando haya terminado. Cuando la solución propuesta esté lista para revisarse, te mencionará en la PR.

## La importancia de instrucciones con un alcance claro

GitHub Copilot no es mágico: no elimina la necesidad de pensar. Incluso las tareas que parecen sencillas se complican cuando se analizan en detalle. Por eso, [define con cuidado el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has descubierto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente de GitHub Copilot en la nube, consulta los archivos de instrucciones incluidos en este repositorio sin modificarlos para que luego puedas observar su efecto.

Abre estos archivos en la interfaz web de GitHub de tu repositorio o en un codespace que ya tengas en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la siguiente sección, observa si la PR resultante incluye comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Estas instrucciones son el origen de esos cambios. Volveremos a señalarlos al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube

La creación de código suele requerir un entorno específico y la ejecución de scripts de configuración, independientemente de quién realice el trabajo. Lo mismo se aplica a las tareas asignadas a Copilot, que trabaja de forma parecida a un ingeniero de software (SWE).

El agente en la nube utiliza [GitHub Actions][github-actions] para configurar su entorno. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de empezar el trabajo. Así, el agente dispone de las herramientas y dependencias necesarias. Este flujo ya está configurado para el taller y proporciona Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

Se parece a cualquier otro archivo de flujo de trabajo de GitHub, pero tiene algunos detalles importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el activador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que se ejecuta correctamente sin tener que esperar a que Copilot lo inicie.

## Añade documentación

Aunque todo el mundo entiende la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores al equipo. Copilot es especialmente útil para crear documentación, así que esta incidencia es ideal para asignársela al agente de GitHub Copilot en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás para revisar su trabajo.

1. En una pestaña nueva del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. En el campo **Title**, escribe `Code lacks documentation`.
6. En el campo **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios (PR), cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece en la lista, espera un par de minutos y actualiza el navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno mediante el flujo de trabajo descrito antes, analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a realizar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios: debería aparecer una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Recorre la sesión en directo y observa cómo resuelve el problema: explora el código y su estado, se detiene a pensar y decidir el plan adecuado y crea código.

El proceso probablemente tardará varios minutos. Uno de los principales objetivos del agente de GitHub Copilot en la nube es realizar tareas de forma asíncrona y dejarte tiempo para otras actividades. Aprovecharemos esa función asignándole otra tarea y, mientras trabaja, nos ocuparemos de añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las grandes ventajas del agente de GitHub Copilot en la nube es que permite dividir el trabajo: tú puedes ocuparte de un conjunto de tareas mientras el agente trabaja en otro. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, podrías dedicar ese tiempo a otras tareas. Asígnasela al agente de GitHub Copilot en la nube.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para utilizar la plantilla en blanco.
5. En el campo **Title**, escribe `Show related games on the game details page`.
6. En el campo **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, deberías ver varios emoji 👀 en el primer comentario de la incidencia, lo que indica que Copilot está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente de GitHub Copilot en la nube funciona de forma parecida a un ingeniero de software (SWE): no es necesario supervisarlo continuamente, sino revisar su trabajo cuando haya terminado. Ahora nos centraremos en crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede resolver deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realizan de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, puedes crear y utilizar agentes personalizados. El [agente de GitHub Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y tiene acceso a instrucciones personalizadas, que ya hemos explorado en módulos anteriores.

## Recursos

- [Acerca del agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
