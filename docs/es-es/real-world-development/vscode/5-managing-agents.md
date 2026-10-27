---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando GitHub Copilot está en modo agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como este trabajo se realiza en tiempo real en tu máquina, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que se producen. Puedes revisar cada diff en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para refinar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones de archivos propuestas;
- dirigirás al agente durante la sesión para añadir un control del modo claro junto al de contraste alto.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de contraste alto. Mientras trabaja, el equipo de diseño ha hecho una petición adicional: también quiere un control para el modo claro, pensado para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de iniciar una conversación nueva, orientarás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta. Aparecerán lecturas y ediciones de archivos, así como los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertir el cambio y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan seguros.

5. Espera a que el agente termine de implementar el modo de contraste alto. Deberías ver un mensaje final que resuma los cambios.

## Dirigir al agente para añadir el modo claro

Ahora que ya está disponible el modo de contraste alto, ampliarás la misma conversación para añadir un control del modo claro. Como continuarás en la misma sesión de chat, el agente conservará todo el contexto de su trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local que haya configurado.

1. En el campo de entrada de Copilot Chat, envía el siguiente prompt de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde donde lo dejó, explorará los cambios realizados y propondrá modificaciones para el modo claro.
3. Revisa cada nueva edición a medida que se aplica, igual que hiciste con el modo de contraste alto. Usa **Keep** o **Undo** en cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar la interfaz de los controles de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto al campo de entrada del chat y enviar un mensaje de seguimiento para aclarar lo que necesitas.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar el conjunto de archivos modificados

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver actualizaciones de componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y examina los diffs. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con el teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición de archivo, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real;
- has aceptado y rechazado ediciones de archivos individuales;
- has dirigido una sesión en curso mediante prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha producido el agente personalizado local durante este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
