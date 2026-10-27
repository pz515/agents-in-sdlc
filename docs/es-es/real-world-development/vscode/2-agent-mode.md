---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a hacer cambios. Y la lista continúa.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizar el modo agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube trabaja de forma asíncrona como un miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- utilizar el modo agente de GitHub Copilot para implementar funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generadas por el modo agente antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior añadiste un helper de editoriales; ahora terminarás con el modo agente de Copilot el resto del trabajo de la capa de datos, la interfaz y las pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local directamente desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con scripts `<script>` de Astro pequeños y específicos que utilizan las API DOM estándar.

### Iniciar el sitio web

Para iniciar el sitio con facilidad, se proporciona la tarea `npm run dev`, que ejecuta el único servidor de desarrollo de Astro. Sigue estos pasos en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar un enlace localhost desde el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, donde ahora se ejecuta el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio:** muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego:** al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los siguientes ejercicios, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indica cuando sea pertinente.

La implementación inicial del sitio funciona, pero queremos mejorarla con nuevas funcionalidades. Empecemos revisando el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente trabaja de forma autónoma: planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y utiliza herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan preferencia por un modelo concreto. Al preparar el taller se utilizó Claude Sonnet 4.5, por lo que se incluye en estas instrucciones con la esperanza de que las sugerencias sean relativamente uniformes y la experiencia resulte satisfactoria. Sin embargo, como los modelos de lenguaje grande (LLM) son probabilísticos, es normal que las sugerencias difieran de las indicadas en el taller.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede utilizar un comando MCP distinto y aun así completar la tarea.

3. Envía a Copilot este prompt para consultar las incidencias del backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se enumeren todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha priorizado los elementos según los que considera más útiles para el usuario.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las directrices generales para añadir funcionalidades, entre ellas:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones para distintos tipos de archivos, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios distintos en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código base. El modo agente de Copilot puede realizar estas tareas. Añade la funcionalidad:

1. Continúa en la conversación actual de Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar los helpers de la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso quizá aparezcan los controles **Keep** y **Undo** en el editor. Cuando termine, tendrás un control **Keep** o **Undo** para todos los cambios; no hace falta seleccionarlos mientras siga trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa del modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has utilizado el modo agente de GitHub Copilot para añadir funcionalidades al sitio web de Tailspin Toys. Has aprendido a:

- utilizar el modo agente para implementar funcionalidades en la interfaz y la capa de datos;
- utilizar el modo agente para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generadas antes de combinarlos en el código base.

Ahora [prueba la funcionalidad con el servidor MCP de Playwright][next-lesson] y abre una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo que has aprendido para pedir a Copilot que implemente esta función. Ten en cuenta lo siguiente:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación y si el usuario podrá elegir el tamaño de página o será fijo;
- proporciona a Copilot las directrices necesarias para crear la implementación que necesitas;
- puede que tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto; es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
