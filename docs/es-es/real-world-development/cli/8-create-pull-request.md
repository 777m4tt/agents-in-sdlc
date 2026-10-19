---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora los revisarás conjuntamente y abrirás una solicitud de incorporación de cambios. Tú combinaste la solicitud de incorporación de cambios (PR) de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente dividiríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad, y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- habilitarás Agent Merge solo después de revisar los cambios y confirmar que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener en manos del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y vuelve a aplicar los cambios sobre la base cuando es necesario. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora, tú has seleccionado **Merge pull request**. Agent Merge puede asumir esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Utiliza Agent Merge para gestionar la PR

Ya has creado todo el código. Ahora revísalo, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, combinará la solicitud.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos necesarios. Si el proceso se bloquea, lee el motivo indicado y, cuando la configuración del repositorio lo permita, completa manualmente la combinación que ya has revisado.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el flujo de trabajo de la solicitud de incorporación de cambios. Has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- creado la PR de filtrado.
- habilitado Agent Merge solo después de revisar los cambios y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
