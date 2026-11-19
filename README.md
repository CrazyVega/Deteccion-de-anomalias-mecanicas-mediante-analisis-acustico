# Detección de anomalías mecánicas mediante análisis acústico

**Proyecto final – Análisis de señales de audio de motores Brushless DC (BLDC) para mantenimiento predictivo.**

<p align="center">
  <img src="/banner.png" width="100%" >
</p>

<h1 align="center">
  Detección de anomalías mecánicas mediante análisis acústico
</h1>

<p align="center">
  <strong>Santiago Andres Vega Pedraza</strong>
</p>

## Integrantes

- **Integrantes:** Santiago Andrés Vega Pedraza
- **Institución:** Universidad Industrial de Santander
- **Asignatura:** Inteligencia Artificial 1
- **Docente:** Gustavo Garzón

## Objetivo general

Desarrollar un prototipo de diagnóstico acústico que clasifique grabaciones de motores Brushless DC (BLDC) en condición saludable (`Healthy`) o en condiciones asociadas a fallas (`Propeller` y `Bearing`), utilizando extracción de características, análisis estadístico, algoritmos clásicos de aprendizaje automático y redes neuronales profundas. Como complemento, aplicar técnicas de aprendizaje no supervisado para explorar la estructura de los datos y analizar la formación de grupos sin utilizar las etiquetas reales durante el ajuste de los algoritmos de clustering.

## Descripción del proyecto

El proyecto estudia si las características de sonido de motores BLDC pueden ayudar a reconocer distintas condiciones de operación. El flujo de trabajo está organizado en tres entregas: exploración y caracterización acústica, clasificación supervisada y aprendizaje no supervisado.

Se trabaja con tres etiquetas proporcionadas por el dataset:

| Etiqueta | Interpretación dentro del dataset |
|---|---|
| `Healthy` | Motor en condición saludable |
| `Propeller` | Condición asociada a la hélice |
| `Bearing` | Condición asociada al rodamiento |

**Alcance:** se trata de una prueba de concepto con grabaciones de motores BLDC A2212 en condiciones controladas. Los resultados no constituyen por sí solos un diagnóstico industrial, ni una validación sobre motocicletas completas o sobre motores no vistos durante el entrenamiento. En este trabajo, las condiciones anómalas corresponden a las clases conocidas `Propeller` y `Bearing`; no se afirma que el sistema detecte automáticamente cualquier falla desconocida.

## Dataset

- **Nombre:** [Brushless DC Motor sound dataset for PdM](https://data.mendeley.com/datasets/j4yr5fmhv4/1)
- **Repositorio:** Mendeley Data, versión 1.
- **DOI:** [10.17632/j4yr5fmhv4.1](https://doi.org/10.17632/j4yr5fmhv4.1)
- **Contenido:** 43 grabaciones de audio en formato WAV, de aproximadamente 10 segundos cada una.
- **Equipos:** cuatro motores BLDC A2212.
- **Condiciones:** saludable, falla asociada a hélice y falla asociada a rodamiento.
- **Licencia indicada por Mendeley Data:** CC BY 4.0.

En la copia del dataset utilizada y revisada en el notebook se identificaron 43 audios distribuidos así:

| Clase | Número de audios |
|---|---:|
| `Healthy` | 22 |
| `Propeller` | 14 |
| `Bearing` | 7 |
| **Total** | **43** |

El dataset original no se incluye dentro de este repositorio para evitar duplicar los archivos de audio. Descárgalo desde Mendeley Data y coloca el ZIP en `data/00_raw/`. El notebook busca los WAV en esa carpeta y, si no los encuentra, intenta localizar el ZIP o solicitarlo cuando se ejecuta en Google Colab.

## Metodología por entregas

### Entrega 01 – Exploración y caracterización acústica

- Inventario de audios, etiquetas, motores y frecuencias de muestreo.
- Inspección de señales y representación mediante formas de onda.
- Cálculo de media, desviación estándar y RMS.
- Identificación exploratoria de valores atípicos mediante el rango intercuartílico (IQR).
- Comparación de espectrogramas y melspectrogramas.
- Extracción de **166 características acústicas por grabación**, combinando estadísticas de melspectrograma, MFCC y descriptores temporales/espectrales.

### Entrega 02 – Aprendizaje supervisado

- Partición estratificada aproximada de entrenamiento y prueba (80/20).
- Validación cruzada para comparar configuraciones e hiperparámetros.
- Evaluación de Decision Tree, Random Forest y Support Vector Machine (SVM).
- Evaluación de redes neuronales tipo perceptrón multicapa (MLP) con distintas arquitecturas y cantidades de épocas.
- Reporte de métricas como Accuracy, precisión, recall, F1-macro y matrices de confusión.

### Entrega final – Aprendizaje no supervisado

1. Imputación de valores faltantes y estandarización de las características numéricas mediante `StandardScaler`.
2. Reducción de las 166 características a dos componentes con `PCA(n_components=2)`.
3. Aplicación de K-means con `n_clusters=3`, correspondiente al número de condiciones conocidas.
4. Aplicación de DBSCAN, con búsqueda de `eps` y `min_samples` apoyada en curvas de k-distancias y criterios internos de agrupamiento.
5. Visualización de los grupos sobre el plano PCA y uso opcional de t-SNE como representación complementaria.
6. Comparación mediante Silhouette, Davies–Bouldin, Adjusted Rand Index (ARI) y Normalized Mutual Information (NMI), según corresponda.

**Importante:** las etiquetas reales (`Healthy`, `Propeller`, `Bearing`) se excluyen del ajuste de K-means y DBSCAN. Se utilizan después para interpretar y comparar los grupos obtenidos. En DBSCAN, la etiqueta `-1` representa puntos considerados ruido por el algoritmo; no debe interpretarse automáticamente como una nueva falla mecánica.

## Estructura del repositorio

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   ├── Proyecto_Final_Deteccion_Anomalias_Audio.ipynb
│   └── 00_respaldo_segunda_presustentacion.ipynb
├── data/
│   ├── 00_raw/                         # ZIP y audios originales (no se versionan)
│   ├── entrega_01_exploracion/
│   ├── entrega_02_supervisado/
│   └── entrega_final_no_supervisado/
├── reports/
│   ├── figures/
│   │   ├── entrega_01_exploracion/
│   │   ├── entrega_02_supervisado/
│   │   └── entrega_final/
│   └── tables/
├── models/
│   ├── supervisados/
│   └── no_supervisados/
├── docs/
│   └── sustentacion/
└── src/
```

## Requisitos

- Python 3.10 o superior (recomendado).
- Jupyter Notebook o Google Colab.
- Dependencias listadas en `requirements.txt`.
- TensorFlow para ejecutar la sección de Deep Learning (MLP) de la Entrega 02.

## Instalación y ejecución

1. Clona el repositorio:

   ```bash
   git clone <URL_DEL_REPOSITORIO_GITHUB>
   cd <NOMBRE_DEL_REPOSITORIO>
   ```

2. Crea y activa un entorno virtual (recomendado):

   ```bash
   python -m venv .venv
   ```

   En Windows:

   ```bash
   .venv\Scripts\activate
   ```

   En macOS/Linux:

   ```bash
   source .venv/bin/activate
   ```

3. Instala las dependencias:

   ```bash
   pip install -r requirements.txt
   ```

4. Descarga el ZIP del dataset desde [Mendeley Data](https://data.mendeley.com/datasets/j4yr5fmhv4/1) y colócalo en `data/00_raw/`.

5. Abre `notebooks/Proyecto_Final_Deteccion_Anomalias_Audio.ipynb` en Jupyter o Google Colab y ejecuta las celdas en orden.

6. Consulta los CSV generados en `data/` y las visualizaciones en `reports/figures/`, organizadas por entrega.

> La sección MLP requiere TensorFlow. Si no está instalado, el notebook intenta instalarlo automáticamente; también se puede instalar manualmente con `pip install tensorflow`.

## Resultados y reproducibilidad

El notebook guarda matrices procesadas, coordenadas PCA/t-SNE, resultados de clustering, métricas y modelos para facilitar la revisión de cada entrega. Las métricas definitivas deben obtenerse ejecutando el notebook completo con el dataset original. No se deben presentar resultados históricos o de ejecuciones anteriores como si fueran necesariamente los resultados de la última ejecución.

## Limitaciones conocidas

- El dataset contiene únicamente 43 grabaciones y presenta desbalance entre clases.
- La clase `Bearing` está asociada al motor M4 en el conjunto suministrado; el modelo podría aprender particularidades de ese motor además de características asociadas a la condición.
- La evaluación supervisada se realiza por grabación, no mediante una separación independiente por motor.
- La reducción PCA a dos componentes facilita la visualización, pero puede descartar información presente en las 166 características originales.
- La adquisición se realizó en condiciones controladas; la generalización a otros motores, ambientes ruidosos o motocicletas completas requiere validación adicional.

## Fuente del dataset y referencia académica

Prieto Estacio, R. S., Bravo Montenegro, D. A., & Rengifo Rodas, C. F. (2023). *Dataset of audio signals from brushless DC motors for predictive maintenance*. **Data in Brief, 50**, 109569. https://doi.org/10.1016/j.dib.2023.109569

Prieto Estacio, R. S. (2023). *Brushless DC Motor sound dataset for PdM* (Version 1). Mendeley Data. https://doi.org/10.17632/j4yr5fmhv4.1

---

**Proyecto académico.** Completa los datos de integrantes, institución, asignatura y docente antes de publicar el repositorio.
