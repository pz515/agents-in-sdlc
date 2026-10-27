---
title: "Lección 10 - Resumen y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado precargada;
- has añadido valoraciones por estrellas, revisado el resultado en un navegador redirigido y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la funcionalidad con Autopilot y revisado el resultado en modo Interactive;
- has orientado al agente con instrucciones personalizadas, actualizado la habilidad `quality-checks` y utilizado esa habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para probar el filtrado en un navegador real;
- has creado y seleccionado un agente personalizado de aseguramiento de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador;
- has revisado el cambio completo de filtrado y autorizado Agent Merge para la PR correspondiente;
- has explorado comandos de barra para el contexto, los modelos, las sesiones compartidas y la delegación opcional en la nube.

## Qué has entregado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizada:

1. **Valoraciones por estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar.
2. **Filtrado y flujo de trabajo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde que planificaste el filtrado hasta que abriste su PR, utilizaste la misma conversación y rama. Combinamos el trabajo en una sola PR para agilizar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: mediante pruebas automatizadas, tu propia inspección en el navegador y la exploración del navegador por parte de Copilot a través de MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, e incluyó los resultados en el nuevo formato. QA combinó esos resultados con la revisión de los requisitos y la cobertura antes de abrir la PR.

Las pruebas nuevas deben resolver carencias reales; si una ejecución de QA determina que no hacen falta pruebas, también puede ser correcto. Antes de autorizar la combinación, revisa el código y las pruebas, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Puedes reutilizar estas personalizaciones en distintas conversaciones y adaptarlas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Utiliza **Plan** para definir el enfoque antes de empezar, **Interactive** para mantener el control de cambios específicos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y uno más potente para trabajos complejos.

El contexto es tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* puede cambiar considerablemente el resultado.

## Más opciones para explorar

Ya has recorrido el flujo de trabajo principal. Estas funciones de CLI también pueden ser útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Pasos siguientes

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Backer Concierge de Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
