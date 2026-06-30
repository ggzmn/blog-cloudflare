---
title: "La IA es mejor escribiendo código que dándole mantenimiento"
publishDate: "2026-05-27"
description: "Pedí un refactor, obtuve uno. El problema es que nadie preguntó cuándo debería ejecutarse ese hook"
tags: ["reflexión", "inteligencia artificial", "carrera"]
draft: true
---

Claude se ha convertido en mi agente AI favorito, en serio... Es la primera vez que sucumbo a "mis bajos instintos" de pagar la suscripción de una herramienta de trabajo, sola por mi cuenta.

Desde hacerle preguntas filosóficas cuestionándole la neutralidad de sus respuestas, hasta Claude Code ayudándome con refactors, test unitarios, preguntas técnicas, etc.

Sin embargo siempre me salta una pregunta: ¿la estaré usando bien?, ¿el código que reviso realmente lo entiendo?, ¿esto no rompe el patrón del framework/librería? Y muchas otras preguntas.

## Tim Corey - Using AI will not make you a better developer

Como muchos saben, el mundo dev se divide en dos ahora mismo: los code vibers y los escépticos. Agregaría un tercero a este diagrama de Venn: los de en medio.

Me parece que [Tim Corey](https://open.spotify.com/episode/6OxtShLRDQCQAmV3iGPD1f?si=771f5f784f6e4260) es de los que queda en medio. En este episodio toca esta misma pregunta y nos deja algo muy claro: cuando no sabes lo que no sabes, no haces las preguntas correctas, escribes el código tal cual lo dio el agente. Lo que da más miedo es que funciona, y da miedo porque si no conoces lo que se escribió, no podrás darle mantenimiento y ni siquiera tendrás la certeza de que esta bien.

¡Es aterrador! dice Corey.

## La anécdota

Cuando recién comencé a utilizar Claude, lo hacía desde el sitio web. Solo quería mejorar el coverage de unos test unitarios de mi propio código y refactorizar código viejo.

Sin embargo, me topé con lo que comenta Corey: Claude reescribió el código de un _custom hook_ viejo y cuando hice la revisión me di cuenta de que no respetaba la lógica de los _custom hooks_. Hizo lo que le pedí pero no era un buen refactor. Ahora que tengo Claude Code me preguntó si con el _skill_ correcto y con el contexto del proyecto sabría realizar un buen refactor... Tal vez sí.

Pero aquí entra una duda oportuna: la mayoría de los devs (o por lo menos aquellos que inician su carrera) no van a pagar una suscripción para trabajar mejor, y no es un juicio, es una realidad. Es probable que muchos estén usando el sitio web del agente sin contexto del proyecto, sin un prompt bien definido, y aceptando el output tal cual.

## Deep inside

Vamos a desmenuzar el "problema", dos componentes uno que mostraba data de acuerdo a _keyWatched_ de un formulario de _ReactHookForm_