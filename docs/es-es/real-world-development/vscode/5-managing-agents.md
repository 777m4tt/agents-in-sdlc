---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas GitHub Copilot en modo agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra continuamente cada llamada a una herramienta y cada edición de archivo. Puedes revisar las diferencias, aceptar o rechazar cambios individuales y orientar la conversación con instrucciones de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- orientarás al agente durante la sesión para que añada un control de modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad utilizado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para los usuarios a quienes el tema predeterminado les resulta demasiado oscuro. En vez de iniciar una conversación nueva, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras el agente trabaja, desplázate por el chat para observar cada llamada a una herramienta. Verás lecturas y ediciones de archivos, además de los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, la diferencia aparecerá en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante esta sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al finalizar, debería mostrar un mensaje que resuma los cambios.

## Orienta al agente para que añada un modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un control del modo claro. Al continuar en el mismo chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de entrada de Copilot Chat, envía esta instrucción de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde el punto en que estaba, explorará los cambios existentes y propondrá modificaciones para añadir el modo claro.
3. Revisa cada nuevo cambio a medida que aparezca, como hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar la interfaz de los controles de una forma que no te gusta—, detenlo con el botón **Stop** situado junto al campo de entrada y envía una instrucción de seguimiento para aclarar lo que quieres.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Utiliza instrucciones de seguimiento para pedir ajustes («simplifica el CSS; utiliza variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisa los cambios pendientes

Antes de confirmar el trabajo, revisa todos los cambios del agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa sus diferencias. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, la has aceptado o rechazado y has enviado instrucciones de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo del agente en tiempo real.
- has aceptado y rechazado ediciones de archivos individuales.
- has orientado una sesión en curso con instrucciones de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios generados por el agente personalizado local durante este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Utilizar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
