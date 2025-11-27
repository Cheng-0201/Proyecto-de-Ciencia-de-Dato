<h1 align="center">
    Estudio sobre el Transporte Publico de la Region Metropolitana de Chile
</h1>

## 1. ETL

## 2. EDA

## 3. Responder las preguntas

### 3.1 Como afecta las condiciones meteorologicas en la cantidad de pasajeros.

En este seccion verificar el siguiente hipotesis:

> Las condiciones meteorologicas(la temperatura, la humedad, la calidad de aire) afectan la cantidad de pasajeros.
> Donde la temperatura y la calidad de aire se afectan negativamente, la humedad se afecta positivamente.`  <br/>
> (i.e. A mayor temperatura, a mayor el indicador de la calidad de aire o a menos la humedad, menos es la cantidad de pasajeros.)

Para esta pregunta vamos a usar los siguientes datos:
- la cantidad de pasajeros.
- la temperatura aparente, humedad relativa y calidad de aire(mp2.5)

desde `2025-04-21` a `2025-04-27`, y dividida por hora.

#### Relacion entre los datos

Para tener un intuicion sobre estos datos, hicimos unos scatterplot y un heatmap de correlacion, los cuales indican las relaciones que pueden tener.

En primer lugar, veamos la tendencia de los puntos.

<div align="center">

![meteo_scatter_corr](./img/meteo/meteo_scatter_corr.png)
<sub>(fig. 3.1-1, Scatterplot entre la cantidad de pasajeros y los datos meteorologicos)</sub>

</div>

Se puede notar que los puntos estan dispersos, parece que se puede trazar una recta con pendiente negatica para el grafico de la calidad de aire y de la humedad, ademas una recta con pendiente positiva para la temperatura.

Luego veamos las correlaciones:

<div align="center">

![meteo_heat_corr](./img/meteo/meteo_heat_corr.png)
<sub>(fig. 3.1-2, heatmap de correlacion entre la cantidad de pasajeros y los datos meteorologicos)</sub>

</div>

Como se ve, todo tiene una correlacion menos que 0.5.

#### Modelo

Para verificar nuestro hipotesis, vamos a entrenar un modelo que recibe la condicion meteorologicas y predice la cantidad de pasajeros, luego comparamos el resultado con el dato real, Asi verificando el eficez del modelo, y nuestro hipotesis pues asumimos que tienen una dependencia entre estos datos.

##### Resultado

Y resultamos los siguientes metricas:

> Raíz del Error Cuadrático Medio (RMSE) en datos de Test: 143.50
Mean Absolute Error (MAE) en datos de Test: 108.86
Coeficiente R^2 en datos de Test: 0.36

Estos numeros nos indica que el modelo no es eficaz, es decir capaz de predecir la cantidad de pasajeros.

Vamos a comprementar estas metricas con unos graficos.

En el siguiente grafico podemos verificar el afecto de cada variable, y concluimos que todas estas variables se afecta negativamente a la cantidad de pasajero.

<div align="center">

<img alt="Meteo_Influence" src="./img/meteo/meteo_influence.png" width="70%"/>

<sub>(fig. 3.1-3, rectas que representan el afecto de cada variable)</sub>

</div>

Y ahora comparamos la variable dependiente real con la predicida, para verificar el eficaz del modelo, queremos que los puntos estan cerca de identidad (y=x).

<div align="center">

<img alt="Meteo_Influence" src="./img/meteo/meteo_comparation.png" width="50%"/>

<sub>(fig. 3.1-4, rectas que representan el afecto de cada variable)</sub>

</div>

Se nota que no todos los puntos esta cerca de la identidad, hay muchas excepciones. fuerza la conclusion de que el modelo falta eficez.

#### Conclusion

En este modelo, la temperatura, la calidad del aire y la humedad afectan la cantidad de pasajeros. Las tres variables lo hacen de forma negativa, sin embargo la humedad, suponemos tiene un efecto positivo.

Ademas este modelo no es capaz de predecir la cantidad de pasajeros, implicando que la cantidad no tiene una dependencia fuerte con los datos meteorologicos.

En conclusion, no establece nuestro hipotesis.


### 3.2 ¿Que impacto tienen los accidentes de trafico en el transporte publico?
La hipotesis de este analisis seran las siguientes:

> El tiempo de viajes en transporte pubico se ve aumentado segun la cantidad de accidentes ese mismo dia.
> Es posible definir rutas de transporte publico más peligrosas.`  <br/>
> Un modelo de Machine Learning puede predecir que tipo de conductor seria necesario para los paraderos con más riesgo

Para esta pregunta vamos a usar los siguientes datos:
- Cantidad de viajes por paradero.
- Informe de siniestros de trafico de conaset.

#### Relacion entre los datos

Dado que los datos originales estaban estructurados por viaje individual (una fila = un viaje), fue necesario consolidar la información a nivel de paradero de subida (x_subida, y_subida) para calcular el riesgo acumulado.

Agrupamiento Geoespacial: Se agruparon los datos por las coordenadas únicas (x_subida, y_subida).

Métricas Agregadas: Se calculó el conteo_viajes (volumen de tráfico) y la suma total de total_victimas por paradero.

<img alt="tiempo-Accidentes" src="./img/choques/tiempo-Accidentes.png" width="50%"/>

Preparación de Variables

Las variables categóricas (tipo_transporte, comuna_subida, time_type) fueron transformadas mediante One-Hot Encoding (pd.get_dummies) para ser procesadas por el algoritmo.

Las variables numéricas (x_subida, y_subida, conteo_viajes) fueron escaladas utilizando StandardScaler. Esto asegura que las coordenadas y el volumen de tráfico contribuyan de manera equitativa a la distancia y las divisiones del árbol, sin que su magnitud distorsione el modelo.

#### Modelo

Creación de la Variable Objetivo (Y): Nivel_Conductor_Requerido
Se creó una variable categórica de tres niveles (Novato, Intermedio, Experto) basada en los percentiles de la métrica de riesgo (total_victimas agregada por paradero):

Novato: Riesgo $\leq$ Percentil 70 (Q70)

Intermedio: Riesgo entre Percentil 70 y Percentil 90

Experto: Riesgo $\geq$ Percentil 90 (Zonas de riesgo crítico)

<Esto permite enfocar los conductores experimentados en el 10% de paraderos con mayor posiibildad de accidentes.>

<img alt="Comunas_siniestras" src="./img/choques/Comunas_siniestras.png" width="40%"/>


**Algoritmo Elegido: Random Forest Classifier.**

La gran utilidad de este modelo radica en su capacidad para prevenir el riesgo antes de que ocurra, respondiendo a la pregunta:

##### "Si un viaje inicia en el paradero X, ¿debe ser asignado a un conductor Novato, Intermedio o Experto?"

Se eligió Random Forest por su alta precisión y su capacidad para manejar la complejidad no lineal de las coordenadas geográficas. Además, este modelo proporciona una métrica de Importancia de Características invaluable para justificar las decisiones de asignación de riesgo.

##### Resultado

##### **Importancia de las Variables**

El modelo reveló claramente qué factores son los impulsores del riesgo. Los resultados mostraron que la geografía es el factor dominante:

- <Coordenadas (x_subida, y_subida): Son las variables más importantes. Esto valida que la ubicación exacta del paradero es el factor principal para definir el riesgo.>

- <Volumen de Viajes (conteo_viajes): El tráfico en el paradero es el segundo factor más relevante, confirmando que la densidad operacional aumenta la probabilidad de siniestro.>

- <Variables Operacionales: Factores como la hora punta (time_type) y el tipo de transporte también contribuyen al riesgo.>

  <img alt="variables_modelo" src="./img/choques/variables_modelo.png" width="50%"/>

El modelo Random Forest demostró ser efectivo para crear fronteras de riesgo basadas en la geografía. Al lograr una buena tasa de predicción (observada en la Matriz de Confusión), aseguramos que la gran mayoría de los paraderos de riesgo crítico ('Experto') sean correctamente identificados, lo que permite una toma de decisiones focalizada:

<img alt="conductores" src="./img/choques/conductores.png" width="70%"/>

Decisión: Asignar conductores con mayor experiencia únicamente a los paraderos clasificados como 'Experto' para mejorar la seguridad operacional.

Este análisis sienta las bases para optimizar la asignación de recursos humanos y reducir los indicadores de siniestralidad.

<img alt="predicciones" src="./img/choques/predicciones.png" width="50%"/>

**Pensamientos de predicciones**


<img alt="matriz_confusion" src="./img/choques/matriz_confusion.png" width="40%"/>


###### ¿Qué podria salir mal?

1. La muesta que elejimos era muy pequeño en comparacion con el dato real, reduciendo la fiabilidad de nuetros resultados.
2. Las variables independientes que ejegimos estan correlacionadas, puede causar multicolinealidad.

