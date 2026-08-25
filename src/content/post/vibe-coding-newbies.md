---
title: "¿Qué le hace el vibe coding a tu cerebro?"
description: "Qué dice la evidencia sobre cómo la IA cambia tu forma de aprender a programar"
publishDate: 2026-08-24
tags: ["vibe coding", "neurociencia", "reflexión", "opinión"]
---

Llevo varias semanas navegando entre videos de YT del uso de IA, vibe coding, algunos papers, etc. Fue tanta información que me he decidido hacer un post acerca de todas las maravillosas dudas que me encontre (¿sarcasmo?, idk).

Honestamente creo que la ia a "democratizado" la oportunidad de programar a muchas personas, bueno, aquí podría haber un debate, pero ese no es el tema.

Muchas veces cuando veo posts de productos hechos sólo "vibe codeando", la curiosidad técnica e intrusiva me gana y me pregunto: ¿Cómo se conectarán a su backend ? ¿Como registran a sus usuarios? ¿Cómo se autentican? ¿Manejan perfiles de acceso? ¿Cómo despliegan? Me hago esas preguntas más por curiosidad que por otra cosa, porque honestamente la rapidez con la que los agentes programan es algo que sigo procesando.

Este post no pretende ser una fuerza externa qué castigue el uso de los modelos. _I'm just curious._

Primero partamos de un lugar más neutral, todos los usuarios de algún modelo hemos visto esta retroalimentación visual cuando le preguntamos algo, cómo si estuviera ¿pensando?
La retroalimentación visual es necesaria, de hecho es muy normal ver un spinner girando, que se detiene cuando el proceso oculto terminó para mostrarnos una respuesta. En el caso de los modelos, la retroalimentación no es un spinner, visualmente vemos el "chain of thought", cómo si el modelo estuviera "razonando/pensando" pero ¡no lo es!.

Hace poco me encontré con un position paper: [_"Position: Stop Anthropomorphizing Intermediate Tokens as Reasoning/Thinking Traces!"_](https://arxiv.org/pdf/2504.09762) ellos mencionan que el "chain of thought" podría no ser tan inocente, es decir sí un usuario recibe está retroalimentación que parece y le llamán razonamiento termina asumiendo que la respuesta es verdadera. Sin embargo, el modelo puede mostrar un razonamiento y llegar a una conclusión que no es precisamente el resultado correcto o viceversa.

El cerebro es una cosa fascinante.

Quedate con esta idea, vamos ahora ha conectar con más data.

¿Recuerdas que al inicio hablaba de estos chicxs con 0 experiencia en desarrollo pero haciendo vibe coding?

En YT hay un video [What AI does to the mind of novice coders](https://www.youtube.com/watch?v=HTUh0OO6Kmo) basado en un paper del 2024: [The Widening Gap: The Benefits and Harms of Generative AI forNovice Programmers](https://dl.acm.org/doi/10.1145/3632620.3671116)

El tema central era resolver un problema a tŕaves de programación, para eso tenían 35 minutos y acesso a copilot y ChatGPT.
Antes de cualquier comentario, cabe aclarar que el paper tiene sus limitaciones, la principal es su muestreo pequeño de 21 sujetos.

El artículo es claro al citar otros dónde los estudiantes mencionan que la ia les resulta útil y no creen depender demasiado de ella. Pero contrasta un pcco con los resultados.
En estos, los "struggling students" fueron quienes más aceptaban las propuestas de los modelos, y en varios de esos casos se documentó frustración explícita cuando el modelo daba sugerencias incorrectas. Mientras que del otro lado, estudiantes del "acceleration group" no necesariamente aceptaron menos sugerencias pero sabían reconocer con más criterio cuáles eran útiles y cuáles no y presentaban menos dificultades para resolver el problema. Lo peculiar es que uno de los participantes del "acceleratin group" nego usar la IA, pero en las grabaciones encontraron otros resultados, aunque solo un caso dentro de una muestra ya pequeña, me deja la duda de si hemos llegado al punto de no notar nuestro uso de ia.

El paper propone que estás diferencias podría ser resultado de _negative expertise_. Marvin Minsky sugiere que un experto ha visto tantos o más ejemplos de lo que no funciona como de lo que sí funciona, y dice: Para pensar de manera efectiva, debemos "saber" bastante sobre qué no pensar. De lo contrario, obtenemos malas ideas y además, nos toma demasiado tiempo.

Y espera por qué el video de YT lanza una frase, tal vez suena ambigua pero cobra sentido con todo lo que hemos dicho: _no saber que no se sabe algo_ o mejor dicho "ignorancia inconsciente". En este caso, al no tener experiencia previa programando, resulta más dificil averiguar porqué las sugerencias de la IA están fallando, y esto se debe a que los estudiantes no han adquirido el conocimiento necesario para saberlo.

Despúes de leer todo esto, me imagine una idea: tenemos todos estos productos que suenan bien, resuelven un problema y son, algunos, desarrollados por usuarios sin conocimiento previo en programación, quienes confian en las respuestas de los modelos, debido al "chain of thought" pues cada vez que solicitan un cambio, una feature nueva, etc, visualmente reciben una retroalimentación dónde el modelo esta "pensando", para finalmente obtener una respuesta, que tal vez, no funciona, pero que no sabrían como resolver, por su cuenta, debido a su falta de experiencia previa.

Ojo, que las investigaciones, sólo son una parte, fui yo la que los conectó. Pero, ¿suena ilógico?

Ahora, veamos la otra cara de la moneda, igual de interesante. Hubo un comentario que ejemplifica este otro lado, no pude recuperarlo pero recuerdo que era un desarrollador hablando de como la IA le ha permitido aprender más y sin lidiar con comentarios hostiles y hechos desde el ego del otro.

Es decir, una persona con experiencia previa, voy asumir porque támbien ha sido mi experiencia, recurre a los modelos no como una fuente confiable sino como una herramienta para potenciar sus habilidades técnicas.

Ya sé que la experiencia personal no es base para asumir verdades, sin embargo puede sugerirse como ejemplo de lo que el paper menciona en sus conclusiones:

> Nuestros hallazgos sugieren que los estudiantes que ya están bien posicionados para tener éxito pueden aprovechar GenAI para acelerar su aprendizaje, mientras que los estudiantes con dificultades pueden verse obstaculizados por el uso de GenAI, dejándolos con una ilusión de competencia, lo cual puede tener efectos negativos que van mucho más allá de una sola sesión, laboratorio o curso.
>
> [The Widening Gap: The Benefits and Harms of Generative AI forNovice Programmers](https://dl.acm.org/doi/10.1145/3632620.3671116)

Otros papers hablan de la resiliencia en sus participantes, aquellos con mayor resiliencia muestrán un patrón de pensamiento cíclico y reflexivo (planificar, revisar, volver atrás), mientras que el grupo de baja resiliencia seguía un patrón lineal. [Metacognitive strategy patterns in AI-assisted programming: Differences across programming resilience levels](https://www.tandfonline.com/doi/full/10.1080/14703297.2025.2563022)

Y esto último tomenlo como un comentario personal, sólo ESO (he leído acerca de neurociencia porque Nala le tiene miedo a la pirotecnia, así que sólo es algo que vinoa mi mente mientras escribía esto).
La resiliencia es una cualidad que se puede desarrollar, es decir los participantes más resilientes podrían ser aquellos que anteriormente se han expuesto a tareas de programación sin la ayuda de la IA, y no sólo han sido expuestos sino que ha sido dentro de un rango donde tendrían control y han logrado resolver el problema sin exposición prolongada a la frustración.
Por ende aumenta su resilencia gracias a la adopción de técnicas y estrategias para superar un problema. Entonces, que un desarrollador tenga experiencia previa influye ositivamente en como usa la IA.

Pero estos últimos artículos son de paga y ya no pude leerlos, así que mi inferencia es sólo una teoría personal.

No pretendo asumir que el vibe coding es negativo, porque realmente no es lo que este post trata de decidir. Solamente quise compartir todo esto que he estado viendo y leyendo en estas últimas semanas, sin darle un peso moral hacia ningún lado.
