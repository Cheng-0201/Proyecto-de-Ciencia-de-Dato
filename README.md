# Proyecto de Ciencia de Datos

En este proyecto analizamos los datos del tansporte publico de la region metropolitana de Chile, 

A partir de estos datos tratamos de responder los siguiente preguntas:

1. Como afecta la conticion meteorologica en la cantidad de pasajeros?
2. 222

Los detalles del proyecto estan en [readme notebook](./notebooks/README.md)

## Datos

En este proyecto, usamos:
- Las [Matrices de Viaje](https://www.dtpm.cl/index.php/documentos/matrices-de-viaje) de DTPM.
- La [mapa vectorial](https://www.bcn.cl/siit/mapas_vectoriales/index_html) de Chile.
- Los datos meteorologicos de [open-meteo](https://open-meteo.com/).

## Procedimientos

En primer lugar, limpiamos, transformamos y exploramos estos datos en el notebook [EDA](./notebooks/EDA.ipynb), a traves de estos procesos tenemos mas intuicion sobre ellos.

Luego empezamos a responder las preguntas que planteamos al principio.

### 1. Como afecta la conticion meteorologica en la cantidad de pasajeros?

En el notebook [ML_Meteo](./notebooks/ML_Meteo.ipynb) respondemos la primera pregunta.

En este notebook hicimos un modelo para predecir la cantidad de pasajeros usando los datos meteorologicos, nos llegamos un modelo con R2 de 0.36 y RMSE de 144 (i.e. una diferencia de 69% comparando con el promedio de dato real).

Esto significa que el modelo no es tan eficiente, y a partir de esto concluimos que las conticiones meteorologicas que elegimos tienen una afecta en la cantidad no tan alta, y tienen un efecto negativo (i.e. afectan negativmente).


### 2. 222

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

---

## Entrega Inicial

[Readme de datos](./data/readme.md)

Para facilitar nuetros exploracion, decidimos usar una muestra con 5000 filas de datos.

[notebook](./notebooks/001%20con%20muetra.ipynb)

## Propuesta

La propuesta en [pdf](/propuesta/Propuesta%20Proyecto%20Intro.%20Ciencia%20de%20Datos.pdf)

La muestra de los datos esta en [`/propuesta/data`](/propuesta/data)

`README_proyecto.md --->`

# Proyecto Final – IMT2200: Introducción a la Ciencia de Datos

Este repositorio corresponde al proyecto final del curso IMT2200. Aquí se documentará el desarrollo completo del trabajo grupal, desde la formulación del problema hasta la presentación final.

## Estructura sugerida

```
proyecto/
├── data/                   # Archivos de datos (raw o procesados)
├── notebooks/              # Notebooks con análisis exploratorio y modelamiento
├── src/                    # Código fuente (si se separa del notebook)
├── figures/                # Imágenes y visualizaciones generadas
├── web/                    # Archivos para GitHub Pages
├── README.md               # Descripción general del proyecto
├── requirements.txt        # Dependencias del proyecto (opcional)
```

## Entregables esperados

- `notebooks/analisis_final.ipynb`: Notebook con todo el análisis, visualizaciones y conclusiones.
- `web/index.html`: Página de presentación del proyecto para GitHub Pages.
- Video de presentación (enlace externo o archivo subido).
- Autoevaluación de otros proyectos (entregada por formulario).

## Cómo comenzar

1. Hacer fork de este repositorio o clonarlo.
2. Organizar los archivos según la estructura recomendada.
3. Asegurarse de que el notebook principal se pueda ejecutar de principio a fin sin errores.
4. Publicar la página web del proyecto usando GitHub Pages desde la carpeta `web`.
