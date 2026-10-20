# DLY0100 · Evaluación Parcial N°2: Clasificación de Engagement Grupal con CNN 2D

Este repositorio contiene la solución, experimentos y análisis técnico para la Evaluación Parcial N°2 de la asignatura Deep Learning (DLY0100).

El proyecto aborda el problema de clasificación de engagement grupal en salas de clases a partir de imágenes estáticas (128 × 128 px) extraídas del dataset OUC-CGE.

---

## Resumen del Proyecto

* Objetivo: Diseñar, implementar, entrenar y evaluar una Red Neuronal Convolucional 2D (CNN) propia en TensorFlow/Keras para clasificar el nivel de engagement en 3 categorías (low, mid, high).
* Dataset: 12.000 imágenes RGB provenientes de 4.000 clips de video (3 imágenes por clip).
  * Train: 9.600 imágenes
  * Validation: 1.200 imágenes
  * Test: 1.200 imágenes
* Regla de Split: División estricta por clip (sin mezcla ni redistribución) para evitar la fuga de información entre conjuntos.

---

## Tecnologías y Herramientas Utilizadas

* Lenguaje: Python 3
* Framework Principal: TensorFlow / Keras (API Keras, tf.data, TFRecord)
* Procesamiento de Datos y Métricas: NumPy, Pandas, Scikit-learn
* Visualización: Matplotlib
* Entorno de Ejecución: Google Colab con Aceleración GPU (T4)

---

## Estrategia Experimental

Se evaluaron y compararon 4 configuraciones distintas de arquitecturas e hiperparámetros en el conjunto de Validation:

| Exp | Arquitectura / Modificación | Batch Size | Learning Rate | Val Accuracy | Val Loss |
|:---:|:---|:---:|:---:|:---:|:---:|
| 1 | Base: 3 bloques Conv(32-64-128) + MaxPool, Flatten, Dense(128) | 64 | 0.0010 | 0.9875 | 0.0312 |
| 2 | Exp 1 + BatchNorm + Dropout(0.25 en Conv, 0.5 en Dense) | 64 | 0.0010 | 0.9583 | 0.3132 |
| 3 | Exp 2 + Data Augmentation (flip, rotación, zoom, traslación) | 64 | 0.0010 | 0.6317 | 0.8383 |
| 4 | 4 bloques Conv(32-64-128-256), 2 Conv/bloque, GAP, Dense(64) | 32 | 0.0005 | 0.9492 | 0.1464 |

Selección del Modelo Final: Se seleccionó el Experimento 1 basándose únicamente en el mejor rendimiento en validación (val_loss de 0.0312 y val_accuracy de 98.75%).

---

## Resultados en el Conjunto de TEST

El modelo seleccionado fue evaluado una sola vez sobre el conjunto de evaluación final (TEST):

* Test Accuracy: 98.75%
* Test Loss: 0.0411
* Macro F1-Score: 0.987

### Matriz de Confusión y Hallazgos Principales

* La clase con mejor desempeño fue low (Recall: 0.993), mientras que mid presentó la mayor ambigüedad (Recall: 0.985), confundiéndose principalmente con la clase adyacente high.
* Análisis Cualitativo (10 Imágenes): Se realizó una revisión de 10 ejemplos al
