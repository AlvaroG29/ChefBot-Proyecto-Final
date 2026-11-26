# Entrega 2 – CNN propia, aumento de datos y variaciones

**Responsable:** Adan (secciones 2 y 3 del notebook)

> **Instrucciones (borra este bloque antes de entregar):**
> - Reescribe los textos con tus palabras; son una base, no el texto final.
> - Reemplaza cada `[COMPLETAR: ...]` con lo que salió en el notebook. No inventes números.
> - Pega las gráficas del notebook donde se indica.
> - Extensión sugerida: 1 a 1.5 páginas con figuras.

## 1. Arquitectura de la CNN propia

La red recibe imágenes de 128x128x3 y está formada por **bloques convolucionales** repetidos. Cada bloque tiene dos convoluciones de 3x3, cada una con Batch Normalization y activación ReLU, seguidas de MaxPooling 2x2 y Dropout de 0.25. Se usaron 3 bloques con 32, 64 y 128 filtros. Después de los bloques hay un GlobalAveragePooling, una capa densa de 128 neuronas con Dropout de 0.5 y una capa final softmax de 10 neuronas, una por platillo.

**Justificación de las decisiones:**

| Decisión | Justificación |
|---|---|
| Dos convoluciones 3x3 por bloque | Cubren la misma zona que un filtro 5x5 con menos parámetros. |
| Más filtros en cada bloque (32 a 128) | Primero se aprenden bordes y colores simples; después, formas más complejas. |
| Batch Normalization | Estabiliza y acelera el entrenamiento; regulariza ligeramente. |
| MaxPooling 2x2 | Reduce el tamaño y el costo; da tolerancia a pequeños desplazamientos. |
| Dropout 0.25 / 0.5 | Combate el sobreajuste; es más fuerte donde hay más parámetros. |
| GlobalAveragePooling | Reduce los parámetros frente a Flatten, importante con ~7,000 imágenes. |

Parámetros totales de la red base: `[COMPLETAR: lo que imprime model.summary()]`.

## 2. Entrenamiento

Se usó el optimizador Adam (tasa de aprendizaje 1e-3), pérdida de entropía cruzada categórica dispersa, lotes de 64 imágenes y hasta 40 épocas. Se usaron dos callbacks:

- **Early stopping:** detiene el entrenamiento si la pérdida de validación no mejora en 8 épocas y recupera los mejores pesos.
- **Reducción de la tasa de aprendizaje:** la reduce a la mitad si la pérdida de validación se estanca durante 3 épocas.

El modelo base entrenó `[COMPLETAR]` épocas en `[COMPLETAR]` segundos.

**[PEGAR AQUÍ: gráfica de curvas de pérdida y precisión del modelo base]**

**Diagnóstico de sobreajuste:** `[COMPLETAR: ¿en qué época dejó de mejorar la validación? ¿hay brecha entre entrenamiento y validación? ¿se redujo la tasa de aprendizaje y cuándo?]`

## 3. Aumento de datos

Se aplicaron transformaciones aleatorias solo durante el entrenamiento: espejo horizontal, giro de hasta ~18°, zoom del 15 %, desplazamiento del 10 % y cambio de contraste del 20 %. Esto genera variaciones distintas de cada foto en cada época, lo que ayuda a que la red no memorice las imágenes.

**[PEGAR AQUÍ: imagen de ejemplo de fotos aumentadas (sección 2.2 del notebook)]**

**Efecto medido:** `[COMPLETAR: compara "Base" con "Sin aumento de datos": accuracy y F1 de validación, y la brecha entre entrenamiento y validación. ¿Redujo el sobreajuste?]`

## 4. Variaciones y resultados

**[PEGAR AQUÍ: tabla de experimentos de la sección 3 del notebook]**

| Experimento | Parámetros | Épocas | Acc. val | F1 val | Brecha |
|---|---|---|---|---|---|
| Base | `[ ]` | `[ ]` | `[ ]` | `[ ]` | `[ ]` |
| Sin aumento de datos | `[ ]` | `[ ]` | `[ ]` | `[ ]` | `[ ]` |
| A: más profunda (4 bloques) | `[ ]` | `[ ]` | `[ ]` | `[ ]` | `[ ]` |
| B: tasa de aprendizaje 3e-4 | `[ ]` | `[ ]` | `[ ]` | `[ ]` | `[ ]` |

**Análisis:**
- Profundidad: `[COMPLETAR: ¿mejoró con un bloque más o solo aumentó el tiempo y el sobreajuste?]`
- Tasa de aprendizaje: `[COMPLETAR: ¿converge mejor o solo más lento?]`
- Mejor CNN propia: `[COMPLETAR: nombre y por qué]`

## 5. Mis tareas en esta entrega (para la tabla del informe)

`[COMPLETAR: por ejemplo: diseñé la arquitectura, entrené el modelo base, ejecuté las 3 variaciones y analicé las curvas.]`
