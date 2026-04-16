Análisis del Comportamiento de Clientes ConnectaTel

## Objetivo del Proyecto
Este proyecto tiene como objetivo evaluar el comportamiento de los clientes de ConnectaTel, una empresa de telecomunicaciones en Latinoamérica, utilizando datos registrados hasta el año 2024. El análisis busca construir un perfil estadístico de los clientes, detectar comportamientos atípicos, crear segmentos de clientes, identificar patrones de consumo, diseñar estrategias de retención y sugerir mejoras en los planes ofrecidos.

## Datasets Utilizados
Se utilizaron tres datasets principales:
- **`plans.csv`**: Contiene información sobre los planes actuales (precio, minutos incluidos, GB incluidos, costo por extra).
- **`users_latam.csv`**: Contiene información de los clientes (edad, ciudad, fecha de registro, plan, churn).
- **`usage.csv`**: Detalle del uso real de los servicios (llamadas y mensajes).

## Etapas del Análisis
El análisis se estructuró en las siguientes etapas:

### 1. Carga y Exploración de Datos
- Se cargaron los tres datasets (`plans`, `users`, `usage`) en DataFrames de pandas.
- Se realizó una revisión rápida de las primeras filas (`.head()`) y la estructura general (`.shape`, `.info()`) para familiarizarse con los datos, sus columnas y tipos.

### 2. Identificación de Problemas de Calidad de Datos
- **Revisión de valores nulos**: Se identificaron valores nulos en `users` (columnas `city` y `churn_date`) y en `usage` (columnas `date`, `duration`, `length`). Se determinó que los nulos en `duration` y `length` eran Missing Not At Random (MNAR) debido al tipo de evento (llamada o mensaje).
- **Detección de valores inválidos y sentinels**: Se encontraron sentinels en `users['age']` (-999) y `users['city']` ('?').
- **Revisión y estandarización de fechas**: Se convirtieron las columnas de fecha a formato `datetime` y se detectaron fechas futuras en `users['reg_date']` (año 2026).

### 3. Limpieza Básica de Datos
- Se reemplazaron los sentinels en `age` por la mediana y en `city` por `pd.NA`.
- Se marcaron las fechas futuras en `reg_date` como nulas (`pd.NaT`).
- Se justificó mantener los valores nulos en `duration` y `length` debido a su naturaleza MNAR.

### 4. Summary Statistics de Uso por Usuario
- Se agregaron las columnas `is_text` y `is_call` en el DataFrame `usage`.
- Se construyó una tabla agregada (`usage_agg`) por `user_id` con el número total de mensajes, llamadas y minutos de llamadas.
- Se renombraron las columnas a `cant_mensajes`, `cant_llamadas` y `cant_minutos_llamada`.
- Se combinó `usage_agg` con el DataFrame `users` para crear `user_profile`.
- Se obtuvo un resumen estadístico de las columnas numéricas y la distribución porcentual del tipo de plan.

### 5. Visualización de Distribuciones y Outliers
- Se generaron histogramas para `age`, `cant_mensajes`, `cant_llamadas` y `cant_minutos_llamada`, diferenciando por tipo de plan, para entender la distribución y la forma de los datos.
- Se utilizaron boxplots para identificar visualmente outliers en las mismas columnas.
- Se calcularon los límites de los outliers usando el método IQR, concluyendo que los outliers en `cant_mensajes`, `cant_llamadas` y `cant_minutos_llamada` deben mantenerse ya que representan el uso real de los usuarios.

### 6. Segmentación de Clientes
- **Por Uso**: Se creó la columna `grupo_uso` clasificando a los usuarios en 'Bajo uso', 'Uso medio' y 'Alto uso' basándose en la cantidad de llamadas y mensajes.
- **Por Edad**: Se creó la columna `grupo_edad` clasificando a los usuarios en 'Joven', 'Adulto' y 'Adulto Mayor'.
- Se visualizaron las distribuciones de los nuevos segmentos de clientes.

### 7. Insight Ejecutivo para Stakeholders
Se proporcionó un análisis ejecutivo resumiendo los problemas de datos, los segmentos de clientes identificados, los patrones de uso extremo y recomendaciones para mejorar la oferta de planes.

## Cómo Ejecutar el Notebook
1. Abre este notebook en Google Colab.
2. Asegúrate de que los archivos `plans.csv`, `users_latam.csv` y `usage.csv` estén disponibles en el directorio `/datasets/` o actualiza las rutas de carga según corresponda.
3. Ejecuta todas las celdas en orden para replicar el análisis.

## Guía de Reproducción
- **Entorno**: Google Colab (Python 3.x).
- **Librerías**: `pandas`, `seaborn`, `matplotlib.pyplot`, `numpy`.
- **Pasos**: Seguir la secuencia de celdas del notebook para la carga, limpieza, análisis y visualización de datos.
