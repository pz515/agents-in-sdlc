---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Veamos otros que pueden ser útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- utilizarás `/model` para explorar los modelos disponibles;
- aprenderás a exportar o compartir una sesión con `/share`;
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en el agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de CLI. Ahora verás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar trabajo en el [agente de Copilot en la nube][about-cloud-agent].

## Explorar el contexto de Copilot CLI

En tareas grandes o complejas puedes alcanzar la ventana de contexto máxima del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario, y puedes examinarla o compactarla manualmente con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es corta, puede que el cambio no sea muy grande.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` si vas a cambiar a una tarea no relacionada y quieres iniciar una conversación nueva.

## Elegir un modelo

Los distintos modelos tienen puntos fuertes diferentes y cada desarrollador puede tener sus preferencias. Copilot CLI permite mostrar y seleccionar el modelo que quieres utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Colaborar y compartir lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar el trabajo o delegarlo

Has completado el núcleo del taller. Copilot CLI también tiene comandos para tareas más grandes:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo;
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea distinta;
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva con un alcance claro y revisa los resultados con el flujo habitual. Si quieres aprender más sobre el trabajo asíncrono de agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y ver qué ocurre internamente. En esta lección:

- has utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- has utilizado `/model` para explorar los modelos disponibles;
- has aprendido a exportar o compartir sesiones con `/share`;
- has explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en la nube.

Copilot CLI incluye más comandos de barra y muchas otras posibilidades. Para terminar el recorrido, [repasaremos lo aprendido][next-lesson] y veremos cómo seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Información sobre el agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
