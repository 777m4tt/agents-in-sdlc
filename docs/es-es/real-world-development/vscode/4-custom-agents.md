---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones específicas, prácticas recomendadas, patrones de código y conocimientos del dominio para que Copilot realice determinados tipos de trabajo con más eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: un agente de accesibilidad que aplique las directrices [WCAG][wcag], un agente de seguridad que siga prácticas de programación segura o un agente de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo contiene frontmatter YAML con al menos `name` y `description`, seguido de una instrucción Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos suelen definirse mediante archivos Markdown e indican a una IA cómo realizar tareas. La forma más sencilla de distinguirlos es que el **agente personalizado** es quien trabaja y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes mientras trabajan. En este taller, el agente personalizado de accesibilidad revisa y actualiza el sitio conforme a las directrices de accesibilidad. Para ello, podría utilizar habilidades como una dedicada al flujo de trabajo de solicitudes de incorporación de cambios o a ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier elemento de IA, prueba y repite hasta encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Comentarios recientes de usuarios indican que el tema oscuro actual resulta difícil de leer debido al contraste insuficiente entre el texto y el fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que los usuarios puedan activar y desactivar.

La accesibilidad es fundamental, por lo que quieres implementar esta funcionalidad lo antes posible. Utilizarás un agente personalizado para generarla.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para comprender cómo orientará a Copilot.

Vuelve a tu codespace y abre un terminal. Crea una rama nueva a partir de `main` para trabajar en la accesibilidad; así mantendrás separada la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que destacan:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Utiliza el agente personalizado en Copilot Chat

VS Code muestra en la lista desplegable de agentes, en la parte inferior de Copilot Chat, todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar un agente personalizado para limitar una sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección de modo y modelo varía según el ejercicio; cada paso lo especificará cuando sea pertinente.

1. En la lista desplegable de agentes de la vista Chat, selecciona **Agent** si todavía no está seleccionado.

   ![Captura de pantalla que muestra el selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona la lista desplegable de agentes situada en la parte inferior de la vista Chat. Muestra el agente activo; de forma predeterminada, es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía esta instrucción al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y una diferencia que podrás revisar.
6. A medida que se apliquen los cambios, se actualizará el indicador **Files changed** con los archivos modificados por el agente.

> [!NOTE]
> El proceso probablemente tardará unos minutos. Copilot realizará cambios reales en el repositorio y editará componentes Astro, estilos y pruebas relacionadas.

En el próximo ejercicio revisarás y orientarás este trabajo mientras sigue en curso.

## Resumen y pasos siguientes

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados en tareas y ámbitos concretos. Los agentes personalizados permiten codificar los conocimientos y estándares del equipo en agentes reutilizables que orientan a Copilot para que realice determinados tipos de trabajo con más eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo utilizar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisando los cambios a medida que se aplican y añadiendo un control para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
