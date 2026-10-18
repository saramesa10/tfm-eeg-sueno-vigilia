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

Los datos no se incluyen en este repositorio. Hay que descargarlos y colocarlos en la ruta que indica el primer cuaderno.

## Contenido y orden de ejecución

Los cuadernos se ejecutan en este orden:

| Cuaderno | Descripción |
|---|---|
| `01_preprocesamiento.ipynb` | Lectura de los registros, recorte, filtrado de 0,5 a 30 Hz, segmentación en épocas y partición por sujetos. Genera `datos_procesados_completo_2canales.npz`. |
| `02_caracteristicas.ipynb` | Cálculo de las 30 características por época (potencia por bandas, estadísticos y parámetros de Hjorth). |
| `03_svm_lineal.ipynb` | SVM lineal. |
| `04_svm_rbf.ipynb` | SVM con núcleo RBF aproximado con el método de Nyström. |
| `05_cnn1d.ipynb` | CNN-1D. |
| `06_cnn_lstm.ipynb` | CNN+LSTM (conjunto reducido). |
| `07_eegnet.ipynb` | EEGNet simplificada. |
| `08_patrones_eeg.ipynb` | Análisis descriptivo de las bandas de frecuencia en vigilia y sueño. |

El archivo `sujetos_por_particion.json` contiene los identificadores de los sujetos de entrenamiento, validación y prueba, para reproducir la misma partición.

## Instalación

Se recomienda un entorno de Python con las versiones de `requirements.txt`:

```
pip install -r requirements.txt
```

## Reproducibilidad

- La partición por sujetos usa `GroupShuffleSplit` con `random_state=42`.
- No se fijó la semilla de inicialización de las redes, por lo que una nueva ejecución puede dar resultados ligeramente distintos a los de la memoria.
- Los tiempos de entrenamiento se midieron en una CPU, sin GPU.

## Memoria del TFM

La memoria completa describe la metodología, los resultados y las conclusiones. [Enlace o referencia, si se publica.]
