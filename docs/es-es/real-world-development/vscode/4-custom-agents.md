---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos de tu flujo de desarrollo. Puedes definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio y proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio para que realice determinados tipos de trabajo de forma más eficaz. Los equipos pueden convertir sus conocimientos en agentes reutilizables: un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con al menos los campos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Hay cierta superposición conceptual entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar operaciones. La forma más sencilla de distinguirlos es esta: un **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades (e incluso otros agentes) mientras realizan su trabajo. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; para hacerlo, podría utilizar habilidades como una habilidad de flujo de trabajo de solicitudes de incorporación de cambios o una que ejecute y gestione pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor se adapte a tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Comentarios recientes de los usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de contraste alto que se pueda activar y desactivar.

La accesibilidad es fundamental, así que quieres implementar esta funcionalidad cuanto antes. Para generarla, utilizarás un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de contraste alto.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu codespace, abre un terminal y crea una rama nueva a partir de `main` para el trabajo de accesibilidad (así mantendrás separada la PR de filtrado del ejercicio 3):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Lee las siguientes secciones, que destacan:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las buenas prácticas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - errores y problemas habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes, en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los siguientes ejercicios, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indica cuando sea pertinente.

1. Si aún no está seleccionado, elige **Agent** en el menú desplegable de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes en la parte inferior de la vista de chat. Muestra el agente activo; de forma predeterminada, es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía el siguiente prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat empezará a trabajar: explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff que puedes revisar.
6. A medida que se apliquen las ediciones, el indicador **Files changed** se actualizará para mostrar el conjunto de archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot está realizando cambios reales en el repositorio: mientras trabaja, puede editar archivos existentes, como componentes Astro, CSS y pruebas relacionadas.

En el siguiente ejercicio revisarás y dirigirás el trabajo en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados para tareas y ámbitos concretos. Permiten convertir los conocimientos y estándares de tu equipo en agentes reutilizables que orientan a Copilot para que realice determinados tipos de trabajo de forma más eficaz.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo utilizar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisando los cambios a medida que se aplican y añadiendo un control del modo claro a la misma sesión.

## Recursos

- [Información sobre los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
