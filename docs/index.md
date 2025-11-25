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

###### ¿Qué podria salir mal?

1. La muesta que elejimos era muy pequeño en comparacion con el dato real, reduciendo la fiabilidad de nuetros resultados.
2. Las variables independientes que ejegimos estan correlacionadas, puede causar multicolinealidad.

