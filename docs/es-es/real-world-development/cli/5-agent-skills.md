---
title: "Lección 5 - Personalizar y utilizar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y utilízala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que el código funciona y utilizado archivos de instrucciones para asegurar que siga nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la mejor opción. Las habilidades ayudan a Copilot a entender cómo ejecutar operaciones de este tipo.

En esta lección:

- explorarás la habilidad existente `quality-checks`;
- personalizarás el formato de los resultados;
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente reúnen instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. En esencia, son carpetas que llevan el nombre de la habilidad y contienen un archivo Markdown llamado `SKILL.md`. El Markdown incluye frontmatter con un nombre y una descripción, una explicación de la función de la habilidad y directrices sobre cuándo utilizarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque utiliza los comandos existentes del proyecto.

Las habilidades pueden encontrarse en `.github/skills` para estar disponibles en el repositorio y compartirse con el equipo, o en la carpeta global de habilidades del usuario, `~/.copilot/skills`.

## Explorar la habilidad

Vamos a explorar la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor de Codespaces.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo invocar la habilidad.
3. Lee las instrucciones y fíjate en cómo guían a Copilot por el proceso de pruebas y lint.
4. Observa que la habilidad aún no tiene una sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Puedes invocar las habilidades directamente desde Copilot CLI o pedirlo en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personalizar el informe

Mejoremos el informe para que indique qué se ha ejecutado, si se ha superado y qué información han devuelto las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guardará automáticamente.

## Ejecutar la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación. Comprueba el cambio:

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo prompt que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos;
- has personalizado el formato de los resultados;
- has vuelto a cargar y ejecutado la habilidad.

La actualización de la habilidad acompañará al filtrado en la PR. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación integrado: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
