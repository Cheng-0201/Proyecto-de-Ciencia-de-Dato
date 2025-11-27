<h1 align="center">
    Estudio sobre el Transporte Público de la Región Metropolitana de Chile
</h1>

## 1. ETL

A partir de los datos originales, extraemos los columnas que nos importan que son:

`tipo_transporte`, `tiene_bajada`, `tiempo_subida`, `tiempo_bajada`, `tiempo_etapa`, `comuna_subida`, `comuna_bajada`, `parada_subida`, `parada_bajada`, `dist_ruta_paraderos`, `dist_eucl_paraderos`, `x_subida`,`y_subida`, `x_bajada`, `y_bajada`

Después la limpiamos y transformamos.

Además agregamos unos columnas como:
- `time_range`: si es `early`, `morning`, `afternoon` o `night`
- `time_type`: es una hora de punta o no, si es, es la de mañana o de noche
para facilitar nuestra exploración.

Por otro lado también extraemos datos como la cantidad de pasajeros y tiempo de viaje por horas.

## 2. EDA

En esta sección exploramos las relaciones que pueden tener los datos. 

### ¿Cómo varía el tiempo de viaje respecto la hora (hora normal y hora de punta)?

Nos resulta:
    
|         time_type | tiempo_etapa |
|------------------:|--------------|
|            normal |  1167.508742 |
| rush_time-morning |  1340.028490 |
|   rush_time-night |  1344.335397 |


es muy lógica, pues en las hora puntas hay más transporte.

Aún más, podemos especificar el tipo de transporte:

|         time_type | tipo_transporte | tiempo_etapa |
|------------------:|----------------:|-------------:|
|            normal |             BUS |   902.424514 |
|                   |           METRO |  1381.771305 |
| rush_time-morning |             BUS |  1041.310440 |
|                   |           METRO |  1545.280265 |
|   rush_time-night |             BUS |  1222.069767 |
|                   |           METRO |  1454.527629 |

### ¿Cómo varían la comuna subida y comuna bajada respecto la hora?

<div align="center">

<img alt="subida" src="./img/eda/eda_subida.png" width="60%"/>

<sub>(fig. 2.1, Heatmap de la Cantidad de subida)</sub>

<br />

<img alt="bajada" src="./img/eda/eda_bajada.png" width="60%"/>

<sub>(fig. 2.2, Heatmap de la Cantidad de bajado)</sub>

</div>

Estos mapas nos permiten entender cómo se comporta la demanda de transporte público en Santiago a lo largo de un día. Utilizamos la intensidad del color para representar la cantidad de pasajeros.

Como pueden ver, la conclusión es que la demanda tanto la de subida como la de bajada está fuertemente centralizada en todos los rangos horarios.

### Cuanto tiempo viaja la mayoría de pasajeros?

<div align="center">

<img alt="tiempo_viaje" src="./img/eda/eda_tiempo_viaje.png" width="80%"/>

<sub>(fig. 2.3, Histograma del tiempo de viaje)</sub>

</div>

A través de este gráfico, nos permite identificar la forma, el pico y el sesgo de la duración de todos los viajes. 

La conclusión es que la distribución está fuertemente sesgada a la izquierda, con un pico de frecuencia muy claro. 

Al observar el pico, se evidencia que la gran mayoría de los viajes dura menos de 1,500 segundos (25 minutos), siendo la duración más común aquella alrededor de los 1,000 segundos (aproximadamente 16-17 minutos), y los viajes de más de 4,000 segundos son muy raros.

Incluso podemos comparar estos mismos tiempos y frecuencia entre cada transporte:

<div align="center">

<img alt="tiempo_viaje_tipo" src="./img/eda/eda_tiempo_viaje_tipo.png" width="80%"/>

<sub>(fig. 2.4, Histograma del tiempo de viaje coloreado segun el tipo de transporte)</sub>

</div>

Podemos observar que la frecuencia alta del BUS en el lado izquierdo (viajes cortos) y la distribución más extendida y larga del METRO hacia la derecha, muestran claramente la función de cada uno: el bus maneja el volumen de viajes cortos y locales, mientras que el metro es esencial para los trayectos más largos.

### ¿Cuáles son las horas en que hay más personas?

<div align="center">

<img alt="dist" src="./img/eda/eda_dist.png" width="80%"/>

<sub>(fig. 2.5, Histograma del cantidad de pasajeros segun la hora)</sub>

</div>

Visualizamos la Cantidad de Pasajeros por Hora del Día en Santiago. 

La conclusión que presenta el gráfico es la clara identificación de los períodos pico o de máxima demanda: se observa un aumento significativo de pasajeros durante la mañana (aproximadamente entre las 7 y 9 AM) y un segundo pico más alto durante la tarde (aproximadamente entre las 5 y 7 PM).

Incluso podemos revisar en qué horarios toman más tiempo los viajes:

<div align="center">

<img alt="tiempo_viaje_dist" src="./img/eda/eda_tiempo_viaje_dist.png" width="80%"/>

<sub>(fig. 2.6, Distribucion del tiempo de viaje segun la hora)</sub>

</div>

A través de este gráfico se ve cómo se incrementa o estabiliza la duración típica del viaje (la mediana) durante las horas pico de la mañana y la tarde, y cuánta incertidumbre hay en el tiempo de viaje, representada por la altura de las cajas.

Además, podemos verificar de donde vienen la mayoría de datos de transporte:

<div align="center">

<img alt="cantidad_tipo" src="./img/eda/eda_cantidad_tipo.png" width="70%"/>

<sub>(fig. 2.7, Histograma del cantidad de pasajeros segun tipo de el transporte)</sub>

</div>

Claramente podemos ver que la mayoría de nuestros datos provienen de transportes como metro o bus


## 3. Responder las preguntas

### 3.1 Como afecta las condiciones meteorológicas  en la cantidad de pasajeros.

En este sección verificar el siguiente hipótesis:

> Las condiciones meteorológicas(la temperatura, la humedad, la calidad de aire) afectan la cantidad de pasajeros.
> Donde la temperatura y la calidad de aire se afectan negativamente, la humedad se afecta positivamente.`  <br/>
> (i.e. A mayor temperatura, a mayor el indicador de la calidad de aire o a menos la humedad, menos es la cantidad de pasajeros.)

Para esta pregunta vamos a usar los siguientes datos:
- la cantidad de pasajeros.
- la temperatura aparente, humedad relativa y calidad de aire(mp2.5)

desde `2025-04-21` a `2025-04-27`, y dividida por hora.

#### Relación entre los datos

Para tener un intuición sobre estos datos, hicimos unos scatterplot y un heatmap de correlación, los cuales indican las relaciones que pueden tener.

En primer lugar, veamos la tendencia de los puntos.

<div align="center">

![meteo_scatter_corr](./img/meteo/meteo_scatter_corr.png)
<sub>(fig. 3.1.1, Scatterplot entre la cantidad de pasajeros y los datos meteorológicos)</sub>

</div>

Se puede notar que los puntos están dispersos, parece que se puede trazar una recta con pendiente negativa para el gráfico de la calidad de aire y de la humedad, además una recta con pendiente positiva para la temperatura.

Luego veamos las correlaciones

<div align="center">

![meteo_heat_corr](./img/meteo/meteo_heat_corr.png)
<sub>(fig. 3.1.2, heatmap de correlación entre la cantidad de pasajeros y los datos meteorológicos)</sub>

</div>

Como se ve, todo tiene una correlación menor que 0.5.

#### Modelo

Para verificar nuestra hipótesis, vamos a entrenar un modelo que recibe la condición meteorológicas y predice la cantidad de pasajeros, luego comparamos el resultado con el dato real, así verificando el eficaz del modelo y nuestra hipótesis pues asumimos que tienen una dependencia entre estos datos.

##### Resultado

Y resultamos los siguientes métricas:

> Raíz del Error Cuadrático Medio (RMSE) en datos de Test: 143.50
Mean Absolute Error (MAE) en datos de Test: 108.86
Coeficiente R^2 en datos de Test: 0.36

Estos números nos indican que el modelo no es eficaz, es decir capaz de predecir la cantidad de pasajeros.

Vamos a complementar estas métricas con unos gráficos.

En el siguiente gráfico podemos verificar el efecto de cada variable, y concluimos que todas estas variables afectan negativamente a la cantidad de pasajeros.

<div align="center">

<img alt="meteo_influence" src="./img/meteo/meteo_influence.png" width="70%"/>

<sub>(fig. 3.1.3, rectas que representan el afecto de cada variable)</sub>

</div>


Y ahora comparamos la variable dependiente real con la predicha, para verificar el eficaz del modelo, queremos que los puntos estén cerca de identidad (y=x).

<div align="center">

<img alt="meteo_comparation" src="./img/meteo/meteo_comparation.png" width="50%"/>

<sub>(fig. 3.1.4, rectas que compara la variable dependiente real y la predicha)</sub>

</div>

Se nota que no todos los puntos están cerca de la identidad, hay muchas excepciones. verifica la conclusión de que el modelo falta eficaz.

#### Conclusion

En este modelo, la temperatura, la calidad del aire y la humedad afectan la cantidad de pasajeros. Las tres variables lo hacen de forma negativa, sin embargo la humedad, suponemos tiene un efecto positivo.

Además este modelo no es capaz de predecir la cantidad de pasajeros, implicando que la cantidad no tiene una dependencia fuerte con los datos meteorológicos.

En conclusión, no establece nuestra hipótesis.

###### ¿Qué podría salir mal?

1. La muestra que elegimos era muy pequeña en comparación con el dato real, reduciendo la fiabilidad de nuestros resultados.
2. Las variables independientes que elegimos están correlacionadas, puede causar multicolinealidad.



