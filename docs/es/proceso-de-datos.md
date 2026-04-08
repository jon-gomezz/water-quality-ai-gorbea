# Pipeline de datos

## Propósito del pipeline de datos

Un sistema predictivo solo es tan fiable como el flujo de datos que hay detrás. En este proyecto, el pipeline de datos se diseñó para transformar registros operativos y ambientales heterogéneos en un dataset unificado adecuado para la **predicción de turbidez**.

El objetivo principal del pipeline no era simplemente recopilar datos, sino hacer que fueran **consistentes, sincronizados, interpretables y preparados tanto para el entrenamiento de modelos como para un futuro uso operativo**.

Esto es especialmente importante en proyectos de IA industrial, donde las mayores dificultades muchas veces no provienen del modelo en sí, sino de la calidad, la estructura y la integración de las fuentes de datos.

---

## Objetivos del pipeline

El pipeline de datos se diseñó para cumplir cinco objetivos principales:

1. **Integrar fuentes heterogéneas** relevantes para el comportamiento de la turbidez
2. **Limpiar y armonizar** registros históricos con estructuras y condiciones de calidad diferentes
3. **Alinear todas las variables en el tiempo** para que puedan utilizarse de forma consistente en tareas de forecasting
4. **Preparar secuencias listas para el modelo** para distintos horizontes de predicción
5. **Preservar la trazabilidad** para que los resultados puedan interpretarse, validarse y monitorizarse después

---

## Principales fuentes de datos

El proyecto combina varios tipos de información porque la turbidez no se explica mediante una única variable.

### 1. Datos operativos SCADA

La primera fuente principal es el **sistema SCADA** de la planta, que proporciona registros históricos del proceso relacionados con el entorno de tratamiento.

Este tipo de datos captura cómo evolucionan la planta y las condiciones de entrada a lo largo del tiempo y aporta el contexto operativo necesario para entender el comportamiento de la turbidez.

Dependiendo de la instrumentación disponible, estos registros pueden incluir:

* medidas relacionadas con la turbidez
* variables de entrada de planta
* indicadores relacionados con la filtración
* variables de operación del proceso
* otras medidas de planta asociadas a las condiciones del tratamiento del agua

Los datos SCADA son esenciales porque reflejan el comportamiento real de la planta en un contexto operativo resuelto temporalmente.

### 2. Datos meteorológicos

La segunda gran fuente es la **información meteorológica**.

Esta fuente es especialmente importante porque las perturbaciones debidas a la lluvia son uno de los principales desencadenantes de los picos repentinos de turbidez.

Las variables meteorológicas pueden aportar información aguas arriba sobre las condiciones ambientales antes de que su impacto completo aparezca en planta, por lo que resultan muy valiosas para una predicción proactiva.

Ejemplos de variables útiles incluyen:

* precipitación
* temperatura
* humedad
* presión atmosférica
* indicadores relacionados con el viento
* otras variables contextuales derivadas del tiempo

### 3. Información contextual de proceso

Además de las medidas directas, el proyecto también considera variables de proceso y de contexto que ayudan a describir el estado del sistema.

Estas variables mejoran la predicción al aportar información sobre cómo está operando la planta cuando se producen cambios ambientales.

---

## Por qué es necesaria la integración de múltiples fuentes

La turbidez es una variable dinámica influida tanto por **factores ambientales externos** como por **condiciones internas del proceso**.

Utilizar solo medidas SCADA ofrecería una visión incompleta porque la lluvia y las perturbaciones ambientales pueden comenzar antes de que su efecto completo aparezca en la entrada de planta.

Usar solo datos meteorológicos también sería insuficiente porque la respuesta de la planta depende de sus propias condiciones hidráulicas y operativas.

El valor del pipeline de datos proviene de combinar ambas dimensiones:

* **los desencadenantes ambientales** del cambio de turbidez
* **el estado operativo** del sistema de tratamiento

Esta combinación es una de las principales razones por las que el proyecto es más robusto que un ejercicio simple de forecasting univariante.

---

## Principales retos del pipeline

<p align="center">
  <img src="/assets/images/data/raw-turbidity-before-outlier-removal.png" alt="Serie bruta de turbidez antes de eliminar outliers" width="860">
</p>
<p align="center"><em>Figura. Ejemplo de la serie bruta de turbidez antes del preprocesado y del tratamiento de outliers.</em></p>

Diseñar el pipeline requirió resolver varios problemas de ingeniería de datos.

### 1. Diferentes resoluciones temporales

Las fuentes de datos disponibles no comparten necesariamente la misma frecuencia de muestreo.

Por ejemplo, las variables SCADA y las series meteorológicas pueden registrarse con distintos intervalos, lo que crea un reto de sincronización para forecasting.

Por ello, se necesita una base temporal común antes de poder utilizar las variables conjuntamente.

### 2. Datos ausentes o incompletos

Los registros industriales reales suelen contener:

* marcas temporales ausentes
* filas incompletas
* interrupciones de sensores
* huecos de comunicación
* fallos parciales de medida

Estos problemas deben tratarse antes del entrenamiento del modelo; de lo contrario, el modelo puede aprender patrones distorsionados o volverse inestable.

### 3. Valores ruidosos y anómalos

Los datasets operativos pueden contener outliers, valores físicamente poco plausibles o registros producidos por problemas de instrumentación y logging en lugar de por un comportamiento real del proceso.

Por eso, la identificación de anomalías es una parte necesaria del pipeline.

### 4. Desalineación entre fuentes

Incluso cuando dos fuentes se registran correctamente, pueden seguir desalineadas en la práctica debido a formatos de timestamp, retardos, convenciones de agregación o ventanas de registro inconsistentes.

### 5. Preparación de secuencias para forecasting

Los modelos de forecasting requieren ventanas temporales estructuradas y no filas aisladas.

Por ello, el pipeline necesita convertir los registros históricos brutos en secuencias que conserven las dependencias temporales y soporten predicción basada en horizontes.

---

## Etapas del pipeline

El pipeline de datos puede entenderse como una secuencia de etapas de ingeniería.

### Etapa 1. Adquisición de datos

La primera etapa consiste en obtener los datos históricos de las fuentes relevantes.

Esto incluye recopilar:

* registros de proceso de planta procedentes del SCADA
* registros meteorológicos de la fuente climática seleccionada
* variables contextuales adicionales cuando estén disponibles

En este punto, la prioridad principal todavía no es el modelado, sino asegurar que la materia prima del proyecto sea suficientemente amplia y representativa de las condiciones operativas de interés.

### Etapa 2. Inspección inicial y revisión de calidad

Antes de cualquier transformación, los datos deben inspeccionarse para entender su estructura y fiabilidad.

Esta revisión incluye comprobar:

* variables disponibles
* unidades y convenciones de nombres
* consistencia de timestamps
* completitud de los registros
* anomalías visibles
* plausibilidad general de las series temporales

Esta etapa es crítica porque muchos problemas posteriores pueden evitarse si los fallos estructurales se detectan pronto.

### Etapa 3. Limpieza y tratamiento de anomalías

Una vez inspeccionados los datos, el siguiente paso es reducir las fuentes de distorsión.

Esta etapa puede implicar:

* eliminar filas inutilizables
* identificar medidas claramente erróneas
* revisar valores sospechosos
* filtrar observaciones imposibles o inconsistentes
* reducir la influencia de artefactos extremos cuando esté justificado

El objetivo no es “embellecer” artificialmente el dataset, sino eliminar registros que no reflejan un comportamiento real del proceso.

### Etapa 4. Armonización temporal

A continuación debe crearse una línea temporal común.

Este es uno de los pasos centrales del proyecto porque la tarea predictiva depende de la relación temporal entre las variables de planta y las condiciones meteorológicas.

La armonización temporal incluye:

* convertir los timestamps a un formato consistente
* alinear las fuentes sobre una rejilla temporal compartida
* decidir cómo agregar o interpolar variables cuando sea necesario
* asegurar que todas las observaciones puedan interpretarse sobre la misma línea temporal

Sin este paso, el forecasting multivariable no sería técnicamente coherente.

### Etapa 5. Integración de fuentes

Después de la armonización, las distintas fuentes se fusionan en un dataset analítico unificado.

Este dataset integrado se convierte en la base para el desarrollo de modelos.

En esta etapa, cada instante temporal contiene una representación sincronizada de:

* estado actual del proceso
* contexto ambiental reciente
* medidas relacionadas con la turbidez
* variables explicativas adicionales

Esta tabla unificada es una de las salidas más importantes del pipeline de datos.

### Etapa 6. Preparación de variables

Una vez que existe el dataset integrado, las variables deben organizarse para forecasting.

Esto incluye preparar el conjunto final de variables y asegurar que el modelo reciba la información en una estructura significativa.

Las tareas típicas en esta etapa incluyen:

* seleccionar las variables más relevantes
* definir claramente las variables objetivo
* preservar el orden temporal
* preparar entradas con retardos o basadas en ventanas cuando sea necesario
* organizar los datos para experimentos multihorizonte

El proyecto no trata esto como un paso puramente automático. La preparación de variables está guiada tanto por conocimiento de dominio como por lógica de forecasting.

### Etapa 7. Preparación de train/validation/test

Antes del entrenamiento de modelos, los datos deben dividirse de una manera que respete la causalidad temporal.

A diferencia de las tareas tabulares aleatorias, los pipelines de forecasting deben evitar fugas de información del futuro hacia el pasado.

Por ello, el dataset se organiza en subconjuntos temporalmente coherentes para:

* entrenamiento
* validación
* prueba

Este paso es esencial para obtener estimaciones realistas del rendimiento.

### Etapa 8. Generación de secuencias para modelos de forecasting

Como el proyecto compara arquitecturas orientadas a secuencias, la etapa final del pipeline convierte el dataset preparado en ventanas temporales listas para el modelo.

Esto significa construir muestras de entrada-salida donde cada predicción se basa en una ventana de contexto histórico y se evalúa frente a un objetivo futuro de turbidez.

Esta etapa de generación de secuencias permite utilizar la misma base de datos unificada en distintas familias de modelos y en múltiples horizontes de predicción.

<p align="center">
  <img src="/assets/images/data/neural-network-data-flow.png" alt="Flujo de datos de red neuronal para el pipeline de forecasting" width="760">
</p>
<p align="center"><em>Figura. Flujo de datos desde las entradas procesadas hasta la predicción de turbidez basada en redes neuronales.</em></p>

---

## Preparación por horizonte de predicción

Una característica importante del proyecto es que el forecasting se evaluó en distintos horizontes:

* **1 hora**
* **3 horas**
* **6 horas**

Esto tiene consecuencias directas sobre el pipeline de datos.

La definición del objetivo, la construcción de secuencias y la lógica de evaluación deben adaptarse al horizonte elegido. Un pipeline preparado para una predicción a 1 hora no es idéntico a uno preparado para una predicción a 6 horas.

Por eso, el pipeline no es un script fijo de preprocesado, sino un flujo configurable capaz de soportar varios escenarios predictivos.

---

## Trazabilidad y reproducibilidad

Otro objetivo importante de diseño del pipeline es la trazabilidad.

En IA industrial es importante saber:

* de dónde procede cada variable
* cómo se alinearon los timestamps
* qué reglas de limpieza se aplicaron
* qué versión del dataset preparado se utilizó
* qué definición de objetivo y horizonte corresponde a cada experimento

Esto es necesario no solo para la reproducibilidad científica, sino también para la confianza operativa. Si más adelante un modelo se despliega o se revisa, el proceso de preparación de datos debe ser comprensible y auditable.

---

## Por qué importa el pipeline de datos en este proyecto

Desde la perspectiva de portfolio, el pipeline de datos es una de las partes técnicas más fuertes del proyecto porque demuestra la capacidad de trabajar con **series temporales reales, imperfectas y procedentes de múltiples fuentes**.

Demuestra habilidades en:

* comprensión de datos industriales
* alineado temporal
* preprocesado consciente de anomalías
* integración de múltiples fuentes
* preparación de secuencias para forecasting
* mentalidad de ingeniería más allá del entrenamiento del modelo

En muchos proyectos de IA aplicada, aquí es donde se crea gran parte del valor técnico real.

---

## Resumen del pipeline

En una sola frase, el pipeline de datos de este proyecto transforma **registros heterogéneos del SCADA y de fuentes meteorológicas** en un **dataset limpio, sincronizado, trazable y preparado para modelos de forecasting** orientado a la predicción de turbidez bajo múltiples horizontes temporales.

---

## Próximos documentos

Las siguientes secciones recomendadas son:

* [`modeling.md`](modeling.md)
* [`system-architecture.md`](system-architecture.md)
* [`results.md`](results.md)
* [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
