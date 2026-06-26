---
title: "Centralizar CSS puede romper todo si no entiendes el orden"
description: "Centralizar CSS suena bien en papel. Esto es lo que pasa cuando"
publishDate: "2026-06-23"
tags: ["css", "tailwindcss", "microfrontends", "monorepo", "arquitectura"]
draft: true
---

## El síntoma

Hace mucho tiempo estaba revisando por qué los estilos de un microfrontend estaban pisando los de otro. Lo primero que hice fue lo que cualquiera haría: abrir el inspector , ver qué regla estaba ganando, y rastrear de dónde venía.

Ahí comienza el camino...

## El trabajo

Lo primero que hice fue checar con las _devtools_ e investigar comenzando por lo básico, alguna clase, una especificidad que le gana a otra, algún _!important_.

Lo más sencillo era pensar que los estilos css se "estaban pisando". El orden importa. Siempre ha importado. Sin embargo, el css que debía aplicarse estaba ahí. [Especificidad y cascade](https://developer.mozilla.org/es/docs/Web/CSS/Guides/Cascade/Specificity)

Venía mucho más "atrás", las variables de _tailwindcss4_ o estaban vacias o con un valor sobreescrito.

Para este momento todavía seguía dudando de alguna especificidad,las reglas css se aplican sin excepción no importa si usamos tailwind o un preprocesador.

Porqué seguir dudando de los estilos pues bien porqué el orden de importación támbien incluye la especificidad plataforma de _microfrontends_ (MFEs) tenía dos posibles responsables, el archivo styles.css en del root del proyecto, otro más dentro de las _lib_ y uno más dentro de cada MF.

En ese momento mi lógica fue comenzar del MFE hacía a fuera, debido a las importaciones de tailwindcss y los archivos styles.css

Describes lo que veías: estilos rotos, inconsistentes entre MFEs, sin razón aparente. Sin contexto técnico todavía, solo lo que experimentaste. 2. El momento "esto no tiene sentido"

Cuánto tiempo tomó encontrar el problema, qué cosas descartaste antes, por qué CSS en microfrontends hace que este tipo de bug sea difícil de rastrear. 3. La línea culpable

Introduces --\*: initial y explicas qué hace exactamente. Aquí está el peso técnico del post, pero acotado a una sola cosa. 4. Por qué el orden importa

Cómo Tailwind 4 asume que @theme se ejecuta primero, y qué pasa cuando esa suposición se rompe en un contexto de múltiples apps importando el mismo archivo. 5. Lo que cambió en cómo piensas

No una solución genérica. Tu conclusión personal: qué preguntas te haces ahora cuando ves un archivo CSS compartido en un monorepo.
