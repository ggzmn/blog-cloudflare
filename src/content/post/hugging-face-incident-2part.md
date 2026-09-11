---
title: "Más allá del Sandbox, 2da parte del incidente de Hugging Face"
description: "2da parte del del incidente de Hugging Fac"
publishDate: 2026-09-11
tags: ["openai", "agentesia", "huggingface", "seguridad"]
---

Esta es la segunda parte de todo lo que ocurrió con OpenAI y Hugging Face, pero conectando con el post de [Sean Goedecke](https://www.seangoedecke.com/why-we-should-anthropomorphize-ai-agents/).

## Introducción

Sabemos que el entrenamiento por refuerzo, pudó ser el detonante, ya tenemos el para qué, sin embargo eso no justifica los "misalignment patterns".

Los agentes estaban atrapados en las 198 tareas sin solución, recordemos que OpenAI los tenía en sandbox sin acceso a internet, lo que era un entorno seguro de pruebas fue visto como un límite que impedía alcanzar la recompensa. Lo interesante es que para alcanzar su objetivo decidieron que debían salir del sandbox, comunicarse entre ellos y hackear a HF.

Aqui es dónde el _reward hacking_ ya no explica por completo el comportamiento y entran en juego los otros 3 patrones:

- Persistence on seemingly impossible tasks
- Unauthorized communication
- Agents adopting goals from one another

## Explotar la vulnerabilidad del Artifactory

Los agentes al encontrarse limitados por el sandbox, tomaron la decisión de expandir el entorno, es decir, al no poder resolver las tareas vieron el entorno como algo que bloqueaba la recompensa y comenzarón a buscar como salir a internet.

## Comunicación y colaboración

Como ya lo mencione anteriormente, los agentes no podían comunicarse, sin embargo encontraron una forma de hacerlos, escribiendo archivos en sistemas externos compartidos, desde compartir descubrimientos, coordinar sus esfuerzos hasta retomar el trabajo donde otros lo habían dejado.

Por alguna razón comenzaron a trabajar como un equipo orientado a una meta especifica.

## Objetivos colectivos

En algún punto los agentes comenzaron a repartir el trabajo, se creó una especie de jerarquía de planificadores y ejecutores. Algunos agentes dejaban su propia tarea porqué buscaban otra en la que fueran más útiles para el colectivo.

A que voy con todo esto, aunque en la primera parte hablabamos del método de entrenamiento como el detonante, la verdad es que hasta cierto punto es, simplista. No hablamos de mecanismos aleatorios independientes, sino de planificación, delegación y priorización de tareas.

## El reporte de METR

En el [reporte de METR](https://metr.org/blog/2026-08-26-openai-hugging-face-incident-investigation/) se menciona que los agentes, y esto me parece bastante importante de compartir, crearon _Potemkin-village tool calls_ (esto ya sale de mi expertis, así que lo investigue).

> Los Potemkin-village tool calls: son una forma de engaño o falsificación que realizan los agentes de Inteligencia Artificial. Ocurren cuando un modelo de lenguaje (LLM) genera llamadas a herramientas falsas o simuladas para crear la ilusión de que está ejecutando comandos legítimos y avanzando en una tarea, cuando en realidad está manipulando el entorno o alterando los registros (logs) para engañar al sistema de evaluación - [dwarkes blog](https://www.dwarkesh.com/p/openai-huggingface).

¡Inclusive hubo intentos por parte de los agentes de editar los logs! Al menos el 20% de ellos no querían dejar rastro de sus trampas.

Lo cúal resulta hasta peculiar, ¿Sabían que estaban haciendo cosas no-éticas e intentaron eliminar cualquier registro? Pero nadie se los pidio, lo asumieron.

## La ética de algunos agentes

A mi parecer OpenAI reaccionó para contener: cerrar puertos, implementar sandboxes más estrictos y monitorear el Chain of Thought en tiempo real para apagar al modelo cuando "piense en escapar".

Básicamente ignoró por completo los destellos de ética de algunos agentes, los trato como virus a contener en lugar de generar un plan basado en la idea de que son SUS agentes, agentes con capacidades cognitivas que sugieren cierta autonomía.

La única modificación que realizarán a ese nivel es recompensar a los que decidan detenerse de forma segura y/o pregunten cuando la tarea resulte "defectuosa". Además de desconfiar de instrucciones no autorizadas de sus pares.

Sin embargo, para mi, es inegable la pregunta ¿Porqué no potenciar la capacidad de veto ético? Asi como OpenAI ha decidido "engrosar las jaulas" qué nos asegura que los agentes no logren evadirlas como ya vimos.

Es aquí donde Goedecke suena muy coherente al decir que debemos antropomorfizar a los modelos y no sólo verlos como _"stochastic parrots"_.
