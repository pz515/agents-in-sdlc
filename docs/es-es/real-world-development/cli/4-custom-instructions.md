---
title: "Lección 4 - Orientar Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una manera concreta, conviene proporcionar a Copilot las directrices necesarias. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta;
- actualizarás el archivo de instrucciones para asegurar que se sigan los estándares de programación;
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- Los comentarios deben explicar la intención y las decisiones que no resulten evidentes, no limitarse a repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar con TSDoc/JSDoc su propósito, parámetros y valores devueltos; también deben documentar el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar el contrato `Props` y actualizar los comentarios cuando cambie el código relacionado.
- Se deben conservar las directrices existentes de formato y lint.

El uso de archivos de instrucciones permitirá proporcionar a Copilot la información adecuada para que realice las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función puede ayudarte a obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar tus convenciones de programación, bibliotecas e incluso los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y proporcionar contexto específico para cada tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto pertinente para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivos o tareas específicos.

> [!NOTE]
> Los formatos de instrucciones adicionales y su compatibilidad varían según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explorémoslos antes de realizar un cambio para poder observar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Verás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: establece un glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que debe seguir.

## Actualizar los archivos de instrucciones según las directrices del equipo

Los archivos existentes son un buen punto de partida, pero aún falta una regla. Modifica el archivo principal `copilot-instructions.md` para que se añadan comentarios TSDoc al código TypeScript nuevo.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade el siguiente elemento como último punto:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Usar las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones actualizadas, cierra y reanuda la conversación de filtrado para que Copilot use las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que describen el código con precisión.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta;
- has actualizado el archivo de instrucciones para garantizar que se sigan los estándares de programación;
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para ejecutar lint y pruebas de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
