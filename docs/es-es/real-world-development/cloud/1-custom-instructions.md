---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una manera concreta, o hay información de contexto que Copilot debería conocer, conviene asegurarse de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] permiten proporcionar esas directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no ejecutarás un prompt antes y después del cambio. El agente de GitHub Copilot en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil demostrar el efecto en tiempo real y comparar ambos resultados. Verás la influencia del archivo de instrucciones más adelante, cuando revises las solicitudes de incorporación de cambios que genere el agente en la nube.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar el modo oscuro y tener un aspecto moderno.
- Se debe añadir documentación al código en forma de comentarios de documentación TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su función.

El uso de archivos de instrucciones te permitirá proporcionar a Copilot la información adecuada para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función puede ayudarte a orientar Copilot para obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar tus convenciones de programación, bibliotecas e incluso los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y proporcionar contexto específico para cada tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto, es decir, contexto pertinente para la mayoría de las solicitudes que se envían a Copilot desde el chat o la CLI. Puede incluir la pila tecnológica utilizada, una descripción general de lo que se está desarrollando, buenas prácticas y otras directrices globales.
- Se pueden crear archivos `.github/instructions/*.instructions.md` para tareas o tipos de archivos concretos. Puedes usarlos para proporcionar directrices para lenguajes específicos (como TypeScript o Astro) o para tareas como crear un componente de interfaz de usuario o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de GitHub Copilot en la nube utilizan tanto las instrucciones a nivel de repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Además, Copilot [admite archivos de instrucciones que siguen otros estándares][custom-instructions-support], como `AGENTS.md` y `CLAUDE.md`.

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices a nivel de proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para proporcionar instrucciones específicas para tipos de archivos (pruebas unitarias, componentes de Astro o la capa de datos) o tareas concretas.
- Usa lenguaje natural. Expresa las directrices con claridad e incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única forma de crear archivos de instrucciones, igual que no hay una única forma de usar la IA. La experimentación te permitirá descubrir qué funciona mejor para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían contar con una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, puede que veas archivos para muchos tipos de tareas, incluidas las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada superficie ofrece esta opción de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección de la superficie que estés utilizando la indicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas **GitHub Actions Workflows**. Se aplican a cualquier interacción que tengas con Copilot.
3. Abre la carpeta `.github/instructions` y echa un vistazo. Verás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: establece un glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincidirá cualquier archivo de prueba TypeScript (por ejemplo, uno que coincida con `**/*.test.ts`).
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que debe seguir al generar código. (Las rutas indicadas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios de documentación TSDoc y los encabezados de comentario de los archivos.

## Añadir un nuevo estándar del repositorio

Este es el único cambio que harás en esta lección: añadir una regla para todo el proyecto que indique que la documentación debe incluirse en el código mediante comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube aplicará esta regla al trabajar en las incidencias que le asignes más adelante en este itinerario.

Antes de editar, prepara una rama de trabajo (el agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así podrás confirmar los cambios de forma ordenada):

1. Desde el terminal de tu codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha indicado antes, `.github/copilot-instructions.md` está pensado para proporcionar información del proyecto a Copilot. Añade los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de programación del proyecto, pero todavía no tiene una regla para la documentación dentro del código; por eso el helper generado no incluía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo debajo de los estándares existentes para indicar a Copilot que añada encabezados de comentario a nivel de archivo y comentarios de documentación TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones pueden crearse a nivel de repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales, o como archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretos. El archivo a nivel de repositorio es el lugar adecuado para estándares generales del proyecto, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirmar, enviar y combinar la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio asignarás una incidencia a Copilot y este creará una rama a partir de `main`; por eso, los cambios de instrucciones deben llegar a `main` para que el agente pueda utilizarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. El agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama permite que el taller refleje cómo trabajarías en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has añadido un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`. Verás cómo se aplica ese estándar en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto de este itinerario.

A continuación, [asigna tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
