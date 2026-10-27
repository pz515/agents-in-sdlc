---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En lugar de recorrer la aplicación manualmente, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que controle un navegador real para probarla. Después, publicarás la rama y abrirás una PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadirás el servidor MCP de Playwright al espacio de trabajo;
- pedirás a Copilot que lo utilice para probar manualmente la funcionalidad de filtrado en un navegador;
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente resulta mucho más útil cuando puede acceder a recursos externos al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

El acceso a estas herramientas y recursos se realiza a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas o servicios externos. El servidor MCP gestiona la comunicación con herramientas externas, como API existentes o herramientas locales (por ejemplo, paquetes de NPM). Cada servidor MCP ofrece un conjunto distinto de herramientas y recursos al agente.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server):** proporciona acceso a un conjunto de API para gestionar repositorios de GitHub. Permite al agente realizar acciones como crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp):** proporciona funciones de automatización del navegador mediante Playwright. Permite al agente realizar acciones como navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a diferentes herramientas y recursos. GitHub aloja un [registro MCP](https://github.com/mcp) para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa cuidadosamente su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Utiliza únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías ver que ya está configurado un servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye esta entrada `github`, por lo que Copilot pudo consultar el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor GitHub MCP remoto][remote-github-mcp-server], así que no hay que instalar nada localmente: VS Code se conecta mediante HTTP y te autenticas con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor `github` remoto, `playwright` es un servidor **local**: VS Code lo inicia en tu máquina con `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, algo necesario en un codespace, donde no hay un escritorio que pueda mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si más adelante Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar y autorizar el servidor

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot utiliza las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y autorízalo para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code lo iniciará —y te pedirá que confirmes que confías en él— la primera vez que Copilot necesite sus herramientas en el siguiente paso.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] permite que Copilot controle un navegador real. En lugar de recorrer la aplicación manualmente para comprobar el trabajo, el agente puede abrir una página, navegar por ella, aplicar filtros, leer los resultados y resumir lo que ha observado. Es una forma rápida de confirmar que una funcionalidad se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright utiliza el [árbol de accesibilidad][accessibility-tree] de la página en lugar de capturas de pantalla. Así, el agente analiza elementos estructurados y etiquetados (botones, enlaces y elementos de lista), igual que lo hace la tecnología de asistencia; por tanto, una comprobación funcional rápida también sirve como prueba básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad de filtrado que acabas de crear:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará sus observaciones. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para realizar esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot utilice el servidor MCP de Playwright, quizá tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[accessibility-tree]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Ahora que has comprobado que la funcionalidad funciona, puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios realizados por Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios: desde github.com o con la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, deja que la cree por ti. Puede buscar la incidencia pertinente y asociarla a la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado en el menú desplegable de agentes para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot obtenga información y realice operaciones. La primera vez que utilice una herramienta de GitHub, puede que tengas que autenticarte; sigue las indicaciones.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace generado por Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- has añadido el servidor MCP de Playwright al espacio de trabajo;
- lo has utilizado para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla;
- has publicado la rama y abierto una solicitud de incorporación de cambios para el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
