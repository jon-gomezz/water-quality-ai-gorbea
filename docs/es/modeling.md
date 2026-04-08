# Modelado

## Propósito de la fase de modelado

El objetivo de la fase de modelado no era simplemente entrenar un algoritmo de forecasting, sino determinar **qué arquitectura neuronal era la más adecuada para la predicción de turbidez bajo distintos horizontes operativos**.

Esta es una distinción importante. En un entorno industrial, el “mejor modelo” no es solo el que obtiene la mejor métrica de forma aislada, sino el que ofrece el equilibrio más útil entre:

* calidad predictiva
* adecuación al horizonte temporal
* robustez
* complejidad de implementación
* practicidad de despliegue

Por esa razón, la fase de modelado se diseñó como un **estudio comparativo** y no como un experimento de un único modelo.

---

## Objetivo de modelado

El objetivo práctico de modelado del proyecto fue predecir la **turbidez del agua bruta en la entrada de la planta de Gorbea** utilizando datos históricos de la planta y, cuando estaban disponibles, variables meteorológicas.

La comparación se estructuró en torno a tres horizontes de predicción:

* **1 hora**
* **3 horas**
* **6 horas**

Estos horizontes se eligieron porque corresponden a distintas necesidades operativas:

* **1 hora** da soporte a la conciencia situacional inmediata
* **3 horas** da soporte a la anticipación a corto plazo y a la planificación de ajustes
* **6 horas** da soporte a una toma de decisiones más proactiva antes de que el evento se vuelva crítico

---

## Por qué se compararon varias arquitecturas

La predicción de turbidez es un problema de series temporales multivariables con varios retos:

* picos repentinos durante episodios de lluvia
* efectos retardados entre la precipitación y la respuesta de la planta
* relaciones no lineales entre variables
* dependencias temporales tanto de corto plazo como de más largo alcance

Por ello, habría sido demasiado restrictivo asumir que una única familia de modelos sería óptima en todos los casos.

La estrategia de modelado comparó, por tanto, cinco arquitecturas con distintos sesgos inductivos:

* **MLP** como baseline simple feed-forward
* **LSTM** como modelo recurrente con memoria larga
* **GRU** como alternativa recurrente más ligera
* **TCN** como arquitectura convolucional temporal
* **Temporal Fusion Transformer (TFT)** como modelo secuencial basado en atención para razonamiento multihorizonte

Esta comparación hace que el proyecto sea mucho más sólido desde la perspectiva de portfolio, porque demuestra una selección sistemática de modelos en lugar de una elección basada en intuición.

---

## Modelos evaluados

## 1. MLP

El Perceptrón Multicapa se incluyó como una arquitectura de referencia sencilla.

En problemas de series temporales, un MLP no modela de forma intrínseca el orden secuencial. En su lugar, la dinámica temporal debe codificarse mediante entradas retardadas o ventanas deslizantes. Sus principales ventajas son:

* simplicidad
* entrenamiento rápido
* inferencia ligera
* bajo coste de implementación

Esto lo hace útil como baseline práctico y como recordatorio de que no todos los problemas de forecasting requieren una arquitectura muy compleja.

Su principal limitación es que su memoria temporal queda fijada por la ventana de entrada elegida y no modela explícitamente la estructura secuencial como sí lo hacen los modelos recurrentes o basados en atención.

---

## 2. LSTM

La red Long Short-Term Memory se incluyó porque es una de las arquitecturas más consolidadas para modelado secuencial con dependencias de largo alcance.

LSTM es especialmente relevante en este proyecto porque los cambios de turbidez no siempre son inmediatos. La lluvia y las perturbaciones aguas arriba pueden tardar un tiempo en propagarse hasta las condiciones de entrada de planta, y el propio comportamiento de la planta evoluciona a lo largo del tiempo.

Sus principales fortalezas son:

* memoria temporal explícita
* capacidad para capturar efectos retardados
* amplio historial de uso en predicción de calidad del agua
* patrones de implementación relativamente maduros y favorables al despliegue

Su principal inconveniente es una mayor complejidad y un entrenamiento más lento que en alternativas más simples.

---

## 3. GRU

La Gated Recurrent Unit se evaluó como una alternativa recurrente más ligera frente a LSTM.

GRU conserva la idea principal de memoria con compuertas, pero utiliza una estructura interna más sencilla y menos parámetros. Esto puede ser valioso cuando:

* el dataset es más limitado
* importa la eficiencia de entrenamiento
* puede alcanzarse un rendimiento recurrente similar con un modelo más pequeño

En el contexto de este proyecto, GRU resulta especialmente interesante porque ofrece un punto intermedio entre capacidad expresiva y simplicidad computacional.

---

## 4. TCN

La Temporal Convolutional Network se incluyó para probar un modelo secuencial no recurrente basado en:

* convoluciones causales
* convoluciones dilatadas
* bloques residuales

Esta arquitectura resulta atractiva porque puede procesar secuencias en paralelo y construir campos receptivos amplios de forma eficiente. En la práctica, esto puede hacer que el entrenamiento sea más estable y más rápido que en modelos recurrentes, manteniendo al mismo tiempo una buena capacidad de modelado temporal.

Su principal limitación es que su campo temporal efectivo queda determinado por diseño. Si las dependencias relevantes se extienden más allá de ese campo receptivo, el rendimiento puede verse afectado.

---

## 5. Temporal Fusion Transformer (TFT)

El Temporal Fusion Transformer se incluyó como la arquitectura más avanzada del estudio.

TFT combina varios mecanismos especialmente relevantes para este proyecto:

* codificación secuencial recurrente
* procesamiento residual con compuertas
* selección de variables
* mecanismos de atención
* gran adecuación para forecasting multihorizonte

Esto lo hace especialmente atractivo para un problema como la predicción de turbidez, donde distintas variables pueden ser importantes en distintos momentos y donde la predicción proactiva a mayor horizonte tiene especial valor.

Su principal compromiso es una mayor complejidad tanto de implementación como computacional.

---

## Diseño experimental

El estudio de modelado no se limitó a una única configuración de dataset.

Cada arquitectura se evaluó bajo dos escenarios de entrada:

1. **Solo datos de planta**
2. **Datos de planta + variables meteorológicas**

Esta comparación fue importante porque una de las hipótesis centrales del proyecto es que la información meteorológica mejora el rendimiento predictivo, especialmente cuando el objetivo es anticipar eventos de turbidez impulsados por la lluvia antes de que su efecto completo sea visible en planta.

La evaluación se repitió después para cada uno de los tres horizontes de predicción:

* 1 h
* 3 h
* 6 h

Como resultado, la fase de modelado compara arquitecturas bajo:

* distintas suposiciones temporales
* distintos escenarios de disponibilidad de información
* distintos casos de uso operativos

---

## Flujo de trabajo de modelado

La fase de modelado siguió un flujo de trabajo consistente.

### 1. Definición del problema

La variable objetivo se definió como la turbidez futura en el horizonte de predicción seleccionado.

Esto significa que cada modelo aprendió a mapear una ventana multivariable de contexto histórico hacia una estimación futura de turbidez.

### 2. Preparación de secuencias

El dataset limpio y sincronizado procedente del pipeline de datos se convirtió en ventanas temporales listas para el modelo.

Cada muestra de entrada contenía una secuencia pasada de variables explicativas, mientras que la salida correspondía a la turbidez en el horizonte futuro de interés.

### 3. División train / validation / test

El dataset se particionó respetando la causalidad temporal.

Esto es esencial en forecasting: el modelo debe entrenarse con datos del pasado y evaluarse sobre periodos posteriores no vistos, en lugar de sobre registros mezclados aleatoriamente.

### 4. Entrenamiento y ajuste

Cada arquitectura se entrenó bajo la misma definición general del problema, permitiendo una comparación más justa entre familias de modelos.

El estudio se centró en comparar la idoneidad de las arquitecturas y no solo en maximizar un único experimento aislado.

### 5. Evaluación

La evaluación combinó:

* **métricas cuantitativas**
* **inspección visual cualitativa**
* **interpretación práctica desde el punto de vista operativo**

Esta lógica de evaluación más amplia es importante porque un modelo puede tener métricas aceptables y, aun así, no seguir los picos de forma útil.

---

## Criterios de evaluación

Las principales métricas cuantitativas utilizadas en el proyecto fueron:

* **MAE** (Mean Absolute Error)
* **RMSE** (Root Mean Squared Error)
* **R²** (coeficiente de determinación)

Estas métricas aportan información complementaria:

* **MAE** refleja la desviación absoluta media
* **RMSE** penaliza con más fuerza los errores grandes
* **R²** mide cuánta varianza explica el modelo

Esta combinación es adecuada para la predicción de turbidez porque, desde el punto de vista operativo, importan tanto el ajuste global como los grandes errores en los picos.

---

## Principales resultados de modelado por horizonte

## Horizonte de 1 hora

En el **horizonte de 1 hora**, todas las arquitecturas mostraron un rendimiento fuerte.

Esto sugiere que el comportamiento de la turbidez a corto plazo es suficientemente predecible para una amplia variedad de familias de modelos cuando se dispone del pasado reciente. Incluso arquitecturas relativamente simples pueden ofrecer un rendimiento útil en este contexto.

Aun así, el **TFT alcanzó el MAE más bajo** en el escenario de 1 hora, mientras que **LSTM, GRU y TFT** mostraron un rendimiento global muy sólido.

<p align="center">
  <img src="/assets/images/results/mlp-fit-1h.png" alt="Ajuste del MLP sobre el conjunto de test para un horizonte de 1 hora" width="860">
</p>
<p align="center"><em>Figura. Ajuste del MLP sobre el conjunto de test para el horizonte de predicción de 1 hora.</em></p>

Desde una perspectiva de ingeniería, esto significa que para conciencia situacional inmediata la elección de arquitectura es importante, pero todavía no resulta decisiva del mismo modo que pasa a serlo en horizontes más largos.

---

## Horizonte de 3 horas

En el **horizonte de 3 horas**, el problema se vuelve sustancialmente más difícil.

Los errores aumentan y la varianza explicada disminuye en todas las arquitecturas, algo esperable en una tarea de forecasting más exigente.

En este escenario de medio alcance:

* **GRU** alcanzó el mejor R² en la configuración enriquecida con meteorología
* **TFT** se mantuvo muy competitivo y robusto
* la separación entre las arquitecturas más fuertes y las más débiles se hizo más visible que en 1 hora

Esto muestra que la predicción a medio plazo ya empieza a premiar arquitecturas con una mayor capacidad de representación temporal.

---

## Horizonte de 6 horas

En el **horizonte de 6 horas**, la diferencia entre arquitecturas pasa a ser decisiva.

Aquí es donde el **Temporal Fusion Transformer destaca claramente**. Con las variables meteorológicas incluidas, ofreció el mejor comportamiento global y la mayor varianza explicada con un margen amplio.

<p align="center">
  <img src="/assets/images/results/tft-fit-6h.png" alt="Ajuste del TFT sobre el conjunto de test para un horizonte de 6 horas" width="860">
</p>
<p align="center"><em>Figura. Ajuste del TFT sobre el conjunto de test para el horizonte de predicción de 6 horas.</em></p>

Este es uno de los hallazgos técnicos más importantes del proyecto:

* la predicción a corto horizonte puede resolverse bien con varios modelos
* la predicción proactiva a largo horizonte requiere una arquitectura más expresiva
* **TFT se convierte en la opción más adecuada cuando el objetivo es dar soporte proactivo con varias horas de antelación**

Este resultado es especialmente relevante porque el horizonte de 6 horas es uno de los más valiosos desde el punto de vista operativo para la gestión de planta.

---

## Efecto de las variables meteorológicas

Una de las preguntas clave de modelado en el proyecto era si la información meteorológica mejora de forma material la predicción de turbidez.

Los resultados muestran que **incluir variables meteorológicas es beneficioso**, sobre todo a medida que aumenta el horizonte de predicción.

Esto tiene sentido desde la perspectiva del proceso:

* las señales de planta reflejan lo que ya está ocurriendo localmente
* las variables meteorológicas ayudan a anticipar lo que puede ocurrir después
* su contribución se vuelve más importante cuanto antes se espera que el sistema pueda reaccionar

Por eso, la configuración combinada **SCADA + meteorología** es una de las fortalezas definitorias del proyecto.

---

## Puntos cuantitativos destacados

Algunas de las conclusiones cuantitativas más claras del estudio son:

* A **1 hora**, el **TFT** logró el MAE más bajo en el escenario con meteorología.
* A **3 horas**, **GRU** y **TFT** fueron las arquitecturas más competitivas.
* A **6 horas**, **TFT** superó claramente al resto, alcanzando el mejor R² y el MAE más bajo entre los modelos evaluados.

Esto confirma que la idoneidad del modelo es **dependiente del horizonte**, que es una de las conclusiones de ingeniería centrales del proyecto.

---

## Interpretación cualitativa de las predicciones

El proyecto también incluyó análisis visual de las trayectorias predichas frente a las reales.

Esto es especialmente importante en un problema con picos bruscos, porque las métricas globales por sí solas pueden ocultar si el modelo realmente es capaz de:

* seguir el momento temporal de los picos de turbidez
* capturar su magnitud
* reproducir cambios abruptos en lugar de suavizarlos en exceso

<p align="center">
  <img src="/assets/images/results/shap-tft-6h.png" alt="Análisis SHAP del modelo TFT para horizonte de 6 horas" width="650">
</p>
<p align="center"><em>Figura. Importancia de variables basada en SHAP para el modelo TFT en el horizonte de 6 horas.</em></p>

El análisis cualitativo mostró que los modelos más competitivos, especialmente **LSTM** y **TFT**, fueron más capaces de seguir el comportamiento dinámico de la turbidez y de alinearse con muchos de los picos críticos.

Esto importa más que el rendimiento medio por sí solo, porque en una planta real de tratamiento los eventos difíciles son precisamente los que más preocupan a los operadores.

---

## Por qué LSTM siguió siendo importante para despliegue

Aunque el **TFT** ofreció el mejor rendimiento en horizontes largos, el proyecto no reduce la conclusión a “TFT gana en todo”.

Para un **despliegue inicial orientado a piloto**, **LSTM** sigue siendo una opción muy atractiva porque ofrece:

* buen rendimiento a corto plazo
* modelado temporal robusto
* menor complejidad de implementación que TFT
* adopción operativa más sencilla en un contexto de despliegue temprano

Este es un mensaje de portfolio muy importante: la recomendación final de ingeniería no depende solo de la superioridad predictiva, sino también de la madurez de despliegue, la mantenibilidad y la simplicidad operativa.

---

## Conclusiones de modelado

La fase de modelado conduce a varias conclusiones principales.

### 1. El horizonte de predicción es un factor decisivo

La clasificación de arquitecturas cambia a medida que aumenta el horizonte de predicción.

### 2. La complejidad de la arquitectura gana valor en horizontes más largos

Cuando la tarea pasa de conciencia inmediata a anticipación proactiva, la capacidad de modelado temporal se vuelve más importante.

### 3. Las variables meteorológicas mejoran el valor del forecasting

La integración de información meteorológica refuerza la capacidad del modelo para anticipar futuras perturbaciones.

### 4. TFT es el modelo más fuerte para predicción proactiva

Para el caso de largo horizonte con mayor valor operativo, TFT aporta la ventaja más clara.

### 5. LSTM es una candidata práctica muy sólida para despliegue

Para un piloto inicial, LSTM ofrece un compromiso eficaz entre calidad predictiva y simplicidad de ingeniería.

---

## Por qué la fase de modelado es fuerte desde la perspectiva de portfolio

Esta parte del proyecto demuestra la capacidad de:

* plantear el forecasting como un problema de comparación de ingeniería
* evaluar de forma sistemática varias arquitecturas de deep learning
* razonar sobre la idoneidad del modelo según el caso de uso y no solo según métricas brutas
* conectar el comportamiento predictivo con la operación de planta
* distinguir entre el mejor modelo desde la investigación y el mejor modelo desde el despliegue

Eso hace que la fase de modelado sea mucho más sólida que la de un notebook típico de benchmark con un solo modelo.

---

## Resumen del modelado

En una sola frase, la fase de modelado muestra que **el rendimiento en predicción de turbidez depende fuertemente del horizonte de predicción**, con **TFT emergiendo como la mejor arquitectura para la predicción proactiva a largo horizonte** y **LSTM destacando como una opción práctica orientada a piloto**.

---

## Próximos documentos

Las siguientes secciones recomendadas son:

* [`results.md`](results.md)
* [`system-architecture.md`](system-architecture.md)
* [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
