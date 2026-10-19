---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es esencial cuando se trabaja con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una manera concreta, o hay información de fondo que Copilot debe conocer, asegúrate de que pueda acceder a ese contexto. Hay varias formas de proporcionar contexto específico a Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de código y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones aplicables a rutas concretas.
- generarás el primer fragmento de la capa de datos para el filtrado (un ayudante de editoriales) con las instrucciones actuales.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volverás a ejecutar la misma instrucción y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás los cambios de instrucciones y el fragmento de filtrado en `main` para que Copilot pueda aplicar las nuevas directrices en el próximo ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares indicados. Copilot no es determinista. El objetivo de este ejercicio es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su función.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información necesaria para realizar tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para ayudarle a entender tu estilo de programación y tus requisitos. Permiten orientar a Copilot para que genere sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de código, las bibliotecas y los tipos de comentarios que prefieres. Es posible crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes relativas al repositorio. Debe contener información general del proyecto, pertinente para la mayoría de las solicitudes de chat o CLI: por ejemplo, la pila tecnológica, una descripción general de lo que se está desarrollando, prácticas recomendadas y directrices globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivo específicos. Puedes usarlos para indicar directrices para lenguajes concretos, como TypeScript o Astro, o para tareas como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completado de código ni sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de GitHub Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` para generar código.
>
> Copilot también [admite archivos de instrucciones en otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Crear archivos de instrucciones en detalle queda fuera del alcance del taller. No obstante, los ejemplos del proyecto de muestra ilustran un enfoque representativo. En general:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices generales del proyecto, como qué se está desarrollando, la estructura del proyecto y los estándares globales de código.
- Utiliza archivos `*.instructions.md` para indicar directrices específicas de tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) o tareas concretas.
- Utiliza lenguaje natural. Expresa las directrices con claridad e incluye ejemplos de cómo debe y cómo no debe ser el código.

No hay una única forma correcta de crear archivos de instrucciones, del mismo modo que no hay una única forma de utilizar la IA. Mediante la experimentación descubrirás qué funciona mejor para tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los archivos de este proyecto, quizá observes que abarcan muchos tipos de tareas, como [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de manera diferente; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno que estés usando te indicará cuándo corresponde.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Estas se aplican a todas las interacciones con Copilot en este repositorio.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de pruebas TypeScript, como los que cumplen `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones más extensos en archivos reutilizables y señalar ejemplos que Copilot pueda seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero todavía no exige documentación en el código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los comentarios de cabecera de archivo.

## Crea una rama para los cambios

Sigue algunas prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace que utilizaste en el ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección de modo y modelo varía según el ejercicio; cada paso lo especificará cuando sea pertinente.

Para observar el efecto de las instrucciones personalizadas, primero envía una instrucción con el archivo actual. Más adelante lo actualizarás y volverás a enviar la misma instrucción para comparar el resultado.

1. Cierra las pestañas del editor abiertas en ejercicios anteriores para que Copilot solo utilice el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el ayudante.
3. En la lista desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla que muestra el selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía esta instrucción:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios de código, que suelen afectar al archivo del ayudante y a sus pruebas.
6. Observa que el ayudante propuesto es una función tipada que acepta un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios de documentación TSDoc y el comentario de cabecera del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, es posible que añada comentarios de documentación aunque no se lo indiques. No pasa nada: el objetivo sigue siendo observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar para todo el repositorio

Como se ha indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Documenta los estándares de código del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares de código, pero todavía no incluye reglas para documentar el código; por eso el ayudante generado no tenía comentarios.
3. Justo después de los estándares existentes, añade estas líneas Markdown para pedir a Copilot que incluya comentarios de cabecera y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para ofrecer directrices globales, o usar archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Vuelve a ejecutar la instrucción y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía la **misma instrucción** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto empieza ahora con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique a código real sobre el que se basará el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones, como cualquier otro recurso del proyecto, se gestionan mediante el control de código fuente. Confirma los cambios y publica la rama en el repositorio.

1. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. En el terminal, ejecuta lo siguiente para confirmar que se han aplicado las actualizaciones de instrucciones y del ayudante:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya modificado para crear el ayudante:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Una vez publicada la rama, crea una solicitud de incorporación de cambios y vincúlala a la incidencia del backlog sobre el estándar de documentación. Copilot puede buscar la incidencia, crear una PR que la cierre y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub por ti. La plantilla del proyecto ya conecta las herramientas de GitHub al espacio de trabajo, así que no hace falta configurar nada. La primera vez que Copilot utilice una herramienta, puede que VS Code te pida iniciar sesión en GitHub. Sigue las instrucciones para conceder acceso. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en la lista de agentes.
3. Pide a Copilot que busque la incidencia sobre las instrucciones y cree una PR desde la rama actual que describa tanto la actualización de instrucciones como la nueva base del ayudante de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando se te pida **Allow** para que Copilot realice acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se haya creado la PR, pide a Copilot que la combine y vuelva a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando se te pida **Allow** para que Copilot realice acciones de GitHub y ejecute comandos del shell, revisa el comando y selecciona **Allow** si corresponde.

¡Has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y pasos siguientes

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y, después, has utilizado Copilot Chat en VS Code para:

- enviar una instrucción de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volver a enviar la misma instrucción y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar las instrucciones y la base del ayudante en `main` para que el siguiente ejercicio pueda aprovecharlas.

A continuación, aplicarás las instrucciones en [modo agente][next-lesson] mientras Copilot añade una funcionalidad al código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales de GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
