# Resultados

## Qué cubre esta sección

Esta sección resume los principales resultados del proyecto desde una perspectiva **técnica** y **práctica**.

Los resultados no se limitan a reportar métricas de modelos. Se interpretan en términos de:

* calidad de forecasting en distintos horizontes
* efecto de la información meteorológica
* capacidad para seguir picos críticos de turbidez
* idoneidad para despliegue piloto
* valor operativo para una planta real de tratamiento

Esto es importante porque, en un proyecto de IA industrial aplicada, el valor final de la solución no depende solo del rendimiento en benchmark, sino también de si puede apoyar decisiones útiles en el mundo real.

---

## Marco de evaluación

La validación comparativa se llevó a cabo sobre:

* **cinco arquitecturas neuronales**: MLP, LSTM, GRU, TCN y TFT
* **dos escenarios de datos**:

  * solo datos de planta
  * datos de planta + variables meteorológicas
* **tres horizontes de predicción**:

  * 1 hora
  * 3 horas
  * 6 horas

Las principales métricas cuantitativas utilizadas fueron:

* **MAE** (Mean Absolute Error)
* **RMSE** (Root Mean Squared Error)
* **R²** (coeficiente de determinación)

La interpretación de resultados se complementó con validación visual de las series predichas frente a las reales sobre el conjunto de test.

---

## Resultado global en una frase

El resultado global más claro del proyecto es que **el horizonte de predicción determina fuertemente la idoneidad del modelo**: la predicción a corto plazo puede resolverse con fiabilidad mediante varias arquitecturas, mientras que la predicción proactiva a largo horizonte se beneficia claramente del **Temporal Fusion Transformer (TFT)**.

---

## Resultados cuantitativos por horizonte

## Horizonte de 1 hora

En el **horizonte de predicción de 1 hora**, el rendimiento global fue excelente en prácticamente todas las arquitecturas.

Este es uno de los hallazgos más relevantes del estudio porque muestra que **el comportamiento de la turbidez a corto plazo es altamente predecible** cuando se dispone del historial reciente de planta.

<p align="center">
  <img src="/assets/images/results/mlp-fit-1h.png" alt="Ajuste del MLP en el horizonte de predicción de 1 hora" width="860">
</p>
<p align="center"><em>Figura. Ejemplo de buen ajuste a corto horizonte utilizando el modelo MLP.</em></p>

### Observaciones principales

* Todos los modelos evaluados alcanzaron resultados sólidos en el escenario de corto plazo.
* Los valores de R² estuvieron en general alrededor o por encima de **0,90**, mostrando un ajuste muy bueno.
* El **TFT alcanzó el MAE más bajo** en la configuración enriquecida con meteorología.
* **LSTM, GRU y TFT** ofrecieron un comportamiento a corto plazo muy fuerte.

### Métricas destacadas

Con variables meteorológicas incluidas:

* **TFT** alcanzó un **MAE de 0,1773 NTU**
* **LSTM** alcanzó un **R² de 0,9065**
* **GRU** alcanzó un **R² de 0,9064**
* **TFT** alcanzó un **R² de 0,9050**

### Interpretación

Desde una perspectiva de ingeniería, esto significa que para **conciencia situacional inmediata** existen varias arquitecturas viables.

Por tanto, la elección del modelo en este horizonte puede guiarse no solo por el rendimiento bruto, sino también por:

* simplicidad de implementación
* coste computacional
* facilidad de despliegue
* mantenibilidad

En otras palabras, a 1 hora el proyecto no queda restringido a un único “ganador”.

---

## Horizonte de 3 horas

En el **horizonte de 3 horas**, la tarea de forecasting se vuelve sustancialmente más difícil.

Esto se refleja en la degradación general de las métricas:

* el MAE aumenta
* el RMSE aumenta
* el R² disminuye en comparación con el caso de 1 hora

Esto era esperable, pero aun así es importante porque marca el punto en el que la elección de arquitectura empieza a importar mucho más.

### Observaciones principales

* La diferencia de rendimiento entre modelos se hace más visible.
* **GRU** y **TFT** emergen como las arquitecturas más competitivas en el escenario enriquecido con meteorología.
* La tarea deja de estar dominada solo por la dinámica local reciente; empieza a importar más una representación temporal más potente.

### Métricas destacadas

Con variables meteorológicas incluidas:

* **GRU** alcanzó el mejor **R² = 0,6635**
* **TFT** se mantuvo muy competitivo con **R² = 0,6322**
* **LSTM** también obtuvo un rendimiento fuerte con **R² = 0,6540**

### Interpretación

Este horizonte es especialmente interesante porque se sitúa entre la **conciencia inmediata** y la **predicción genuinamente proactiva**.

En este punto, el estudio sugiere que:

* los baselines simples empiezan a perder fuerza relativa
* los modelos recurrentes y basados en atención ofrecen beneficios más claros
* la contribución de las variables meteorológicas se vuelve más significativa

El horizonte de 3 horas actúa, por tanto, como el punto de inflexión del estudio: es donde el problema deja de comportarse como una tarea sencilla de forecasting a corto plazo y empieza a premiar arquitecturas temporales más ricas.

---

## Horizonte de 6 horas

El **horizonte de 6 horas** es el escenario más exigente y también uno de los más valiosos desde el punto de vista operativo.

Aquí aparece el resultado más importante del proyecto.

<p align="center">
  <img src="/assets/images/results/tft-fit-6h.png" alt="Ajuste del TFT en el horizonte de predicción de 6 horas" width="860">
</p>
<p align="center"><em>Figura. Ejemplo de ajuste a largo horizonte utilizando el modelo TFT.</em></p>

### Observaciones principales

* La mayoría de las arquitecturas sufren una caída fuerte de rendimiento.
* La diferencia entre el mejor modelo y el resto pasa a ser decisiva.
* **TFT supera claramente a todos los demás candidatos**.

### Métricas destacadas

Con variables meteorológicas incluidas:

* **TFT** alcanzó **R² = 0,6314**
* **TFT** alcanzó **MAE = 0,4072 NTU**

En comparación, el resto de modelos se mantuvo en niveles de varianza explicada bastante más bajos, con valores de R² apenas por encima de **0,35** en la mayoría de los casos.

### Interpretación

Esta es una de las conclusiones técnicas más fuertes de todo el proyecto.

Para la **predicción proactiva a largo horizonte**, el problema requiere un modelo capaz de:

* manejar relaciones temporales de mayor alcance
* integrar de forma eficaz información heterogénea
* extraer señales útiles de la meteorología y del contexto de proceso
* mantenerse robusto cuando el objetivo de predicción está varias horas por delante

El TFT es la única arquitectura evaluada que satisface de forma clara esos requisitos en este caso de estudio.

Por eso, se convierte en el modelo más fuerte cuando el objetivo es la **anticipación temprana de eventos de turbidez con varias horas de antelación**.

---

## Efecto de las variables meteorológicas

Una de las hipótesis centrales del proyecto era que añadir información meteorológica mejoraría el rendimiento predictivo.

Los resultados respaldan esa hipótesis.

### Hallazgo principal

Las variables meteorológicas mejoran la configuración de forecasting, y su valor se vuelve más evidente a medida que aumenta el horizonte de predicción.

### Por qué esto tiene sentido

Este comportamiento es coherente con la lógica física y operativa del sistema:

* las señales de planta reflejan lo que ya está ocurriendo localmente
* las variables meteorológicas aportan información sobre lo que puede ocurrir a continuación
* esa capacidad adicional de anticipación se vuelve más útil cuanto antes se espera que el sistema pueda reaccionar

### Implicación práctica

Esto confirma que el proyecto se beneficia de tratar la predicción de turbidez como un **problema de predicción multifuente**, y no como una serie temporal puramente interna de planta.

Esta decisión de diseño es una de las razones por las que el proyecto es más sólido que un ejercicio estrecho de forecasting univariante.

---

## Validación cualitativa

Las métricas numéricas se complementaron con un análisis visual de los valores predichos frente a los reales sobre el conjunto de test.

Esto es especialmente importante en este tipo de problema porque un modelo puede tener un error medio aceptable y, aun así, fallar a la hora de reproducir los eventos que más importan operativamente: los **picos bruscos de turbidez**.

<p align="center">
  <img src="/assets/images/results/shap-tft-6h.png" alt="Importancia de variables SHAP para el TFT en el horizonte de 6 horas" width="650">
</p>
<p align="center"><em>Figura. Vista de importancia de variables para el modelo TFT en el escenario de largo horizonte.</em></p>

### Principales hallazgos cualitativos

* Los modelos más competitivos fueron capaces de seguir con bastante fidelidad la evolución dinámica general de la turbidez.
* **LSTM** y, especialmente, **TFT** mostraron mejor capacidad para seguir tanto el momento temporal como la magnitud de muchos de los picos críticos.
* **MLP**, pese a su simplicidad, funcionó sorprendentemente bien en el horizonte de 1 hora.
* Sin embargo, cuando el horizonte aumentó a 6 horas, arquitecturas más simples como MLP perdieron mucha capacidad para reproducir los picos críticos con precisión suficiente.

### Por qué esto importa

Desde el punto de vista de la operación de planta, este comportamiento cualitativo es tan importante como la tabla de métricas en sí.

En una planta de tratamiento, la predicción más valiosa no es la que minimiza el error medio en periodos tranquilos, sino la que ofrece un aviso útil cuando la turbidez está aumentando bruscamente.

Por eso, el mejor seguimiento de picos por parte de **LSTM** y **TFT** resulta especialmente significativo.

---

## Del mejor modelo de investigación al mejor modelo para piloto

Un resultado importante del proyecto es que el **mejor modelo en términos de rendimiento predictivo máximo** no coincide automáticamente con el **mejor modelo para un primer despliegue**.

### Mejor modelo para forecasting a largo horizonte

Si se considera únicamente la fuerza predictiva, especialmente a 6 horas, la mejor opción es claramente:

* **Temporal Fusion Transformer (TFT)**

### Mejor modelo para un piloto inicial

Para un primer despliegue orientado a piloto, el proyecto identifica una candidata práctica muy sólida:

* **LSTM**

### Por qué LSTM sigue siendo importante

Aunque TFT obtuvo los mejores resultados en horizontes largos, LSTM sigue siendo muy relevante porque ofrece:

* buen rendimiento a corto plazo
* modelado temporal fiable
* menor complejidad de implementación
* mayor facilidad de mantenimiento y depuración
* menor fricción de adopción en una configuración real inicial

Esta es una de las conclusiones de ingeniería más maduras del proyecto.

La recomendación final de despliegue no depende solo de quién gana el benchmark, sino del equilibrio entre:

* rendimiento
* simplicidad
* mantenibilidad
* riesgo de despliegue

---

## Resultado de validación del prototipo

Otro resultado importante del proyecto es que el trabajo no se detiene en el entrenamiento y test offline.

La metodología se llevó lo suficientemente lejos como para validar un **prototipo funcional**, lo que significa que el proyecto logró más que una comparación puramente de laboratorio entre arquitecturas neuronales.

### Qué significa esto en la práctica

El proyecto demostró la viabilidad de un pipeline orientado a piloto capaz de:

* recopilar e integrar datos relevantes
* preprocesarlos de forma consistente
* ejecutar inferencia con el modelo seleccionado
* comparar predicciones con observaciones reales
* exponer las salidas para su monitorización y validación visual

Este es un resultado muy fuerte desde la perspectiva de portfolio porque muestra que el proyecto avanzó desde la **experimentación con modelos** hacia la **preparación para despliegue**.

---

## Significado operativo de los resultados

Los resultados son relevantes no solo porque las métricas sean buenas, sino porque respaldan un caso de uso operativo creíble.

### Beneficios operativos potenciales

Si el sistema de forecasting se utiliza de forma efectiva, puede apoyar:

* una respuesta más temprana ante picos de turbidez
* decisiones de dosificación de coagulante mejor informadas
* una mejor planificación de la filtración
* menor sobrerreacción en periodos de incertidumbre
* una mayor conciencia situacional de los operadores durante perturbaciones impulsadas por lluvias

### Significado estratégico para entornos tipo AMVISA

De forma más amplia, los resultados muestran que la predicción de turbidez basada en IA no solo es técnicamente viable, sino también operativamente significativa dentro del contexto de digitalización de utilities de agua.

Esto refuerza el valor del proyecto tanto como:

* una **solución específica de caso** para la planta de Gorbea
* una **referencia reutilizable** para futuros sistemas predictivos en infraestructuras similares

---

## Ideas principales

Las principales conclusiones que emergen de los resultados son las siguientes:

### 1. La predicción de turbidez a corto plazo es altamente viable

A 1 hora, todas las arquitecturas principales lograron un rendimiento fuerte, lo que confirma que el forecasting a corto plazo ya puede aportar una conciencia operativa útil.

### 2. El horizonte de predicción es el gran diferenciador

A medida que el horizonte aumenta, el problema se vuelve mucho más difícil y la elección de modelo importa mucho más.

### 3. Las variables meteorológicas merece la pena integrarlas

Su contribución se vuelve más valiosa a medida que aumenta la necesidad de anticipación.

### 4. TFT es el modelo más fuerte para predicción proactiva a largo horizonte

Es el ganador más claro cuando el objetivo es predecir con varias horas de antelación.

### 5. LSTM es una candidata muy creíble para piloto

Ofrece un equilibrio excelente entre fiabilidad y practicidad de implementación.

### 6. El proyecto demuestra viabilidad orientada al despliegue

El trabajo valida no solo una idea de forecasting, sino también un camino de prototipo hacia aplicación industrial.

---

## Resumen de resultados

En una sola frase, los resultados muestran que **la predicción de turbidez basada en IA es técnicamente viable y operativamente significativa**, con **varios modelos funcionando bien a corto horizonte, una mejora clara al integrar meteorología y TFT emergiendo como la mejor arquitectura para soporte proactivo a largo horizonte**.

---

## Próximos documentos

Las siguientes secciones recomendadas son:

* [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
* [`lessons-learned.md`](lessons-learned.md)
