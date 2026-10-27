---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste al agente de GitHub Copilot en la nube que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. Puedes examinar las operaciones realizadas e incluso dirigir una sesión en curso para ayudar a orientarla.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para solicitar funcionalidades adicionales.

## Escenario

Después de asignar al agente la tarea de crear un modo de contraste alto, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de selección de modo, parecía lógico incluir esta funcionalidad. Quieres orientar el trabajo del agente para asegurarte de que añada tanto el modo claro como el contraste alto.

## Revisar las tareas del agente de GitHub Copilot en la nube

Veamos el estado actual de todas las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Fíjate en la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - generar API para modificar juegos;
   - añadir un modo de contraste alto al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, que pueden incluir:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Fíjate también en el panel de solicitudes de incorporación de cambios de la derecha. Desde ahí puedes consultar la solicitud y los archivos modificados para supervisar el trabajo.

## Dirigir el agente en la nube

Ahora que has visto las tareas activas, vamos a pedir a Copilot que añada el control del modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Sigue la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido e incorpora la solicitud a su flujo de trabajo.

## Dejar que Copilot trabaje

Al igual que antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo de trabajo después de completar el paso en el que estaba trabajando cuando enviaste el mensaje.

Como antes, el proceso tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo en curso e incluso redirigir tareas en curso para ajustar su alcance o proporcionar más orientación.

Has explorado estos conceptos:

- utilizar el centro de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar funcionalidades adicionales.

Mientras Copilot termina de trabajar en las funcionalidades de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
