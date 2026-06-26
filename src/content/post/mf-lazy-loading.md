---
title: "Module federation y el costo de cargar todo"
publishDate: "2026-03-01"
description: "Eager loading disfrazado de lazy loading: a veces optimizar performance no es tocar el código, es cuestionar la arquitectura."
tags:
  ["module federation", "performance", "react", "arquitectura", "lighthouse"]
pinned: true
---

Hace bastante tiempo colaboré en el desarrollo de una plataforma cuyas apps estaban distribuidas en repos individuales que tiempo después vivirían una migración enorme. Lo novedoso era el uso de _Module Federation_, porque cambia por completo la arquitectura.

## Auditoría

El equipo lidiaba con un "pequeño" problema, si bien, desde siempre la aplicación tardaba en descargarse, los tiempos con MF no sólo habían aumentado sino que habían empeorado.

Me daba mucha curiosidad saber que ocurría así que me puse a revisarlo detalladamente.

## Lighthouse

El paso más simple fue comenzar con _Lighthouse_. Sí, sí, sí, había carga de fuentes que tardaban, archivos CSS que se podían optimizar. Lo que me llamó la atención fue la descarga masiva de archivos js y es que al parecer los _entry point_ se descargaban siempre que el usuario cambiaba entre módulos.

## Contexto

La plataforma tenía aplicaciones independientes, es decir, no había lógica de negocio compartida.

Como les digo, se comenzó a utilizar _Module Federation_.

La base es super interesante, cada aplicación puede mantenerse encapsulada, inclusive con diferente stack en el front end. Cada _remote_ expone un componente que el host consume en su configuración.

Si bien MF lleva años como patrón de arquitectura y especialmente se observa en los _microservicios_, yo no había tenido la oportunidad de verlo en acción en el front end.

Creo que parte de usar MF fue debido a que la plataforma tenía muchas dependencias en común y se podían reducir los tiempos de carga, ya que al navegar del módulo A al módulo B: React, React Router, etc., se volvían a cargar.

Más allá de cuestionar decisiones, estamos aquí para analizar lo que encontré, esa es la parte interesante de todo esto.

### Retomando el tema

Entonces, el problema era que el tiempo de carga de la app había aumentado. La revisión comenzó con Lighthouse y en el se detectó la carga de varios componentes expuestos como _remote_, lo que indicaba que la carga _on demand_ no estaba implementada correctamente.

Aquí podría existir una pregunta muy válida: ¿por qué no cargar todos los componentes de MF de una vez? Bueno, porque no todos se ocupan de una vez. Tiene sentido compartir de entrada React, React Router, el theme. Eso sí se usa en todos lados. Pero las aplicaciones expuestas como _remotes_ deberían cargarse on demand.

No es óptimo cargar 20 _remotes_ si el usuario logueado solo tiene acceso a 2 o a 10, y sobre todo porque tal vez ese usuario solo iba a interactuar con una app y ya.

Aquí está la parte más interesante: el tema ya no es una optimización de fuentes, ni _code splitting_. La optimización proviene de la arquitectura y de cómo se decidió cargar estos _remotes_. Y aunque de primera instancia se podría pensar que el problema se resuelve con _lazy loading_, no precisamente, porque esta ya existía en el código. _Eager loading disfrazado de lazy loading._

> MF había resuelto la descarga repetida de dependencias compartidas: React, React Router, entre otras. Sin embargo, el menú de navegación dependía de las rutas de cada remote, y cada remote tenía sus propias sub-rutas con lazy loading. Eso obligaba al host a conocer toda esa estructura antes de renderizar, de lo contrario la app fallaba en runtime porque las rutas aún no existían al momento de montar el componente.
>
> La solución fue hacer la carga asíncrona: esperar a que todos los remotes estuvieran
> resueltos antes de renderizar.
>
> Funcionaba. **Pero asíncrono no es lo mismo que on demand**. Todos los remotes se descargaban y eso se repetía cada vez que el usuario navegaba entre aplicaciones y el componente de rutas se volvía a montar.

## Propuestas sobre la mesa

La validación podría haber comenzado por:

- Que el host solo resolviera el remote hasta que el usuario navegara a esa ruta, no al autenticarse.
- Que en el network tab solo apareciera la descarga del entry point del remote correspondiente, y no de todos.
- Que usuarios con acceso limitado a módulos no descargaran código que nunca iban a ejecutar.

La propuesta tenía una limitación clara: si cada remote manejaba su propio router con sub-rutas, el host necesitaba conocer algo de esa estructura para no romper la navegación directa por URL. Eso implicaba una relación entre host y remotes que no existía...

La solución parece obvia, pero hay una tensión con React Router v7: por defecto necesita conocer todas las rutas del árbol desde el inicio. Eso significa que el host no puede diferir la carga de un remote y al mismo tiempo registrar sus sub-rutas dinámicamente.

Aqui radicaba la tensión dentro de la arquitectura: mientras MF promete carga on demand por diseño, la restricción de React Router lo hacía técnicamente inviable sin redefinir el "contrato" entre host y remotes.

> By default, React Router wants you to provide a full route tree up front via createBrowserRouter(routes). This allows React Router to perform synchronous route matching, execute loaders, and then render route components in the most optimistic manner without introducing waterfalls. The tradeoff is that your initial JS bundle is larger by definition — which may slow down application start-up times as your application grows.

Entonces, la solución más limpia no dependería de React Router sino de la arquitectura de MF cada remote podría publicar un manifiesto estático con sus rutas, y el host lo consumiría para registrarlas sin necesidad de descargar el remote completo. Así el host conocería la estructura de rutas de cada uno sin resolverlo, y solo descargaría el remote cuando el usuario navegara a esa ruta.

Haciendo un poco de research encontre que ReactRouter sabe de esta limitante y por lo tanto disponibilizaron una alternativa llamada [_PatchRoutesOnNavigationFunction_](https://reactrouter.com/api/data-routers/createBrowserRouter#optspatchroutesonnavigation), en teoría, define porciones del árbol de rutas de forma diferida durante las navegaciones.

No alcancé a validar ninguna de las dos en producción y, el refactor siguió fuera de mis tareas asignadas. Pero la dirección era clara: el problema no era de código, era de arquitectura. Y la solución tampoco iba a venir de un workaround en el router.

El código no era el problema. Nunca lo fue.
