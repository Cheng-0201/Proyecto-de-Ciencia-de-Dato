# Proyecto de Ciencia de Datos

En este proyecto analizamos los datos del tansporte publico de la region metropolitana de Chile, y los conectamos a datos meteorologicos y de siniestros.

A partir de estos datos tratamos de responder los siguiente preguntas:

1. Como afecta la condicion meteorologica en la cantidad de pasajeros?
2. Como los accidentes de trafico afectan el tiempo de transporte y la necesidad de experiencia al volante
3. Uso de distintos Modelos de Aprendizaje supervisado para la predicción de tiempo de viaje

Los detalles del proyecto estan en [readme notebook](./notebooks/README.md)

Y el tenemos un [notebook completo](./notebooks/Informe_Final.ipynb) que contiene todos los procedimiento que hicimos, desde el principio hasta la final.

## Datos

En este proyecto, usamos:
- Las [Matrices de Viaje](https://www.dtpm.cl/index.php/documentos/matrices-de-viaje) de DTPM.
- Los [dato de siniestros](https://mapas-conaset.opendata.arcgis.com/search) de Metropolitana.
- La [mapa vectorial](https://www.bcn.cl/siit/mapas_vectoriales/index_html) de Chile.
- Los datos meteorologicos de [open-meteo](https://open-meteo.com/).

## Procedimientos

En primer lugar, limpiamos, transformamos y exploramos estos datos en el notebook [EDA](./notebooks/EDA.ipynb), a traves de estos procesos tenemos mas intuicion sobre ellos.

Luego empezamos a responder las preguntas que planteamos al principio.

### 1. Como afecta la conticion meteorologica en la cantidad de pasajeros?

En el notebook [ML_Meteo](./notebooks/ML_Meteo.ipynb) respondemos la primera pregunta.

En este notebook hicimos un modelo para predecir la cantidad de pasajeros usando los datos meteorologicos, nos llegamos un modelo con R2 de 0.36 y RMSE de 144 (i.e. una diferencia de 69% comparando con el promedio de dato real).

Esto significa que el modelo no es tan eficiente, y a partir de esto concluimos que las conticiones meteorologicas que elegimos tienen una afecta en la cantidad no tan alta, y tienen un efecto negativo (i.e. afectan negativmente).


### 2. Como los accidentes de trafico afectan el tiempo de transporte y la necesidad de experiencia al volante

En el notebook [Analisis_riesgo](./notebooks/Analisis_riesgo.ipynb) juntamos la base de datos de transporte ya analizada con registros de accidentes durante el mismo periodo de tiempo en la region metropolitana. Hicimos un modelo que pudera clasificar el nivel de experiencia ideal para el conductor segun la historia de siniestros en las coordenadas cercanas.

Mediante un modelo de "random forest" pudimos identificar los sectores criticos donde se deberian establecer mayores recursos para evitar mas accidentes. Los resultados y analisis se encuentran en el notebook

### 3. Uso de distintos Modelos de Aprendizaje supervisado para la predicción de tiempo de viaje

En el notebook [tiempo_viaje](./notebooks/tiempo_viaje.ipynb) respondemos esta pregunta.

El análisis comparativo evidencia que el problema de predicción de tiempos de viaje es de naturaleza no lineal. La implementación de modelos basados en árboles (Random Forest y Gradient Boosting) superó significativamente a la regresión lineal base, elevando el coeficiente $R^2$ de 0.68 a 0.81 y reduciendo el error promedio (RMSE) en un 26% (122 unidades). Si bien Random Forest presentó el mejor desempeño marginal, el estancamiento de las métricas en torno a 0.81 indica una saturación de la capacidad predictiva de las variables actuales, sugiriendo que para superar este umbral será necesario incorporar nuevas fuentes de datos (como horarios o flujo vehicular en tiempo real).

## Scripts

Hicimos unos scripts para facilitar nuestro trabajo:

1. `reducer.ipynb`: Encarga de procesar el dato orginal a dato crudo y una muestra que vamos a procesar.
2. `datos_por_hora.ipynb`: Encarga de procesar los datos crudos, extraendo los datos necesitamos y que son separados por horas.

Todos estos estan en la carpeta [`/notebooks/script`](/notebooks/script/)

## Librería Externos

En este proyecto usamos las librería externos `matplotlib`, `numpy`, `pandas`, `geopandas`, `requests`, `scikit-learn`, `seaborn`.

Usa `pip install -r requirements.txt` para instalar todos.

Ademas como todos lo que hicimos estan en Jupyter Notebook(`.ipynb`), por lo tanto para poder ejecujar se requiere tambien el libreria `jupyter`

## Quién somos

- [@aaronpisaa](https://github.com/aaronpisaa)
- [@Benjasud-coder](https://github.com/Benjasud-coder)
- [@nicolasmartinezb](https://github.com/nicolasmartinezb)
- [@Cheng-0201](https://github.com/Cheng-0201)

## Licencia

Este repositorio está licenciado bajo Creative Commons CC-BY 4.0.