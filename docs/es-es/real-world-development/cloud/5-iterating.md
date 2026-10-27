---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

A lo largo de este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste a Copilot que añadiera documentación al código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos controles para los modos de contraste alto y claro. Vamos a revisar los cambios del código y, si hace falta, enviar comentarios a Copilot para mejorar el resultado.

### Escenario

Como ya hemos indicado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre queremos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con eso en mente, revisemos las sugerencias de GitHub Copilot para crear la documentación, la funcionalidad de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de GitHub Copilot en la nube realiza sus tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que todo siga siendo seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) creada por el agente de GitHub Copilot en la nube: añadir documentación al código. Utilizarás la interfaz estándar de solicitudes de incorporación de cambios de GitHub.com.

> [!NOTE]
> Al revisar la solicitud, puede que aparezca una advertencia indicando que un firewall ha bloqueado GitHub Copilot. **Es normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente de GitHub Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de incorporación de cambios.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation** o algo más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes hacer una pausa o repasar lo que has aprendido hasta ahora.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que ahora incluye comentarios de documentación TSDoc y otros cambios de documentación. Los cambios exactos variarán.

   Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Deberías ver que los flujos de trabajo se ponen en cola en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no es un proceso unidireccional. Puedes mencionar a Copilot en comentarios, igual que harías con otros miembros del equipo, tanto en la solicitud como en comentarios insertados en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados son no deterministas, no podemos prescribir un texto concreto. Puedes pedirle, por ejemplo, que:

- añada al principio de cada archivo de código un encabezado con una breve descripción de su función;
- añada comentarios de documentación TSDoc a archivos TypeScript y Astro;
- cree un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otro usuario. Usa una de las ideas anteriores u otra sugerencia sobre la documentación que quieras incluir en el código base.
2. Selecciona **View Session** para ver el trabajo de Copilot. Observa cómo inicia una nueva sesión para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud de incorporación de cambios.

    ![Botón Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer una nueva confirmación en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Opción para marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Volvamos a la solicitud que Copilot creó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Show related games on the game details page** o algo más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código que ha generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Deberías ver que los flujos de trabajo se ponen en cola en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente la funcionalidad de juegos relacionados. Ve a tu codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar con el agente de GitHub Copilot en la nube y solicitar actualizaciones. Por ejemplo, puedes pedirle que cambie el número de juegos relacionados que se muestran o que añada encabezados de comentario y comentarios de documentación TSDoc (recuerda que la incidencia se asignó **antes** de actualizar las instrucciones personalizadas). Al igual que antes, puedes solicitarlo en un nuevo comentario de la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir tanto el modo de contraste alto que asignaste en el ejercicio 3 como el modo claro que solicitaste desde el centro de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Add high contrast mode to website** o algo más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código que ha generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para cambiar de modo;
   - el uso del almacenamiento local para guardar las preferencias del usuario;
   - los cambios de CSS o de estilos para los modos de contraste alto y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.);
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Deberías ver que los flujos de trabajo se ponen en cola en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente las funciones de accesibilidad. Ve a tu codespace, abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los controles de los modos de contraste alto y claro en el navegador. Comprueba que funcionan según lo previsto y que las preferencias se mantienen al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones siguiendo buenas prácticas de accesibilidad. Si detectas problemas de accesibilidad o posibles mejoras, puedes mencionar a **@copilot** en un comentario y solicitar cambios, igual que hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube es más útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otras tareas. Para crear ese hábito, registra algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés para patrocinadores en la página de detalles del juego;
- implementar la paginación en la lista de juegos;
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has examinado los archivos de instrucciones personalizadas incluidos en este repositorio** para ver más adelante su efecto en el resultado del agente en la nube.
- **Has asignado incidencias al agente de GitHub Copilot en la nube** y has visto cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir controles de los modos de contraste alto y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las solicitudes de incorporación de cambios del agente en la nube e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de GitHub Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
