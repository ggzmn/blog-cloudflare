---
title: "Cómo uso IA en mi flujo de trabajo sin dejar de pensar"
publishDate: "2026-05-01"
description: "No fui early adopter entusiasta. Lo adopté despacio, con desconfianza. Y creo que eso fue una ventaja."
tags: ["claude code", "copilot", "workflow", "react", "reflexión"]
pinned: true
---

Cuando GitHub Copilot empezó a aparecer en los flujos de trabajo de mi equipo, no lo rechacé — lo observé. Antes de adoptarlo quería entender qué papel iba a jugar en mi forma de trabajar. La pregunta que me hice desde el principio fue: ¿cómo sabes que el código que aceptas es realmente funcional sino lo entiendes?

Esa pregunta definió cómo lo uso hoy.

No porque crea que mi código no necesita mejorar, de hecho, _Claude_ me ha ayudado a profundizar en cosas que daba por entendidas. Sino porque quería usarlas con intención, no por inercia.

Hay ocasiones donde sí dejo que un agente resuelva algo casi solo — cuando necesito que una parte del código que desconozco se comporte de cierta forma, para poder enfocarme en lo que sí tiene que estar bien hecho y commiteado. Eso no es pereza, es _priorizar_. La diferencia está en saber cuándo hacerlo.

## La adopción lenta como ventaja

Hay una narrativa dominante sobre IA en desarrollo: hay que adoptarla rápido, usarla para todo, ser más productivo. Y es verdad que puede hacerte más rápido. Sin embargo creo que en esta competencia de aprender a usar el último agente, perdemos de vista la capacidad de cuestionarnos lo que le pedimos, lo que delegamos, el entendimiento y otras cosas más.

Veo en redes lo mucho que puedes hacer con Claude Code u otros agentes: proyectos enteros generados en minutos, demos impresionantes, flujos completamente automatizados. Pero eso no es el trabajo diario de la mayoría de developers. En la vida real no estás creando proyectos nuevos todos los días ni diseñando esquemas de base de datos desde cero. Estás manteniendo código que lleva años en producción, trabajando en un módulo que alguien más escribió, resolviendo un bug en un contexto que no está documentado en ningún lado. Adoptar agentes en ese contexto es menos espectacular y requiere más criterio, no menos.

Porque la IA no conoce tu proyecto. No sabe que ese componente tiene una restricción de negocio específica. No sabe que tu equipo decidió hace tres meses no usar cierta librería por razones que están en una conversación de Slack que nadie va a buscar. No sabe nada de eso.

Así que, en definitiva, ha aumentado mi producción y automatizado tareas que son recurrentes y repetitivas, pero no hace todo por mí.

## Las herramientas que uso y por qué cambié

No llegué a mi setup actual de golpe. Empecé con Copilot integrado a VS Code. Me parece bueno para el autocompletado y sugerencias, predice lo que vas a escribir con bastante precisión. Pero con el tiempo me encontré con sus límites: deficiente con tests unitarios, lento para dar respuesta, y con sugerencias que no contextualizaban bien el código que ya existía.

El primer cambio "fuerte" no fue por las herramientas de IA, fue por el editor. Trabaje con un monorepo grande y VS Code empezó a ser lento en todo: al abrir el proyecto, durante la edición, copilot de plano moría. Migré a Cursor principalmente por rendimiento. Su agente lo tengo desactivado, no me interesa usarlo, lo ejecute hasta que finalizó la prueba gratuita, pero la migración ocurrió más por una necesidad de performance del editor.

Para el agente uso Claude Code, corriendo desde una extensión dentro de Cursor. La diferencia respecto a un chat es concreta: tiene acceso directo al proyecto, puede leer archivos, proponer cambios y ejecutarlos. Eso cambia lo que es posible delegar.

### Ejemplo

En ese monorepo hay un middleware que levanta contenedores por aplicación. El problema era que al detectar cualquier cambio, actualizaba _todas_ las apps y no solo la que estaba editando. Era código que funcionaba pero que no me funcionaba del todo, y no era algo que pudierse optimizar yo mismo en ese momento. Lo delegué a Claude Code, le di contexto del comportamiento esperado, y lo dejé refactorizar hasta que solo actualizara el contenedor de la app modificada. Funcionó. Y yo pude enfocarme en lo que realmente importaba.

Eso es lo que significa delegar con criterio: saber qué código vale tu tiempo y qué código puede resolverse sin él.

## Dos momentos que me hicieron pensar

### El código que no podía explicar

En algún momento acepté una sugerencia de Claude que resolvía el problema que tenía. Funcionaba. Los tests pasaban. Estaba a punto de hacer el commit cuando me pregunté: _¿puedo explicar exactamente qué hace esta función?_

No podía.

No de forma completa. Entendía el resultado pero no el camino. Y eso, para mí, es una señal de alerta. No tanto porque alguien me fuera a preguntar en un code review. El problema real es otro: el código que no entiendo del todo tiende a ser explicitamente lo qué se pide en el prompt sin embargo, puede no acoplarse al contexto de negocio, omite reglas implícitas que no están escritas en ningún lado, y crea deuda para quien lo toque después.

No subí ese código. Lo reescribí hasta entenderlo. Tardé un poco más, pero en definitiva fue la mejor solución.

### La solución correcta en el lugar equivocado

La segunda experiencia fue diferente. La IA me dio una solución técnicamente impecable. Limpia, eficiente, bien estructurada. El problema era que no encajaba con cómo estaba construido el resto del módulo. Seguía un patrón distinto, usaba una abstracción que el equipo había decidido no usar, y habría creado inconsistencia en una parte del código que ya tenía suficiente deuda técnica.

Técnicamente correcto. Contextualmente equivocado.

Esto es lo que no cambia sin importar qué tan buena sea la herramienta: la IA no tiene acceso a las decisiones que se tomaron en una reunión hace seis meses, ni a la conversación de Slack donde alguien explicó por qué ese patrón no se usa aquí. Ese contexto vive en las personas, no en el código.

## Cómo la uso ahora

Con el tiempo encontré un equilibrio que funciona para mí. No es un framework ni una metodología. Son decisiones simples sobre para qué sí y para qué no.

**Para qué sí uso IA:**

- Generar y mejorar unit tests, especialmente casos borde que puedo pasar por alto
- Refactorizar código legacy que lleva mucho tiempo sin mantenimiento, le doy contexto, reviso el resultado, me aseguro que las reglas de negocio que no están implicitas permanezcan intactas
- Escribir documentación inline: JSDoc, comentarios de funciones complejas
- Redactar comentarios en PRs de forma más clara
- Resumén de código desconocido cuando entro a una parte del proyecto que no conozco
- Tareas repetitivas que no requieren decisión: renombrar variables consistentemente, formatear estructuras, generar boilerplate
- Delegar partes del código que necesitan funcionar de cierta manera, para poder enfocarme en lo que sí requiere mi atención completa

**Para qué no la uso:**

- Decisiones de arquitectura o estructura de componentes, eso requiere conocer el proyecto completo
- Lógica de negocio crítica sin revisarla línea por línea
- Cualquier cosa donde el output no pueda adaptarse al contexto real del proyecto

Una cosa que aprendí: los comentarios en el código, que parecen algo super _old school_, siguen siendo útiles cuando trabajas con código legacy o cuando la IA entra al flujo. Un buen comentario que explica _por qué_ se tomó cierta decisión es exactamente el tipo de contexto que la IA no puede inferir sola, y que sí puede aprovechar cuando está ahí. No es obsoleto, es documentar las decisiones que no se ven en el diff.

## La pregunta que me hago siempre

Antes de aceptar cualquier sugerencia de IA, me hago una sola pregunta:

_¿Puedo explicar este código como si la IA no existiera?_

Si la respuesta es sí, lo acepto, lo ajusto si es necesario y sigo. Si la respuesta es no, no lo subo hasta entenderlo.

Pero hay una capa más debajo de esa pregunta. No se trata solo de que _yo_ pueda explicarlo. Se trata de si el código tiene sentido en el contexto del proyecto: si sigue los patrones del equipo, si respeta las reglas de negocio que no están escritas en ningún lado, si otro developer puede mantenerlo sin necesitar una explicación especial.

La IA es una herramienta muy buena. Pero sigue siendo una herramienta, sin pleno conocimiento de lógica de negocio. Y si la dejas que tome decisiones de contexto por ti, el día que algo falle, no vas a saber por qué. No porque el código sea malo, sino porque nunca fue realmente tuyo.

---

_¿Tienes una regla propia para usar IA en tu flujo de trabajo? Me interesa leerla._
