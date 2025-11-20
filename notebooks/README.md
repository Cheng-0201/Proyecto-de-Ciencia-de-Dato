### **Contexto**
El sistema de transporte público Red Metropolitana de Movilidad en Santiago es una red dinámica que debe adaptar su oferta para satisfacer las variaciones extremas de la demanda a lo largo de la semana. Nuestro proyecto se enfocará en analizar críticamente cómo esta adaptación de la oferta se plasma en los datos de la GTFS (General Transit Feed Specification) vigente y cómo impacta en diferentes áreas geográficas de la ciudad.

El desafío de la planificación no solo radica en cubrir la demanda máxima de los días laborales (peak), sino también en asignar recursos de manera eficiente durante los fines de semana, cuando los patrones de viaje cambian drásticamente (viajes recreativos, menor densidad de pasajeros, horarios de servicio más restringidos).

### **Motivación**
Como estudiantes de ciencia de datos, nos motiva aplicar técnicas de Análisis de Datos para evaluar la equidad y eficiencia del sistema de transporte en función del tiempo y el espacio. Específicamente, buscamos:

**1.- Cuantificar la Disparidad Semanal:** Mapear y cuantificar las diferencias en la oferta de transporte programada (frecuencias, cobertura de paradas) entre un día laboral y un fin de semana. Esto es crucial, ya que la diferencia en la calidad del servicio entre semana y fin de semana afecta la movilidad de los trabajadores con horarios no tradicionales y el acceso a servicios básicos.

**2.- Identificar la Vulnerabilidad Geográfica:** Analizar si las comunas periféricas o sectores específicos, que dependen fuertemente del bus, experimentan una reducción de servicio proporcionalmente mayor durante el fin de semana en comparación con las zonas centrales. Este análisis espacial tiene implicancias directas en la planificación urbana.

**3.- Generar decisiones operacionales:** Nuestro objetivo es generar visualizaciones y métricas concisas que puedan ser utilizadas por la DTPM o futuros estudios para optimizar la programación de itinerarios (frecuencias y distribución de flota) en función de la localización y el día, buscando la estabilidad y predictibilidad del servicio.



### **Analisis**

**1.- Etapa de Agregación y Transformación**

Dado que los datos originales estaban estructurados por viaje individual (una fila = un viaje), fue necesario consolidar la información a nivel de paradero de subida (x_subida, y_subida) para calcular el riesgo acumulado.

Agrupamiento Geoespacial: Se agruparon los datos por las coordenadas únicas (x_subida, y_subida).

Métricas Agregadas: Se calculó el conteo_viajes (volumen de tráfico) y la suma total de total_victimas por paradero.

Variables Categóricas: Para tipo_transporte, comuna_subida y time_type, se utilizó la moda (.mode()) para capturar la característica dominante del paradero.

Creación de la Variable Objetivo (Y): Nivel_Conductor_Requerido
Se creó una variable categórica de tres niveles (Novato, Intermedio, Experto) basada en los percentiles de la métrica de riesgo (total_victimas agregada por paradero):

Novato: Riesgo $\leq$ Percentil 70 (Q70)

Intermedio: Riesgo entre Percentil 70 y Percentil 90

Experto: Riesgo $\geq$ Percentil 90 (Zonas de riesgo crítico)

<Esto permite enfocar los conductores experimentados en el 10% de paraderos con mayor posiibildad de accidentes.>

Preparación de Variables Predictoras (X)

Codificación: Las variables categóricas (tipo_transporte, comuna_subida, time_type) fueron transformadas mediante One-Hot Encoding (pd.get_dummies) para ser procesadas por el algoritmo.

Normalización: Las variables numéricas (x_subida, y_subida, conteo_viajes) fueron escaladas utilizando StandardScaler. Esto asegura que las coordenadas y el volumen de tráfico contribuyan de manera equitativa a la distancia y las divisiones del árbol, sin que su magnitud distorsione el modelo.

**2.- Modelamiento y Evaluación del Random Forest**

Algoritmo Elegido: Random Forest Classifier.

La gran utilidad de este modelo radica en su capacidad para prevenir el riesgo antes de que ocurra, respondiendo a la pregunta:

"Si un viaje inicia en el paradero X, ¿debe ser asignado a un conductor Novato, Intermedio o Experto?"

El modelo minimiza el riesgo al asegurar que los conductores más experimentados sean asignados a los puntos con la mayor probabilidad de siniestralidad crítica.

Justificación: Se eligió Random Forest por su alta precisión y su capacidad para manejar la complejidad no lineal de las coordenadas geográficas. Además, este modelo proporciona una métrica de Importancia de Características invaluable para justificar las decisiones de asignación de riesgo.

Configuración: Se usaron 100 estimadores (n_estimators=100) y se aplicó el parámetro class_weight='balanced' para mitigar el desbalance natural de clases (donde 'Novato' es la clase más frecuente, mientras que 'Experto' es minoritaria, pero más importante).


### **Resumen de resultados**
Importancia de las Variables

El modelo reveló claramente qué factores son los impulsores del riesgo. Los resultados mostraron que la geografía es el factor dominante:

Coordenadas (x_subida, y_subida): Son las variables más importantes. Esto valida que la ubicación exacta del paradero es el factor principal para definir el riesgo.

Volumen de Viajes (conteo_viajes): El tráfico en el paradero es el segundo factor más relevante, confirmando que la densidad operacional aumenta la probabilidad de siniestro.

Variables Operacionales: Factores como la hora punta (time_type) y el tipo de transporte también contribuyen al riesgo.

El modelo Random Forest demostró ser efectivo para crear fronteras de riesgo basadas en la geografía. Al lograr una buena tasa de predicción (observada en la Matriz de Confusión), aseguramos que la gran mayoría de los paraderos de riesgo crítico ('Experto') sean correctamente identificados, lo que permite una toma de decisiones focalizada:

Decisión: Asignar conductores con mayor experiencia únicamente a los paraderos clasificados como 'Experto' para mejorar la seguridad operacional.

Este análisis sienta las bases para optimizar la asignación de recursos humanos y reducir los indicadores de siniestralidad.


### **¿Qué podria salir mal?**

Este análisis se basa en los datos historicos de accidentes de trafico, lo que introduce varias limitaciones:

- Primero, existe un sesgo si los datos no incluyen accidentes leves o no reportados, lo que subestima el riesgo real. Se podria pasar por alto lugares criticos en donde se necesite conductores mas experimentados.

- Segundo, el modelo puede crear un riesgo de "sobre-asignación" si los paraderos clasificados como 'Experto' son cubiertos de manera excesiva, desviando recursos de zonas 'Intermedio' que también requieren atención. 

- Tercero, como la geografía (coordenadas) es el factor dominante, el modelo no toma en cuenta cambios en la infraestructura o la operación (desvíos de ruta, nuevos paraderos, etc.), ya que asume que el riesgo en un punto geográfico se mantendrá constante, pudiendo llevar a decisiones subóptimas si el entorno cambia.