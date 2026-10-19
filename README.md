# tfm-eeg-sueno-vigilia

# Clasificación de señales EEG para la identificación de estados dormir/despertar

Código del Trabajo Fin de Máster (TFM) del Máster Universitario en Inteligencia Artificial de la Universidad Internacional de Valencia (VIU).

**Autora:** Sara Mesa Rodríguez
**Director:** Álvaro Sánchez Rocamora
**Curso:** 2025-2026

## Descripción

El trabajo clasifica épocas de 30 segundos de EEG en dos estados, vigilia y sueño. Se comparan tres redes neuronales (CNN-1D, CNN+LSTM y una variante de EEGNet) y dos SVM sobre características calculadas explícitamente (lineal y con núcleo RBF). Se usan los canales Fpz-Cz y Pz-Oz, y la partición de los datos se hace por sujetos.

## Datos

Se usa la base de datos pública *Sleep-EDF Expanded* (subconjunto *Sleep Cassette*, 153 registros de 78 sujetos), disponible en PhysioNet:
https://physionet.org/content/sleep-edfx/

Los datos no se incluyen en este repositorio. Hay que descargarlos y colocarlos en la ruta que indica el primer cuaderno de la carpeta `Preprocesamientos`.

## Estructura del repositorio

| Carpeta | Contenido |
|---|---|
| `Preprocesamientos` | Lectura de los registros, recorte de ±30 minutos alrededor del periodo de sueño, filtrado de 0,5 a 30 Hz, segmentación en épocas, normalización y partición por sujetos. Genera los datos procesados que usan los demás cuadernos. |
| `SVM` | Extracción de las 30 características por época (potencia por bandas, estadísticos y parámetros de Hjorth), SVM lineal (cuaderno 14), SVM con núcleo RBF aproximado con el método de Nyström y pesos de clase (cuaderno 15) y figura de la curva ROC (cuaderno 16). |
| `CNN-1D` | Pruebas con el conjunto reducido (1 y 2 canales, configuraciones V1 y V2) y cuaderno final con el conjunto completo (`11_dataset_completo_cnn1d_v1.ipynb`). |
| `CNN+LSTM` | Pruebas con el conjunto reducido (configuraciones V1 y V2). |
| `EEGNET` | Pruebas con el conjunto reducido y cuaderno final con el conjunto completo (configuración V2). |
| `Señales` | Cuadernos que generan las figuras de señales y los análisis descriptivos de las bandas de frecuencia en vigilia y sueño. |

En cada carpeta, el cuaderno con «completo» en el nombre corresponde al conjunto completo (153 registros). Los demás son las pruebas con el conjunto reducido (20 sujetos). Cada carpeta incluye también las figuras que genera el cuaderno (matriz de confusión, curva ROC y curvas de entrenamiento).

## Orden de ejecución

1. `Preprocesamientos`: genera los datos procesados.
2. `SVM`: extracción de características y entrenamiento de las dos SVM.
3. `CNN-1D`, `CNN+LSTM` y `EEGNET`: entrenamiento y evaluación de las redes.
4. `Señales`: figuras y análisis de patrones.

Los cuadernos usan rutas absolutas del equipo donde se ejecutaron, por lo que hay que adaptarlas antes de ejecutarlos.

## Resultados con el conjunto completo

Conjunto de prueba con 12 sujetos y 44.733 épocas.

| Modelo | *Accuracy* | AUC |
|---|---|---|
| SVM lineal | 0,912 | 0,975 |
| SVM con núcleo RBF | 0,940 | 0,988 |
| CNN-1D (V1) | 0,964 | 0,995 |
| EEGNet (V2) | 0,955 | 0,995 |

## Entorno

Python con MNE (lectura de los registros EEG), TensorFlow/Keras (redes neuronales), scikit-learn (SVM), NumPy y Jupyter. Las versiones exactas se indican en la sección 5.3 de la memoria.

## Reproducibilidad

- La partición por sujetos usa `GroupShuffleSplit` en dos pasos, ambos con `random_state=42`. Da 54 sujetos de entrenamiento, 12 de validación y 12 de prueba.
- Los identificadores de los sujetos son los índices de orden de lectura (0 a 77). Los sujetos de prueba son 0, 4, 5, 12, 30, 31, 34, 35, 40, 54, 59 y 70.
- No se fijó la semilla de inicialización de las redes, por lo que una nueva ejecución puede dar resultados ligeramente distintos a los de la memoria.
- Los tiempos de entrenamiento se midieron en una CPU, sin GPU.

## Memoria del TFM

Trabajo Fin de Máster, Universidad Internacional de Valencia, octubre de 2026.

