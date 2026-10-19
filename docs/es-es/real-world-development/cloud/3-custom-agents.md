---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones específicas, prácticas recomendadas, patrones de código y conocimientos del dominio para que Copilot realice determinados tipos de trabajo con más eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: un agente de accesibilidad que aplique las directrices [WCAG][wcag], un agente de seguridad que siga prácticas de programación segura o un agente de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo contiene frontmatter YAML con al menos `name` y `description`, seguido de una instrucción en Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos suelen definirse mediante archivos Markdown e indican a una IA cómo realizar tareas. La forma más sencilla de distinguirlos es que el **agente personalizado** es quien trabaja y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes mientras trabajan. En este taller, el agente personalizado de accesibilidad revisa y actualiza el sitio conforme a las directrices de accesibilidad. Para ello, podría utilizar habilidades como una dedicada al flujo de trabajo de solicitudes de incorporación de cambios o a ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier elemento de IA, prueba y repite hasta encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados explorarás:

- cómo se definen los agentes personalizados.
- cómo se asigna una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Comentarios recientes de usuarios indican que el tema oscuro actual resulta difícil de leer debido al contraste insuficiente entre el texto y el fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que los usuarios puedan activar y desactivar.

La accesibilidad es fundamental, por lo que quieres implementar esta funcionalidad lo antes posible. Utilizarás un agente personalizado para generarla.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para comprender cómo orientará a Copilot.

Vuelve a tu codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que destacan:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Crea y asigna una incidencia

El centro de control es el lugar central para gestionar todos los agentes del entorno. Puedes asignar tareas al agente de GitHub Copilot en la nube, supervisarlas e incluso redirigirlo y ofrecerle indicaciones adicionales. Asigna a Copilot la tarea de crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña de incidencias.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. En el campo **Title**, escribe `Add high contrast mode to website`.
6. En el campo **Description**, escribe:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura del cuadro de asignación al agente en la nube con las opciones del agente personalizado y Accessibility resaltadas](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y pasos siguientes

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados en tareas y ámbitos concretos. Los agentes personalizados permiten codificar los conocimientos y estándares del equipo en agentes reutilizables que orientan a Copilot para que realice determinados tipos de trabajo con más eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo se asigna una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y orientar la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para utilizar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para utilizar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
