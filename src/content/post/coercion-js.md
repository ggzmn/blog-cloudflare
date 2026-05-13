---
title: "Validación más limpia con coerción y ES6"
description: "Cómo simplificar validaciones complejas usando coerción de JavaScript y métodos de ES6, reduciendo casos de prueba y haciendo el código más reutilizable."
publishDate: "2021-07-01"
tags: ["javascript", "es6", "testing", "coercion"]
---

> 📦 **Recuperado de [dev.to](https://dev.to) — escrito hace casi 5 años.** Lo rescato tal como estaba: una foto de cómo pensaba entonces. Parte de crecer es poder mirar atrás y ver el camino.

---

*Coerción de manera simple: Es la acción de forzar a que un objeto se comporte como si fuera de otro tipo.*

Hace unos días estaba viendo un curso de React en alguna plataforma de educación online, me percaté que tenían una validación del siguiente tipo.

> ⚠️ *El ejemplo de código original estaba alojado en Glitch, plataforma que cerró su servicio de hosting. Ya no tengo acceso al embed original.*

A pesar de que es funcional y correcto, no me dejó muy conforme, ya que actualmente realizo test unitarios con Mocha & Chai y una de las premisas de los test unitarios es la validación de todos los casos de la porción de código a testear.

¿Te imaginas cuántos casos existen en la validación del `if` de la línea 17? Te cuento:

1. El primero es cuando todos los `x.y.trim() === ''` son falsos.
2. Cuando todos los `x.y.trim() === ''` son verdaderos.
3. Cuando tiene algún verdadero (en este caso son 5 validaciones, una por posición).

Son aproximadamente 7 casos 🤯 tomando en cuenta aquellas validaciones necesarias de acuerdo al operador lógico. La pregunta es: ¿para qué tantas validaciones? La respuesta es: para que el UT cubra todos los casos (o por lo menos la mayoría) y alcances la cobertura necesaria. Por otro lado, si ya lo has notado, los únicos valores que te podría retornar esa función son `true` o `false`… Muchos casos para el valor a retornar, ¿no crees?

---

¡Manos a la obra! Podrías implementar una función más "corta" en la que se omitan los operadores lógicos y en donde sólo tendrías que hacer test para dos casos: si te retorna `true` o `false`.

```js
const searchEmpty = (obj) =>
  Object.keys(obj).every((key) => Boolean(obj[key].trim()));
```

La otra característica es que sería una función más reutilizable — ahora funcionaría con objetos con otras keys y diferente número de keys. Todo esto gracias a la coerción y ES6.

---

Te explico: la función hace uso de `every`, que en términos generales determina si **todos** los elementos en el array satisfacen una condición.

*¡What! ¿Un array? Pero si veo un objeto…*

Sí, es un objeto, pero `Object.keys` es un método usado para retornar las propiedades enumerables, y entonces obtenemos un array con el nombre de las keys.

Luego creamos una arrow function cuyo único trabajo es validar la propiedad del objeto mediante el nombre que se le pasa en los parámetros. Aquí es donde entra en acción la coerción: como `trim()` retorna un string sin espacios, `searchEmpty` retornaría dos valores — un string con contenido (que por coerción sería `true`) o un string vacío (que sería `false`) — evitando así los operadores lógicos.

Para el test unitario sólo tendrías que validar dos casos: cuando sea `true` o `false`, sin depender de los valores exactos del objeto, sin "casarlo" con un único objeto similar. Tendrás un código más limpio y más reutilizable.

---

Como este caso hay muchos más que pueden ser reducidos gracias a la coerción de Javascript.
