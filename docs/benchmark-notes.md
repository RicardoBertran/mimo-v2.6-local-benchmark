# Notas del benchmark

## Propósito

Esta prueba no pretende establecer una clasificación general del modelo. Su objetivo es documentar una ejecución real y reproducible de una tarea larga de generación de código en local: construir una aplicación visual autocontenida y corregirla mediante una segunda pasada del propio modelo.

## Entorno observado

- **Modelo:** Xiaomi MiMo-V2.6-Distill-Qwen-9B
- **Cuantización:** GGUF Q8_0
- **GPU:** RTX 5060 Ti 16 GB
- **Inferencia:** Llama UI / llama.cpp
- **Ventana de contexto:** 122.880 tokens
- **Salida:** sin límite configurado

La configuración exacta se conserva en [`../config/MiMo-5060Ti-CUDA.ini`](../config/MiMo-5060Ti-CUDA.ini). La ruta del modelo es específica de la máquina original y debe adaptarse al repetir la prueba.

## Metodología

1. Se cargó el modelo con la configuración incluida en el repositorio.
2. Se envió el prompt principal completo, sin pedir una respuesta resumida.
3. La respuesta se guardó como un archivo HTML autónomo y se abrió en el navegador.
4. Al detectar fallos de ejecución y comportamiento, se continuó la conversación con el prompt de revisión.
5. Se volvió a guardar la respuesta completa y se comprobó el resultado final en el navegador.
6. La versión funcional resultante se conserva en [`../demo/index.html`](../demo/index.html).

## Resultados registrados

| Métrica | Valor |
|---|---:|
| Prompt tokens | 1929 |
| Prompt processing | 1718.29 tokens/s |
| Output tokens | 11979 |
| Tiempo total de generación | 4 min 47 s |
| Velocidad de generación | 41.63 tokens/s |

No se han añadido estimaciones ni resultados derivados. Estas cifras corresponden a la ejecución mostrada en el short.

## Alcance y límites

Una sola ejecución no mide por sí sola la calidad general, la estabilidad ni el rendimiento medio del modelo. La velocidad puede cambiar con versiones distintas del backend, controladores, sistema operativo, carga de la GPU, parámetros de caché y longitud efectiva del contexto. La evaluación de la calidad también es práctica: que la demo final funcione no implica que la primera salida estuviera libre de errores, y precisamente la corrección en una segunda pasada forma parte del experimento.

## Qué conviene comprobar al replicarlo

- Que el archivo GGUF y su cuantización coinciden.
- Que Flash Attention y la descarga de capas a GPU están realmente activas.
- Que la caché K/V usa el tipo esperado.
- Que no hay un límite de salida distinto de `-1`.
- Que el prompt se envía completo y sin modificaciones.
- Que se registran por separado el procesamiento del prompt y la generación.
- Que la demo se prueba en un navegador moderno, incluidos controles, clic, ratón, pausa, reinicio, colisiones y redimensionado.

