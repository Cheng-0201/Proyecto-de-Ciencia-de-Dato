<h1 align="center">
    Estudio sobre el Transporte Público de la Región Metropolitana de Chile
</h1>

## 1. ETL

## 2. EDA

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
<sub>(fig. 3.1-1, Scatterplot entre la cantidad de pasajeros y los datos meteorológicos)</sub>

</div>

Se puede notar que los puntos están dispersos, parece que se puede trazar una recta con pendiente negativa para el gráfico de la calidad de aire y de la humedad, además una recta con pendiente positiva para la temperatura.

Luego veamos las correlaciones

<div align="center">

![meteo_heat_corr](./img/meteo/meteo_heat_corr.png)
<sub>(fig. 3.1-2, heatmap de correlación entre la cantidad de pasajeros y los datos meteorológicos)</sub>

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

<img alt="Meteo_Influence" src="./img/meteo/meteo_influence.png" width="70%"/>

<sub>(fig. 3.1-3, rectas que representan el afecto de cada variable)</sub>

</div>


Y ahora comparamos la variable dependiente real con la predicha, para verificar el eficaz del modelo, queremos que los puntos estén cerca de identidad (y=x).

<div align="center">

<img alt="Meteo_Influence" src="./img/meteo/meteo_comparation.png" width="50%"/>

<sub>(fig. 3.1-4, rectas que compara la variable dependiente real y la predicha)</sub>

</div>

Se nota que no todos los puntos están cerca de la identidad, hay muchas excepciones. verifica la conclusión de que el modelo falta eficaz.

#### Conclusion

En este modelo, la temperatura, la calidad del aire y la humedad afectan la cantidad de pasajeros. Las tres variables lo hacen de forma negativa, sin embargo la humedad, suponemos tiene un efecto positivo.

Además este modelo no es capaz de predecir la cantidad de pasajeros, implicando que la cantidad no tiene una dependencia fuerte con los datos meteorológicos.

En conclusión, no establece nuestra hipótesis.

###### ¿Qué podría salir mal?

1. La muestra que elegimos era muy pequeña en comparación con el dato real, reduciendo la fiabilidad de nuestros resultados.
2. Las variables independientes que elegimos están correlacionadas, puede causar multicolinealidad.

