---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Acuerda un enfoque con Plan, crea el filtrado desde una incidencia con Autopilot y revisa el resultado en modo Interactive."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para integrarse con el flujo de trabajo de una organización y ayudarnos a crear las funcionalidades adecuadas de la forma adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, finalmente, la incorporarás al proyecto.

> [!NOTE]
> Continuarás el flujo de trabajo de la funcionalidad en la misma conversación y rama. Normalmente utilizarías ramas o PR diferentes para los distintos tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub.
- definirás los requisitos en modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador reenviado.

Durante el resto del flujo de trabajo actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás la validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay que acordar detalles como la combinación de categorías antes de empezar a programar. Utilizarás el modo Plan para tomar esas decisiones y, a continuación, autorizarás una implementación con un alcance definido mediante Autopilot.

## Contexto

Incorporar agentes de programación con inteligencia artificial (IA) al flujo de desarrollo no cambia sus fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo necesario.
2. Crear un plan de lo que hay que desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código una vez que se haya revisado y el proceso de integración continua haya terminado correctamente.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen este esquema.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina cuánta autonomía tiene el agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: tú y el agente trabajáis juntos. El agente propone cambios y espera tus instrucciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y repite el proceso hasta completar la tarea.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de valoraciones por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Utiliza las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la instrucción y volver a la pestaña **Session**.

Observa que la instrucción empieza ahora por `#7` (o un número similar). El carácter `#` permite incluir en el contexto una incidencia o solicitud de incorporación de cambios (PR) de GitHub.

## Planifica la funcionalidad de filtrado

Planificar permite definir el enfoque para implementar una funcionalidad o realizar tareas antes de asignárselas a Copilot. Siempre es recomendable dedicar tiempo a planificar cualquier trabajo complejo. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador bajo la instrucción muestra **Plan**.
2. Después de la referencia a la incidencia añadida en el paso anterior, escribe esta instrucción:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a preparar el plan. Primero explorará el proyecto y luego determinará el mejor enfoque.

3. Es posible que Copilot te pregunte cómo debe funcionar el filtrado. Responde según tus preferencias; no hay una única respuesta correcta.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte que no refleje tus decisiones.

## Aprueba Autopilot

Una vez escrito y revisado el plan, puedes implementarlo. Deja que Copilot trabaje en la tarea con Autopilot.

Autopilot permite a Copilot iterar sobre el problema hasta considerar que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o una opción con un nombre parecido en la versión instalada.
2. Confirma que el indicador bajo la instrucción muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar de inmediato; revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por terminadas.

## Revisa y verifica la implementación

Una vez generado, el código debe revisarse antes de combinarlo, como cualquier otro código. Revisa el código y ejecuta el sitio para comprobar que todo tiene buen aspecto.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para pasar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y con las decisiones que tomaste al planificar.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿funciona? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editorial y las combinaciones acordadas en el plan.
4. Confirma que las funciones de restablecimiento y los resultados vacíos se comportan según la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y pasos siguientes

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub.
- has definido los requisitos en modo Plan.
- has implementado la funcionalidad con Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y cómo garantizar que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
