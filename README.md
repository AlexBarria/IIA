# IIA ADD

## **Carrera de especialización en Inteligencia Artificial (CEIA)**

## **Universidad de Buenos Aires (UBA)**

En este repositorio se encuentran los trabajos prácticos final relacionado a las materia introducción a inteligencia artificial (IIA) y análisis de datos (ADD)

# Temarios de las materias
**IIA**
>Numpy para AI - Arrays, vistas, manipulación, slicing, indexing, performance y broadcasting.
>
>Datasets sintéticos, PCA y regresión lineal.
>
>Aprendizaje estadístico (Regresión Lineal): esperanza condicional, ECM y máxima verosimilitud.
>
>Aprendizaje estadístico (Regresión Lineal): Bayes y estimadores puntuales (bias y varianza).
>
>Optimización, gradientes (GD, SGD y mini-batch GD), hiperparámetros, regularización (Lasso y Ridge).
>
>Aprendizaje estadístico (regresión logística): clasificación binaria y softmax.
>
>Expectation maximization: K-means.

**ADD**
>**Introducción al análisis de datos y herramientas:** Git, Conda, Numpy y Pandas.
>
>**Conceptos básico de análisis de datos:** Variables aleatorias, función de distribución de probabilidad, función de distribución conjunta y marginal, distribuciones condicionales, esperanza, varianza, funciones generadoras de momentos, estadisticas de órden k y teorema central del límite.
>
>**Variables aleatorias especiales:** Distribuciones.
>
>**Introducción al análisis de datos:** media, varianza muestral, reglas empíricas, estimación de desvío estándar con rango, diagramas de B&W.
>
>**Conceptos de estadística y teoría de la información:** tipos de errores, ensayo unilateral, test de hipótesis con varianza desconocida, valor-p.
>
>**Test estadísticos:** test de independencia de Pearson, test de t-studen de dos muestras y ANOVA.
>
>**Entropía**
>
>**Preparación de datos:** técnicas de imputación univariada y multivariada, codificación de variables categóricas, trasnformación de variables, discratización, outliers, feature scaling, integración con pipelines de SKLearn.
>
>**Selección de features:** coeficientes de correlación de Pearson, Coeficiente de Spearman, criterio de la inforamción mutua, ANOVA, coeficiente de correl,ación de Kendall, métodos embebidos, métodos wrapper.
>
>**Reducción de la dimensionalidad:** PCA y SVD.
>
>**Ingeniería de features y modelos**

# Trabajo final integrador

En este trabajo práctico se realizó un análisis exploratorio de los datos para aplicar
diferentes técnicas aprendidas en las materias de análisis de datos e introducción a
inteligencia artificial. Para trabajar se utilizó un dataset de Kaggle que contiene datos para
realizar predicciones del clima en Australia.

De forma genérica podemos mencionar los siguientes pasos:

><img src="https://github.com/AlexBarria/IIA/blob/main/images/Pipeline.PNG" width="450" height="350">
>
>**Análisis de la distribución de los datos**
>
><img src="https://github.com/AlexBarria/IIA/blob/main/images/histogramas.PNG" width="750" height="400">
>
>**Codificación de variables binarias:** Se cambian los valores Yes : 1 y No: 0.
>
>**Detección de outliers:** se realizó un estudio de las distribuciones de los datos utilizando histogramas, QQ-plots y Box plots. En base a eso se seleccionaron algunas variables y se generó una función para detectar y reemplazar outliers por NaN para luego ser imputados.
>
><img src="https://github.com/AlexBarria/IIA/blob/main/images/outliers.PNG" width="450" height="150">
>
>**Obtención de coordenadas geográficas:** utilizando la biblioteca GeoPy se obtuvieron las coordenadas geográficas de cada ciudad, se generaron columnas Latitude y Longitude.
>
>**Clusterización con KNN:** En base a la información obtenida en el punto anterior se generaron 3 clusters en función de la ubicación geográfica de esos puntos. Al observar los clúster obtenidos se identifica una distribución geográfica acorde a los distintos climas de la región, por lo que se definió realizar una codificación ordinal para las etiquetas de cada clúster. La codificación asigna etiquetas de mayor valor a los climas que a priori se supone presentarán mayores precipitaciones.
>
><img src="https://github.com/AlexBarria/IIA/blob/main/images/mapa_aust.PNG" width="300" height="300">
>
><img src="https://github.com/AlexBarria/IIA/blob/main/images/mapa_aust_2.PNG" width="400" height="300">
>
>**Escalado de las características:** se escalan las variables numéricas utilizando MinMaxScaler. Se decidió hacer este paso acá ya que luego se utilizan métodos de imputación que calculan distancias entre las distintas observaciones y la diferencia de escalas podría afectar el resultado.
>
>**Imputación de datos faltantes:** Primero se calcula la media de cada variable por ciudad, obteniendo una matriz de valores; ciudad vs variable. Esa matriz luego es utilizada para imputar los valores faltantes de la mayoría de las características.
Para las observaciones que aún presentan datos faltantes (ciudades con faltantes por completo en una variable, por ejemplo) se utilizó KNN para imputar esos datos. La imputación por KNN no se realizó sobre todo el dataset si no que se eligieron las columnas con datos faltantes y algunas columnas sin faltantes de variables que podrían estar relacionadas para mejorar el valor provisto por el algoritmo.
>
>**Codificación de las variables del viento:** para codificar estas variables se utilizó codificación circular con senos y cosenos. Se optó por esta forma de codificación ya que el órden las direcciones del viento se podría interpretar de forma circular y comparado con One Hot Encoding el número de columnas generadas en este último método es mucho mayor.
>
>**Creación de nuevas features:** para tratar de eliminar algunas características y encontrar otras que pudiesen aportar mayor información al modelo, se crearon nuevas features operando con las ya existentes. Principalmente se generaron variables de amplitud (variable 3pm - variable 9am) para todas aquellas variables que el dataset presentaba dos mediciones a lo largo del día.
>
>**Selección de features:** Para selección de features se utilizó el siguiente procedimiento: correlación (kendall para clasificación y pearson para regresión), criterio de la información mutua y recursive feature elimination.
>
><img src="https://github.com/AlexBarria/IIA/blob/main/images/correlaciones.PNG" width="600" height="400">
>
>          
>
><img src="https://github.com/AlexBarria/IIA/blob/main/images/corr_mat.PNG" width="600" height="500">
>
>**Normalización de datos:** antes de ingresar al modelo los datos fueron normalizados utilizando QuantileTransformer().

Para entrenar modelos se utilizaron las bibliotecas provistas por sklearn. Para clasificación se utilizaron los modelos de LogisticRegressiony SGDclassifier. Para regresión se utilizó LinearRegressor y luego un transformador polinómico de órden 3 para realizar una regresión polinómica. Se utilizaron algunas herramientas como gridsearch para el ajuste de los hiper parámetros.

