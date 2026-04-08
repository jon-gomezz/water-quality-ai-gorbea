# Resumen del proyecto

## Resumen ejecutivo

Este proyecto presenta una **metodología basada en IA para predecir la turbidez del agua bruta** en un contexto real de tratamiento de agua potable. El trabajo se desarrolló en torno a la **planta potabilizadora de Gorbea**, operada por **AMVISA (Aguas Municipales de Vitoria-Gasteiz)**, donde pueden producirse aumentos repentinos de turbidez en muy poco tiempo durante episodios de lluvias intensas.

El proyecto va más allá de construir un único modelo predictivo. Su principal aportación es el diseño y la validación de una **metodología sistemática y reutilizable** para desarrollar, evaluar y preparar el despliegue de soluciones de IA para la predicción de la calidad del agua en entornos industriales.

La metodología se aplicó a un caso de estudio real integrando **datos históricos de planta procedentes del SCADA** con **variables meteorológicas**, comparando múltiples arquitecturas de deep learning y validando un prototipo funcional con vistas a un futuro despliegue piloto.

<p align="center">
  <img src="/assets/images/overview/aerial-infrastructure.png" alt="Vista aérea de la infraestructura de la planta de Gorbea" width="700">
</p>
<p align="center"><em>Figura. Vista general del entorno físico del caso de estudio de Gorbea.</em></p>

---

## Por qué era necesario este proyecto

La turbidez es una de las variables operativas más sensibles en el tratamiento de agua potable.

En la planta de Gorbea, la turbidez del agua bruta puede pasar rápidamente de valores bajos a picos severos durante eventos de tormenta. Estos episodios generan consecuencias operativas directas:

* mayor consumo de coagulante
* mayor frecuencia de lavado de filtros
* menor estabilidad del proceso
* menor tiempo de reacción para los operadores
* mayor riesgo de incumplimiento si el evento no se gestiona adecuadamente

En la práctica, esto significa que una estrategia puramente reactiva muchas veces no es suficiente. Los operadores obtienen mucho más valor de un sistema capaz de **anticipar el evento con antelación**, para poder adaptar la dosificación, la filtración y las decisiones operativas antes de que la calidad del agua se deteriore.

---

## Objetivo principal del proyecto

El objetivo principal fue crear un **marco de ingeniería replicable** para la predicción de calidad del agua basada en IA y demostrar su valor práctico a través de un caso industrial real.

En este proyecto, eso significó:

* definir un flujo metodológico completo
* aplicarlo a la predicción de turbidez en la planta de Gorbea
* identificar el modelo más adecuado para distintos horizontes de predicción
* validar la solución como un prototipo realista para futuros usos industriales

Por tanto, el proyecto tiene un doble propósito:

1. resolver un problema predictivo relevante en una planta real
2. aportar una metodología reutilizable en futuras iniciativas de digitalización

---

## Alcance del trabajo

El proyecto se estructuró como un caso de estudio de ingeniería, no como un ejercicio académico de modelado puramente teórico.

Incluyó:

* definición de objetivos y requisitos operativos
* identificación de variables relevantes para medir e integrar
* combinación de datos de proceso SCADA con información meteorológica
* preprocesado y sincronización de fuentes de datos heterogéneas
* estudio comparativo de múltiples arquitecturas de redes neuronales
* evaluación en varios horizontes de predicción
* validación de un prototipo funcional
* consideraciones de diseño para despliegue piloto, operación y monitorización

El proyecto estuvo, por tanto, orientado a la **preparación para despliegue**, aunque se mantuvo a nivel de prototipo y no incluyó una operación productiva completa a largo plazo.

---

## La solución técnica en una sola visión

La lógica técnica del proyecto puede resumirse así:

1. recopilar y preparar datos históricos operativos y ambientales
2. definir un objetivo de predicción robusto relacionado con el comportamiento de la turbidez
3. entrenar y comparar distintos enfoques de modelado secuencial
4. evaluar tanto el rendimiento predictivo como la utilidad operativa
5. proponer un prototipo desplegable alineado con la realidad de planta

Esto hace que el proyecto se acerque más a un **flujo de trabajo de IA industrial** que a un benchmark aislado de machine learning.

---

## Modelos evaluados

Se llevó a cabo un estudio comparativo utilizando cinco familias de redes neuronales:

* **MLP**
* **LSTM**
* **GRU**
* **TCN**
* **Temporal Fusion Transformer (TFT)**

La comparación se realizó para tres horizontes de predicción:

* **1 hora**
* **3 horas**
* **6 horas**

Esta configuración multihorizonte era importante porque el valor operativo cambia según el horizonte temporal. Una predicción a corto plazo puede servir para mejorar la conciencia situacional inmediata, mientras que una predicción más larga resulta más útil para ajustar el proceso de forma proactiva.

---

## Resultado principal

El principal resultado del caso de estudio es que **el horizonte de predicción influye fuertemente en la idoneidad de cada modelo**.

El proyecto mostró que:

* todos los modelos pueden aportar predicciones útiles a corto plazo
* las diferencias entre modelos se vuelven más importantes a medida que aumenta el horizonte
* el **Temporal Fusion Transformer (TFT)** obtuvo el mejor rendimiento en horizontes de predicción más largos
* un enfoque basado en **LSTM** ofrece un equilibrio práctico muy sólido para un despliegue piloto inicial gracias a su fiabilidad y a su relativa simplicidad de implementación

Esta es una de las conclusiones de ingeniería más relevantes del proyecto: el mejor modelo no es solo el que logra la mejor métrica, sino el que mejor encaja con el propósito operativo y con las restricciones de despliegue.

---

## Valor práctico

El valor práctico del proyecto reside en su potencial para apoyar:

* una respuesta más temprana ante eventos de turbidez
* un uso más eficiente de los coagulantes
* una mejor gestión de la filtración
* una mejor planificación operativa durante perturbaciones provocadas por lluvias
* un apoyo más sólido a la toma de decisiones digitales para los operadores de planta

De forma más amplia, el proyecto demuestra cómo la IA puede integrarse en entornos de infraestructura crítica de forma estructurada y realista.

---

## Qué hace que este sea un buen proyecto de portfolio

Este proyecto es especialmente relevante como caso técnico de portfolio porque combina:

* un **problema industrial real**
* **predicción de series temporales** con consecuencias operativas
* **integración de datos heterogéneos**
* **comparación de modelos de deep learning**
* **validación de prototipo**
* **mentalidad de ingeniería orientada al despliegue**

Demuestra no solo habilidades de desarrollo de modelos, sino también la capacidad de plantear un problema de IA aplicada de extremo a extremo: desde la comprensión del contexto y la preparación de datos hasta las decisiones de arquitectura, la validación y la planificación de un piloto.

---

## Ideas clave

* El proyecto se centra en **predecir la turbidez antes de que se convierta en un problema operativo**.
* Su principal aportación es una **metodología de IA reutilizable**, no únicamente un modelo entrenado.
* La solución integra **datos SCADA + variables meteorológicas** en un contexto real de planta.
* Se compararon distintas familias de modelos bajo escenarios de predicción de **1 h, 3 h y 6 h**.
* **TFT** ofreció el mejor rendimiento en horizontes largos, mientras que **LSTM** surgió como una opción práctica preparada para piloto.
* El proyecto demuestra la viabilidad de la **IA como herramienta de apoyo operativo** para la digitalización del tratamiento del agua.

---

## Próximos documentos

Para seguir explorando el proyecto, las siguientes secciones recomendadas son:

* [`context-and-problem.md`](context-and-problem.md)
* [`methodology.md`](methodology.md)
* [`data-pipeline.md`](data-pipeline.md)
* [`modeling.md`](modeling.md)
* [`results.md`](results.md)
