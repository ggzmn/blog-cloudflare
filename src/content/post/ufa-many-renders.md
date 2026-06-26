---
title: "Lo que aprendí con useFieldArray de React Hook Form"
description: "A veces no siempre lo que funciona es lo optimo, "
publishDate: "2026-05-29"
tags: ["react", "react-hook-form", "performance", "hooks", "refactor"]
draft: true
---

[INTRO - TU VOZ]

<!-- Contexto: formulario complejo, cuatro tablas dinámicas, useFieldArray. Algo funcionaba pero había un delay visible. No rompía nada, pero molestaba. -->

## El problema

[TU VOZ]

<!-- El custom hook se instanciaba en cada componente por separado. Cada tabla tenía su propio array. Aunque el contenido era el mismo, React lo veía como un valor nuevo en cada render. -->

```javascript
// Así se veía el hook original
const useTableForm = () => {
  const { fields, append, remove } = useFieldArray({ name: "items" });
  return { fields, append, remove };
};
```

<!-- Cada tabla llamaba useTableForm() por separado → instancia propia → array nuevo en cada render → re-renders en cascada en las cuatro tablas -->

## Por qué no era obvio

[TU VOZ]

<!-- No rompía nada. Solo un pequeño delay al modificar valores. Fácil de ignorar, difícil de rastrear. -->

## La solución

[TU VOZ]

<!-- Tres cambios: estado subido al componente padre que wrapeaba las tablas, compartido por contexto, referencia estabilizada con useMemo -->

```javascript
// Así quedó la estructura después del refactor
// [ajusta con lo que recuerdes]
```

## Lo que aprendí

[TU VOZ]

<!-- Un custom hook que se llama múltiples veces no es automáticamente un hook compartido. Cada llamada es una instancia nueva. -->

---

_[pregunta de cierre]_
