---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar trabajo opcionalmente al agente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier interfaz de línea de comandos (CLI) de calidad, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con la herramienta. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros comandos útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- utilizarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, utilizar worktrees y delegar tareas al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal del CLI. Ahora explorarás algunas funciones adicionales: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar trabajo al [agente de GitHub Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando es necesario, y puedes inspeccionar o compactar el contexto manualmente con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se reparte el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Es posible que no haya cambiado mucho si la conversación ya era breve.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Utiliza `/compact` si quieres decidir cuándo hacerlo. Utiliza `/clear` o `/new` si cambias a una tarea sin relación y quieres empezar una conversación nueva.

## Elige un modelo

Cada modelo tiene distintas ventajas y cada desarrollador tiene sus propias preferencias. Copilot CLI permite ver los modelos disponibles y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Trabajar en equipo y compartir lo aprendido ayuda a todos a sacar más partido de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un GitHub gist.

1. Escribe `/help` y revisa las opciones de `/share` en la versión instalada.
2. Si quieres compartir la sesión, elige el destino que mejor se adapte a tus necesidades; por ejemplo, `/share file` para exportarla como archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir instrucciones, respuestas y detalles del proyecto.

Publicar un enlace o gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delega

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de GitHub Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear worktrees adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien delimitada, y revisa los resultados con tu flujo de trabajo habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y pasos siguientes

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y observar lo que ocurre internamente. En esta lección:

- has utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- has utilizado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para trabajar en paralelo, utilizar worktrees y delegar tareas al agente en la nube.

Copilot CLI ofrece más comandos de barra y funciones que puedes explorar. Para terminar este recorrido, [repasarás lo que has aprendido][next-lesson] y descubrirás los pasos siguientes.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
