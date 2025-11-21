### **Contexto**
El sistema de transporte público Red Metropolitana de Movilidad en Santiago es una red dinámica que debe adaptar su oferta para satisfacer las variaciones extremas de la demanda a lo largo de la semana. Nuestro proyecto se enfocará en analizar críticamente cómo esta adaptación de la oferta se plasma en los datos de la GTFS (General Transit Feed Specification) vigente y cómo impacta en diferentes áreas geográficas de la ciudad.

El desafío de la planificación no solo radica en cubrir la demanda máxima de los días laborales (peak), sino también en asignar recursos de manera eficiente durante los fines de semana, cuando los patrones de viaje cambian drásticamente (viajes recreativos, menor densidad de pasajeros, horarios de servicio más restringidos).

### **Motivación**
Como estudiantes de ciencia de datos, nos motiva aplicar técnicas de Análisis de Datos para evaluar la equidad y eficiencia del sistema de transporte en función del tiempo y el espacio. Específicamente, buscamos:

**1.- Cuantificar la Disparidad Semanal:** Mapear y cuantificar las diferencias en la oferta de transporte programada (frecuencias, cobertura de paradas) entre un día laboral y un fin de semana. Esto es crucial, ya que la diferencia en la calidad del servicio entre semana y fin de semana afecta la movilidad de los trabajadores con horarios no tradicionales y el acceso a servicios básicos.

**2.- Identificar la Vulnerabilidad Geográfica:** Analizar si las comunas periféricas o sectores específicos, que dependen fuertemente del bus, experimentan una reducción de servicio proporcionalmente mayor durante el fin de semana en comparación con las zonas centrales. Este análisis espacial tiene implicancias directas en la planificación urbana.

**3.- Generar decisiones operacionales:** Nuestro objetivo es generar visualizaciones y métricas concisas que puedan ser utilizadas por la DTPM o futuros estudios para optimizar la programación de itinerarios (frecuencias y distribución de flota) en función de la localización y el día, buscando la estabilidad y predictibilidad del servicio.



### **Analisis**

#### 1. Como la condicion meteorologica afecta la cantidad de pasajeros

En el notebook [ML_Meteo](./ML_Meteo.ipynb) hicimos un modelo para predecir la cantidad de pasajeros usando los datos meteorologicos, nos llegamos un modelo con R2 de 0.36 y RMSE de 144 (i.e. una diferencia de 69% comparando con el promedio de dato real).

Esto significa que el modelo no es tan eficiente, y a partir de esto concluimos que las conticiones meteorologicas que elegimos tienen una afecta en la cantidad no tan alta, y tienen un efecto negativo (i.e. afectan negativmente).

#### 2. Como los accidentes de trafico afectan el tiempo de transporte y la necesidad de experiencia al volante

En el notebook [link]() juntamos la base de datos de transporte ya analizada con registros de accidentes durante el mismo periodo de tiempo en la region metropolitana. Hicimos un modelo que pudera clasificar el nivel de experiencia ideal para el conductor segun la historia de siniestros en las coordenadas cercanas.

Mediante un modelo de "random forest" pudimos identificar los sectores criticos donde se deberian establecer mayores recursos para evitar mas accidentes. Los resultados y analisis se encuentran en el notebook


### **¿Qué podria salir mal?**

Este análisis se basa en los datos historicos de accidentes de trafico, lo que introduce varias limitaciones:

- Primero, existe un sesgo si los datos no incluyen accidentes leves o no reportados, lo que subestima el riesgo real. Se podria pasar por alto lugares criticos en donde se necesite conductores mas experimentados.

- Segundo, el modelo puede crear un riesgo de "sobre-asignación" si los paraderos clasificados como 'Experto' son cubiertos de manera excesiva, desviando recursos de zonas 'Intermedio' que también requieren atención. 

- Tercero, como la geografía (coordenadas) es el factor dominante, el modelo no toma en cuenta cambios en la infraestructura o la operación (desvíos de ruta, nuevos paraderos, etc.), ya que asume que el riesgo en un punto geográfico se mantendrá constante, pudiendo llevar a decisiones subóptimas si el entorno cambia.


- La muesta que elejimos era muy pequeno en comparacion con el dato real, reduciendo la fiabilidad de nuetros resultados.
