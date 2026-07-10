---
title: "Centralizar CSS puede romper todo si no entiendes el orden"
description: "Los posibles issues que pueden surgir cuando mezclamos características CSS con TailwindCSS"
publishDate: "2026-06-23"
tags: ["css", "tailwindcssv4", "custom-properties", "debugging", "migracion"]
---

## El síntoma

Hace mucho tiempo estaba revisando por qué los estilos de una plataforma que funcionaban, después de una migración dejaron de hacerlo.

Aquí hay dos características importantes, la app migró a TailwindCSS v4 y la misma tenía un theme personalizado.

## El trabajo

Se comenzó a checar con las _devtools_ e investigar desde lo básico: alguna clase, una especificidad que le gana a otra, algún _!important_.

Lo más sencillo era pensar que los estilos css se "estaban pisando". El orden importa. Siempre ha importado. Sin embargo, el css que debía aplicarse estaba ahí. [Especificidad y cascade](https://developer.mozilla.org/es/docs/Web/CSS/Guides/Cascade/Specificity)

Venía mucho más "atrás", las variables de _tailwindcss4_ y el tema global.

## CSS y el uso de variables y el valor "initial"

Todas las aplicaciones comparten e importan un archivo css _global.theme.css_, que luce más o menos así:

```css title="global.theme.css" {4}
@import 'tailwindcss';

@theme {
  --*: initial;
  /* Teal — clinical trust, "vet" side of the brand */
  --color-vet-50: #eefbf9;
  --color-vet-100: #d3f3ee;
  --color-vet-200: #a8e7dd;
  ...
```

El problema proviene del `--*:initial` y es que, en realidad lo que hace es resetear todas las custom properties incluyendo las que Tailwind define internamente para sus utilidades.

Parece una forma fácil de reutilizar variables. Sin embargo, el principal issue radica al momento de usar utilidades como `text-xs`, `p-4` etc, al no contar con un valor, las clases existen pero no generan ningún estilo porque las variables que referencian están en initial (sin valor).

Claro, entonces, lo más sencillo es declararles un valor:

```css title="global.theme.css"
@import 'tailwindcss';

@theme {
  --*: initial;

  ...

  --text-xs: 0.75rem;
  --text-xs--line-height: calc(1 / 0.75);
  --text-sm: 0.875rem;
  --text-sm--line-height: calc(1.25 / 0.875);
  --text-base: 1rem;
  --text-base--line-height: calc(1.5 / 1);
  --text-lg: 1.125rem;
  --text-lg--line-height: calc(1.75 / 1.125);
  ...

  /* Spacing */
  --spacing-xs: 0.5rem;
  --spacing-sm: 1rem;
  --spacing-md: 1.5rem;
  --spacing-lg: 2rem;

```

Pero ojo, hay que declararlas TODAS, veamos por qué:

```tsx title="welcome.tsx" {4}
export function NxWelcome({ title }: { title: string }) {
  return (
    <div className="body-wrapper">
      <div className="container max-w-md">
        <div id="welcome">
          <h1 className="text-color-paw-950">
            <span> Hello there, </span>
            Welcome {title} 👋
          </h1>
        </div>
      </div>
    </div>
  );
}

export default NxWelcome;
```

Lo que por un momento pensé es que si no declaras `max-w-md`, no debería tener valor debido al _initial_. Lo cual fue un error, porque aparecía en las devtools pero con una variable `max-width: var(--spacing-md)` que en ningún momento definí.

Leyendo la documentación de tailwind, esta indica que si el valor `max-w-md` no existe entonces usa por defecto `--spacing-*`. Ver [max-w-<number> utilities ](https://tailwindcss.com/docs/max-width#customizing-your-theme).

Pero aún hay más, tailwind revisa tres namespaces en orden de prioridad:

```js title="values.js"
["--max-width", "--spacing", "--container"];
```

Usa el primero que tenga una clave `md` definida, Como el archivo _global.theme.css_ solo sobrescribió el spacing, tomará ese valor: `1.5rem` en lugar de `28rem`.

Ver código fuente: [utilities.ts](https://github.com/tailwindlabs/tailwindcss/blob/35a3e9c5159bea77af0d48f0c8849279211cc7e9/packages/tailwindcss/src/utilities.ts#L1073-L1074)

## Conclusión

Lo que parece una simple instrucción, que incluso tailwind sugiere usar para "resetar" el theme, no es tan evidente... [Using a custom theme](https://tailwindcss.com/docs/theme#using-a-custom-theme)

Al ser una migración, había clases de Tailwind que ya funcionaban correctamente. El nuevo archivo CSS centralizado introdujo comportamientos inesperados en esas clases porque el reset afectó variables que nadie había tocado.

Considero que usar `--*: initial` tiene más sentido cuando construyes un tema desde cero y tienes control total sobre todas las variables que Tailwind necesita. En una migración, donde coexisten estilos existentes y del tema ya definido, es una instrucción demasiado agresiva. Una alternativa sería resetear solo lo que necesitas cambiar y dejar que Tailwind mantenga sus valores por defecto en el resto, por ejemplo:

```css title="global.theme.css"
@theme {
  --color-*: initial; /* solo resetea colores */
  --text-*: initial; /* solo resetea tipografía */
}
```

Para ver el comportamiento puedes descargar el repositorio [ggzmn#feat/css-vs-tailwindcssv4](https://github.com/ggzmn/canine-scheduler/tree/feat/css-vs-tailwindcssv4)
