---
title: "Vite 6 no respeta tu devDependency si el código dice lo contrario"
description: "Una dependencia dev llegó a producción porque Vite sí la bundleó"
publishDate: 2026-07-28
tags: ["vite", "security", "sonarqube", "dependencies", "bundling", "rollup"]
---

## Instalé una librería de Excel: ¿dependency o devDependency?

Hace ya tiempo necesitaba generar unos reportes descargables en formato Excel. Busqué en npm y descargué la que consideré la mejor alternativa para mis necesidades. El tema es que no me había percatado de que al correr el _npm install_ había puesto la flag para agregarla como devDependency, y esto se convertiría en un aprendizaje más profundo después del PR review...

## Cómo me di cuenta

Mi lider de equipo nos alertó por la dependencia, y me hizo una pregunta, tal vez muy simple: ¿cómo es posible que pasara el pipeline de SonarQube?

## Y SonarQube tampoco lo atrapó

Revisando el _package.json_ noté que había subido la nueva dependencia como devDependency, pero había algo más de lo que no me había percatado: el pipeline tampoco había levantado un warning por esa librería.

## Búsqueda de respuestas

Lo más rápido que se encontre es que el pipeline no revisaba vulnerabilidades en las dependencias dev, y suena muy lógico: si un desarrollador declara aquellas que no se usan más que en el entorno de desarrollo, como _eslint_ para que revisarlas sino llegan al bundle de producción. Pero había otra duda más rara, ¿cómo es posible que Vite generara un bundle funcional productivo con esta nueva librería? Y aquí está lo más interesante.

Vite no le importa si el paquete está en `dependencies` o `devDependencies`. Si el código de producción lo importa, lo bundlea. Punto.

```js
// código de producción
import { exportToExcel } from "librería-de-excel"; // clasificada como devDependency
```

> [Automatic Dependency Discovery](https://v6.vite.dev/guide/dep-pre-bundling.html#automatic-dependency-discovery)
>
> If an existing cache is not found, Vite will crawl your source code and automatically discover dependency imports (i.e. "bare imports" that expect to be resolved from node_modules) and use these found imports as entry points for the pre-bundle. The pre-bundling is performed with _esbuild_ so it's typically very fast.
>
> After the server has already started, if a new dependency import is encountered that isn't already in the cache, Vite will re-run the dep bundling process and reload the page if needed.

El quote anterior justifica lo que ocurre en _mode: development_, pero ¿qué pasa con el bundle de producción? Básicamente algo similar, pero debido a _Rollup_.

La documentación de _Rollup_ sobre [Tree-Shaking](https://rollupjs.org/introduction/#tree-shaking) explica que arma el bundle en base al análisis estático de lo que se importa en el código, no menciona en ningún momento `dependencies` ni `devDependencies` como parte de esa decisión.

Uniendo eso con lo que viví: si el análisis estático solo mira los `import` del código, entonces no importa en qué sección del `package.json` esté declarado el paquetem si el código de producción lo importa, Rollup lo va a incluir en el bundle.

Nota aclaratoria: esto es acerca de la versión 6 de Vite. Para la versión actual, Vite usa _Rolldown_; en una vista rápida a la documentación no encontre que realice lo mismo, de forma explicita. Sin embargo, hay un blog que menciona que tienen el mismo comportamiento: [ver más](https://kettanaito.com/blog/building-a-treeshakable-library-with-rollup).

## Lo que arreglé (y lo que no)

Lo más importante era migrar a otra dependencia que no tuviera la vulnerabilidad, además de que se solicitó la actualización del pipeline para que también escaneara `devDependencies`.

Mientras escribía esto me di cuenta que algo había faltado: no verifiqué el vector real de la vulnerabilidad. Vi "severidad alta" y actué, cambié la librería sin revisar si el CVE específico aplicaba a mi caso de uso. En este caso mi código solo transformaba una respuesta del servidor a `.xlsx` (escritura, no lectura de archivos externos), y varias de las vulnerabilidades reportadas para esa librería estaban documentadas específicamente sobre funciones de lectura. No sé si me afectaba directamente o no.

Cambiar de librería frente a una alerta de severidad alta sin patch disponible fue, probablemente, la decisión correcta de todos modos. Pero fue una decisión tomada por precaución, no por haber confirmado que estaba expuesta.

## Otras soluciones que pude haber tomado

- Lint rule que bloquee que código de producción importe algo de `devDependencies` [ver más](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-extraneous-dependencies.md).

```js
// .eslintrc.js
module.exports = {
  plugins: ["import"],
  rules: {
    "import/no-extraneous-dependencies": [
      "error",
      {
        devDependencies: false,
      },
    ],
  },
};
```

## La lección

`package.json` es una declaración de intención, no una garantía de comportamiento del bundler. La distinción entre `dependencies` y `devDependencies` parece simple hasta que un bundler y un scanner de seguridad la interpretan cada uno a su manera y ninguno te avisa cuando eso pasa.

Lo importante es que esto no es un bug, es el comportamiento normal de Vite. Inclusive uno de los dev de vite lo deja como respuesta en un hilo de github del 2021.

> [Answered by yyx990803 on Jan 29, 2021](https://github.com/vitejs/vite/discussions/1803)
>
> Actually, as of latest beta.51+, it is no longer necessary to care about dep vs. devDep since Vite now automatically discovers dependencies. The docs will be updated soon.
