# MiMo V2.6 9B — benchmark local en RTX 5060 Ti

Repositorio del short donde pruebo **MiMo V2.6 Distill Qwen 9B** en local, en Q8, sobre una RTX 5060 Ti, generando una demo visual completa y dejando la configuración, los prompts y el resultado final para replicar la prueba.

La prueba consistió en pedir al modelo una aplicación interactiva completa en un único `index.html`, sin frameworks, librerías, imágenes ni otros recursos externos. El resultado es una simulación de partículas de estética futurista/neón con física, interacción, métricas y controles en tiempo real.

## Modelo y entorno

| Elemento | Valor |
|---|---|
| Modelo | Xiaomi MiMo-V2.6-Distill-Qwen-9B |
| Formato | GGUF Q8_0 |
| GPU | NVIDIA GeForce RTX 5060 Ti 16 GB |
| Interfaz / backend | Llama UI / llama.cpp |
| Contexto configurado | 122.880 tokens (120K) |
| Límite de salida | Sin límite (`n-predict = -1`) |

> La ruta del modelo en la configuración es la usada durante la prueba. Cámbiala para que apunte al archivo GGUF de tu equipo.

## Configuración

La configuración completa y reproducible está en [`config/MiMo-5060Ti-CUDA.ini`](config/MiMo-5060Ti-CUDA.ini).

Parámetros principales:

- Contexto: `122880`
- Temperatura: `0.6`
- Top K / Top P: `20` / `0.95`
- Penalización de repetición: `1.05`
- Batch / ubatch: `512` / `128`
- Flash Attention: activado
- Caché K/V: `q4_0`
- Capas en GPU: `999`
- Ajuste automático de memoria: activado, objetivo `512 MiB`
- Caché de prompt y métricas: activadas

## Resultados del benchmark

| Métrica | Resultado |
|---|---:|
| Tokens del prompt | 1929 |
| Procesamiento del prompt | 1718.29 tokens/s |
| Tokens de salida | 11979 |
| Tiempo total de generación | 4 min 47 s |
| Velocidad de generación | 41.63 tokens/s |

Los valores anteriores son los observados en esta ejecución concreta. El rendimiento puede variar según la versión de `llama.cpp`, los controladores, la configuración y el resto del sistema.

## En qué consistió la prueba

El prompt exigía una simulación de partículas en un solo archivo con, entre otros requisitos:

- Canvas a pantalla completa y al menos 150 partículas.
- Velocidad, dirección, masa y tamaño variables.
- Rebote en bordes, colisiones, aceleración, fricción y límite de velocidad.
- Atracción suave hacia el puntero y ondas expansivas al hacer clic.
- Conexiones por proximidad con opacidad dependiente de la distancia.
- Panel de FPS, partículas, velocidad media y conexiones activas.
- Controles en tiempo real y botones de pausa, reinicio y activación de funciones.
- Diseño oscuro, moderno y adaptable.
- JavaScript vanilla y animación con `requestAnimationFrame`.

El prompt completo está en [`prompts/01-benchmark-prompt.md`](prompts/01-benchmark-prompt.md).

## Revisión y corrección

La primera versión generada tuvo fallos de ejecución y comportamiento. En lugar de editarla manualmente, se pidió al propio modelo que revisara de forma exhaustiva la inicialización, el estado, el bucle de simulación, los controles, las conexiones, las ondas, las colisiones y el ratón.

Ese segundo mensaje está en [`prompts/02-review-prompt.md`](prompts/02-review-prompt.md). La versión corregida y definitiva está en [`demo/index.html`](demo/index.html).

## Probar el resultado final

No es necesario instalar nada:

1. Descarga o clona este repositorio.
2. Abre [`demo/index.html`](demo/index.html) en un navegador moderno.
3. Mueve el puntero para atraer partículas y haz clic sobre el canvas para crear una onda expansiva.
4. Modifica los controles del panel para observar cómo cambia la simulación.

También puedes servir el repositorio con cualquier servidor HTTP local, aunque la demo funciona directamente mediante `file://`.

## Cómo replicar el benchmark

1. Descarga el GGUF Q8_0 de `Xiaomi MiMo-V2.6-Distill-Qwen-9B` desde una fuente de confianza.
2. Abre Llama UI con un backend compatible de `llama.cpp`.
3. Importa o reproduce los ajustes de [`config/MiMo-5060Ti-CUDA.ini`](config/MiMo-5060Ti-CUDA.ini).
4. Sustituye el valor de `model` por la ruta real del GGUF en tu equipo.
5. Inicia el modelo y envía sin modificaciones el contenido de [`prompts/01-benchmark-prompt.md`](prompts/01-benchmark-prompt.md).
6. Guarda la respuesta completa como `index.html` y pruébala en el navegador.
7. Si aparecen errores, envía el contenido de [`prompts/02-review-prompt.md`](prompts/02-review-prompt.md) dentro de la misma conversación.
8. Guarda la respuesta corregida y comprueba todas las interacciones.
9. Compara las métricas que muestre tu instalación con las de esta ejecución, sin asumir que serán idénticas.

## Narrativa del short

El vídeo presenta MiMo 9B como un modelo nuevo de Xiaomi y muestra que puede ejecutarse en local, en Q8, sobre una RTX 5060 Ti de 16 GB. Se enseña la velocidad real registrada en Llama UI y cómo el modelo produjo casi 12.000 tokens de código. También se cuenta la parte menos perfecta del experimento: la primera versión falló, se pidió al modelo revisar y corregir su propio trabajo y, tras esa segunda pasada, se muestra el resultado final funcionando. Este repositorio reúne la configuración, ambos prompts y el código definitivo para que cualquiera pueda inspeccionar o repetir la prueba.

## Contenido del repositorio

```text
.
├── README.md
├── LICENSE
├── .gitignore
├── config/
│   └── MiMo-5060Ti-CUDA.ini
├── prompts/
│   ├── 01-benchmark-prompt.md
│   └── 02-review-prompt.md
├── demo/
│   └── index.html
├── media/
│   ├── broll/
│   │   ├── config-9x16.png
│   │   ├── prompt-benchmark-9x16.png
│   │   └── prompt-review-9x16.png
│   └── video/
│       └── .gitkeep
└── docs/
    └── benchmark-notes.md
```

## Licencia

El contenido de este repositorio se publica bajo la licencia [MIT](LICENSE).

