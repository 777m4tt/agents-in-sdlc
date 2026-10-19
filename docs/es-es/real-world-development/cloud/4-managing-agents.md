---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste al agente de GitHub Copilot en la nube que se ocupara de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agente que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaban actualizando los estilos del sitio y añadiendo la función para cambiar de modo, parecía lógico incluir esta nueva función. Orientarás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de GitHub Copilot en la nube

Consulta el estado actual de todas las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código base.
    - generar API para modificar juegos.
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
    - obtener el código del repositorio.
    - crear el entorno de trabajo de Copilot.
    - configurar servidores MCP.
    - realizar diversas operaciones para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) que aparece a la derecha. Desde allí puedes consultar la PR y los archivos modificados para supervisar el trabajo.

## Orienta al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada un modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión que corresponde al modo de alto contraste. El título exacto dependerá del nombre que Copilot haya elegido y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade esta instrucción:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes con el cuadro de diálogo de orientación de la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la instrucción.
5. Observa cómo Copilot la reconoce y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot se pondrá a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Como antes, tardará varios minutos. Puedes aprovechar el tiempo para hacer una pausa y reflexionar sobre lo que has aprendido y explorado.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir tareas en curso para ajustar el alcance o aportar instrucciones adicionales.

Has explorado estos conceptos:

- explorar el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

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
