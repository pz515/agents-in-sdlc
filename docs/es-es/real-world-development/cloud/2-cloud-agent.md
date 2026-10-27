---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que haya muy pocas organizaciones —si es que hay alguna— que no tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog porque no había tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina el trabajo, genera una solicitud de incorporación de cambios en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

Con el agente de GitHub Copilot en la nube, explorarás lo siguiente:

- personalizar el entorno de generación de código;
- asegurarte de que las operaciones se realicen de forma segura;
- comprender la importancia de definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco ideal: comprobarás que falta por completo. Como primer paso, quieren añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. Quieren que la página de detalles de cada juego muestre juegos relacionados —otros títulos de la misma categoría— para que las personas puedan seguir explorando. Por ahora no necesitan una implementación pulida: solo quieren algo que les permita probar la experiencia de usuario. Aunque esto bloquea su trabajo, hay otras incidencias con mayor prioridad.

Estos son ejemplos de tareas que pueden acabar relegadas y que conviene asignar al agente de GitHub Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas. Después, vuelves para revisar el trabajo de Copilot y comprobar que el resultado es el esperado.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que con una persona, puedes asignarle trabajo de distintas maneras, por ejemplo, [asignándole una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después de la asignación, Copilot crea una solicitud de incorporación de cambios en borrador para registrar su progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión de Copilot mientras sigue en curso o una vez completada. Cuando la solución propuesta esté lista para que la revises, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no puedes saltarte el trabajo de análisis. Incluso las tareas que parecen sencillas pueden volverse complejas al examinarlas en profundidad, así que [define cuidadosamente el alcance de las tareas para el agente de GitHub Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un programador en pareja con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente de GitHub Copilot en la nube, revisa los archivos de instrucciones que ya incluye este repositorio para poder comprobar su efecto más adelante.

Abre los siguientes archivos en la interfaz web de GitHub para tu repositorio o en un codespace que ya tengas en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los requisitos de los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, observa si la solicitud de incorporación de cambios resultante incluye comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Estas reglas proceden de los archivos de instrucciones. Volveremos a señalarlas al revisar las solicitudes en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente de GitHub Copilot en la nube

Crear código, independientemente de quién lo haga, suele requerir un entorno específico y algunos scripts de configuración para garantizar que todo esté en buen estado. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo creando un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. Este flujo ya está configurado para facilitar el taller y ofrecer esta oportunidad de aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para estos pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Para clonar el repositorio en los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la interfaz y la aplicación: Node.js (ahora toda la aplicación utiliza Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Fíjate en el activador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona correctamente sin tener que esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores al equipo. Copilot es especialmente útil para crear documentación, así que esta es una incidencia ideal para asignar al agente de GitHub Copilot en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio futuro volverás para revisar el resultado.

1. Abre una nueva pestaña del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de generar. Tendrá un título parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la solicitud de incorporación de cambios. Copilot está configurando su entorno (ejecutando el flujo de trabajo que se ha descrito antes), analizando el proyecto y determinando cómo abordar el problema.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate hacia abajo por la cronología de la solicitud de incorporación de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes recorrer la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a pensar y decidir el plan adecuado y, después, crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente de GitHub Copilot en la nube es permitirle trabajar de forma asíncrona para que puedas ocuparte de otras tareas. Aprovecharemos esa función y, además de asignarle otra tarea al agente, escribiremos código para añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Como ya se ha indicado, una gran ventaja del agente de GitHub Copilot en la nube es que permite dividir el trabajo: tú puedes centrarte en un conjunto de tareas mientras él se ocupa de otro. Añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, pero aun así puedes dedicar ese tiempo a otras tareas. Vamos a asignársela al agente en la nube.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás varios emoji 👀 en el primer comentario de la incidencia. Indican que Copilot ya está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente de GitHub Copilot en la nube trabaja de forma parecida a un ingeniero de software: no hace falta supervisarlo continuamente, puedes revisar el resultado cuando haya terminado. Ahora veremos cómo crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias a Copilot para que las resuelva de forma asíncrona. También puedes utilizar Copilot para abordar deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código;
- asegurarte de que las operaciones se realicen de forma segura;
- comprender la importancia de definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes empezar a crear y utilizar agentes personalizados. [El agente de GitHub Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Información sobre el agente de GitHub Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
