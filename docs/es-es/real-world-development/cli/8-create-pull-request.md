---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de aseguramiento de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Tú combinaste manualmente la PR de valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería separar la funcionalidad, las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en una sola PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- examinarás todos los cambios de la funcionalidad y las pruebas de verificación;
- crearás la PR de filtrado;
- habilitarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

Durante el trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a basar la rama cuando es necesario. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esta parte, pero debes revisar el trabajo y confirmar que está listo antes de habilitarlo.

## Utilizar Agent Merge para gestionar la PR

Una vez creado el código, revísalo, crea la PR y permite que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agentes con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y gestionar la PR. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- has examinado todos los cambios de la funcionalidad y las pruebas de verificación;
- has creado la PR de filtrado;
- has habilitado Agent Merge después de revisar los cambios y has confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, las sesiones compartidas y la delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
