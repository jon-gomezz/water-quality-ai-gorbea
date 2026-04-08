# Metodología

## Propósito de la metodología

La principal aportación de este proyecto no es solo un modelo entrenado de predicción de turbidez, sino una **metodología estructurada** para diseñar, validar y preparar el despliegue de sistemas de predicción de calidad del agua basados en IA en entornos industriales reales.

La metodología se concibió como un **marco de ingeniería reutilizable**. Es decir, el objetivo era definir un flujo de trabajo que pudiera aplicarse más allá del caso concreto de Gorbea, adaptándolo a otras plantas, a otras variables de calidad del agua o a otros casos de uso predictivos dentro de programas de digitalización de utilities.

Por eso el proyecto se organizó como una metodología de ciclo de vida completo y no como un experimento de machine learning limitado.

---

## Principios metodológicos

La metodología se construye en torno a varios principios prácticos.

### 1. Enfoque de ingeniería de extremo a extremo

El proyecto trata la predicción como solo una parte del sistema. Una solución útil de IA industrial también debe abordar:

* adquisición de datos
* preprocesado
* selección de modelo
* validación
* integración operativa
* monitorización
* mantenimiento

### 2. Reutilización

El marco está pensado para ser transferible. El objetivo no es resolver únicamente un problema aislado de una planta, sino definir un proceso repetible que reduzca la necesidad de rediseñar todo el flujo de trabajo desde cero en proyectos futuros.

### 3. Utilidad operativa por encima de la lógica de benchmark

La metodología no optimiza únicamente métricas offline. También considera si el sistema resultante puede apoyar decisiones operativas reales, si puede integrarse en flujos de trabajo existentes y si el personal de planta podría confiar en él y utilizarlo de forma realista.

### 4. Conciencia de ciclo de vida

El modelo no se trata como un artefacto estático. La metodología asume que las condiciones ambientales, el comportamiento de la planta y las distribuciones de datos pueden evolucionar con el tiempo, por lo que la monitorización, el mantenimiento y la gestión del ciclo de vida del modelo se consideran parte del diseño desde el principio.

---

## Flujo de trabajo de alto nivel

La metodología se organiza en cinco grandes fases:

1. **Definición de objetivos y requisitos**
2. **Diseño del sistema**
3. **Implementación y validación**
4. **Estrategia de operación**
5. **Mantenimiento y gestión del ciclo de vida**

<p align="center">
  <img src="/assets/images/methodology/methodology-phases.png" alt="Diagrama de las fases de la metodología" width="760">
</p>
<p align="center"><em>Figura. Principales fases de la metodología reutilizable propuesta en el proyecto.</em></p>

Esta estructura hace que el marco sea adecuado tanto para desarrollo experimental como para preparación industrial orientada a piloto.

---

## Fase 1. Definición de objetivos y requisitos

La primera fase establece qué debe conseguir el sistema y bajo qué restricciones debe operar.

### 1.1 Planteamiento del problema

La metodología comienza traduciendo el problema operativo real a una tarea de predicción.

En el caso de Gorbea, esto significó definir el reto central como:

> anticipar la turbidez del agua bruta en la entrada de planta con suficiente antelación como para apoyar decisiones operativas proactivas.

Este paso es importante porque alinea la tarea de IA con la necesidad real de la planta y no con un objetivo abstracto de modelado.

### 1.2 Identificación de variables relevantes

Un sistema de predicción solo es tan bueno como la información que recibe. Por ello, la metodología incluye un análisis temprano de qué variables deben medirse e integrarse.

Para este caso, la información relevante incluye:

* variables de planta relacionadas con la turbidez
* variables de proceso procedentes del sistema SCADA
* indicadores operativos relacionados con la filtración
* variables meteorológicas
* variables del contexto hidrológico cuando resulten relevantes

El objetivo es identificar un conjunto de señales capaz de explicar tanto el estado actual de la planta como las condiciones aguas arriba que pueden provocar cambios futuros en la turbidez.

### 1.3 Definición de horizontes de predicción

El valor de una predicción depende mucho del horizonte temporal. Una predicción a 1 hora y una predicción a 6 horas no sirven al mismo propósito operativo.

Por eso, la metodología define explícitamente los horizontes de predicción en lugar de tratar el forecasting como una única tarea genérica.

En este proyecto, los horizontes seleccionados fueron:

* **1 hora**
* **3 horas**
* **6 horas**

Esto permitió evaluar qué modelos eran mejores para conciencia situacional inmediata y cuáles eran mejores para una anticipación más proactiva.

### 1.4 Consideraciones económicas y de sostenibilidad

La metodología también incluye una reflexión temprana sobre el valor práctico.

Eso significa plantear preguntas como:

* ¿puede el sistema reducir sobredosificaciones innecesarias de coagulante?
* ¿puede mejorar la gestión de la filtración?
* ¿puede reducir ineficiencias operativas durante eventos extremos?
* ¿es el valor esperado de la predicción coherente con el esfuerzo de despliegue?

Este paso ayuda a asegurar que el proyecto permanezca vinculado a un beneficio real de ingeniería.

---

## Fase 2. Diseño del sistema

Una vez que los objetivos están claros, la siguiente fase define la arquitectura de la solución.

Esta fase es más amplia que el diseño del modelo. Incluye infraestructura, lógica software, flujo de datos y consideraciones hardware.

### 2.1 Análisis de la infraestructura

Antes de construir el flujo predictivo, la metodología exige comprender el contexto real de despliegue.

Esto incluye:

* características del emplazamiento
* condiciones de riesgo
* instrumentación existente
* disponibilidad de datos
* restricciones de comunicaciones
* disponibilidad energética y de computación

Desde la perspectiva de portfolio, este es uno de los aspectos más importantes de la metodología porque muestra que el sistema se diseña teniendo en cuenta el entorno real.

### 2.2 Arquitectura software

La capa software se diseña alrededor del flujo completo de datos e inferencia.

Sus componentes principales son:

#### Comunicaciones con los centros de decisión

La metodología define cómo debe moverse la información entre el sistema de predicción y el entorno operativo de toma de decisiones.

En el caso de Gorbea, la lógica propuesta conecta las salidas del sistema de predicción con el contexto de control de AMVISA mediante una arquitectura digital ligera orientada al intercambio de información vía web y a la evaluación piloto.

#### Preprocesado de datos

El preprocesado es un bloque metodológico central.

El proyecto integra **registros operativos del SCADA** y **series meteorológicas**, que no comparten de forma natural la misma resolución temporal ni las mismas condiciones de calidad. Por ello, la metodología define un proceso de limpieza y armonización que incluye:

* alineamiento temporal entre fuentes
* eliminación de filas incompletas cuando es necesario
* detección de registros claramente erróneos
* revisión manual de valores sospechosos
* mitigación de la distorsión causada por valores extremos
* creación de un dataset unificado listo para entrenamiento

Este paso es especialmente importante porque la integración de datos heterogéneos es uno de los principales puntos de fallo en proyectos de IA industrial.

#### Persistencia y almacenamiento de datos

La metodología también contempla cómo deben almacenarse la información procesada y las salidas del sistema predictivo.

Esto incluye una lógica de historiador o data lake capaz de conservar:

* entradas del modelo
* predicciones generadas
* valores reales observados
* trazas de evaluación
* referencias a las versiones del modelo

Esto es esencial para análisis posteriores, monitorización y trazabilidad.

#### Selección del algoritmo de IA

En lugar de asumir desde el principio un único mejor modelo, la metodología define la selección del modelo como un proceso comparativo.

En este proyecto, se seleccionaron cinco familias de redes neuronales para evaluación:

* **MLP**
* **LSTM**
* **GRU**
* **TCN**
* **Temporal Fusion Transformer (TFT)**

Estos modelos se eligieron porque representan distintas formas de manejar dependencias temporales, complejidad e interpretabilidad.

#### Diseño del pipeline de entrenamiento

La metodología software también incluye un pipeline formal de entrenamiento.

Esto significa que el proyecto no se limita a “probar modelos”, sino que define un flujo de trabajo consistente para:

* preparación del dataset
* separación train/validation/test
* entrenamiento de modelos
* ajuste de hiperparámetros
* comparación entre arquitecturas
* selección final del modelo

### 2.3 Diseño de hardware y sensorización

La metodología también incluye el lado físico del sistema.

Esto implica:

* lógica de selección y configuración de sensores
* estrategia de medición de proceso
* arquitectura de comunicaciones
* consideraciones de edge computing cuando resulten relevantes

Incluso en un proyecto orientado a portfolio, mantener este bloque aporta valor porque muestra que la solución de IA se concibe como parte de un sistema ciberfísico más amplio y no como código aislado.

---

## Fase 3. Implementación y validación

Después de la fase de diseño, la metodología pasa a la implementación.

### 3.1 Entrenamiento de modelos

El primer paso es el entrenamiento real de las arquitecturas seleccionadas utilizando el dataset preparado.

La metodología exige evaluar el rendimiento de forma rigurosa en los distintos horizontes de predicción y comparar los modelos bajo la misma definición del problema.

Este diseño comparativo es importante porque evita elegir el modelo final basándose únicamente en intuición o en la popularidad de una arquitectura.

### 3.2 Validación funcional en laboratorio

Antes de cualquier despliegue orientado a campo, la metodología incluye una validación en un entorno controlado.

El propósito de esta etapa es verificar que:

* el flujo de datos funciona correctamente
* el preprocesado se comporta como se espera
* la inferencia puede ejecutarse de forma fiable
* las salidas son coherentes e interpretables
* el sistema puede operar como un prototipo funcional

Esta etapa actúa como puente entre el modelado offline y la preparación real de un piloto.

### 3.3 Preparación para despliegue piloto

La metodología se extiende después hacia una configuración de campo orientada a piloto.

En el caso de Gorbea, la propuesta no es un control autónomo completo, sino una **configuración de apoyo a la decisión en modo piloto**. El modelo genera predicciones para fines de evaluación y visualización, sin intervenir directamente en los lazos de control de planta.

Esta es una decisión metodológica muy sólida porque reduce el riesgo operativo y al mismo tiempo permite una evaluación significativa en un entorno real.

### 3.4 Lógica de interfaz y alertas

Un modelo solo se vuelve útil cuando su salida puede interpretarse y utilizarse.

Por ello, la metodología incluye el diseño de:

* visualización orientada al usuario
* comparación entre valores predichos y reales
* vistas de seguimiento del error
* futura lógica de alertas basada en umbrales y niveles de severidad

Esto hace que el marco se parezca más a un sistema de apoyo desplegable que a un experimento académico.

---

## Fase 4. Estrategia de operación

La metodología también define cómo debe comportarse el sistema durante la operación.

### 4.1 Monitorización del modelo

Una vez que el sistema está en funcionamiento, su comportamiento debe observarse de forma continua.

Por ello, la metodología incluye la monitorización de:

* calidad de predicción a lo largo del tiempo
* diferencias entre turbidez predicha y observada
* posibles derivas de rendimiento
* anomalías en el pipeline de entrada
* fallos de ejecución o problemas de comunicación

Esto es crítico porque las condiciones del tratamiento de agua son dinámicas y la degradación del modelo siempre es una posibilidad realista.

### 4.2 Política de actuación

Un sistema operativo de IA debe definir qué ocurre después de generar una predicción.

En el alcance piloto de este proyecto, el modelo funciona en **shadow mode**, lo que significa que las predicciones se generan y evalúan, pero no activan acciones automáticas de control.

Esto hace que la metodología sea más segura y más realista para una primera implementación. Permite a la utility ganar confianza en el sistema antes de considerar una integración operativa más estrecha.

### 4.3 Cumplimiento y trazabilidad

La metodología incluye explícitamente la trazabilidad como requisito de diseño.

Para un sistema de predicción industrial, debe ser posible reconstruir:

* qué entradas se utilizaron
* qué versión del modelo generó la salida
* qué predicción se produjo
* cuál fue el valor real observado
* cómo evolucionó el error

Esto es especialmente importante en entornos regulados y en cualquier piloto que pueda evolucionar más adelante hacia producción.

---

## Fase 5. Mantenimiento y gestión del ciclo de vida

Uno de los grandes puntos fuertes de la metodología es que no termina cuando el primer prototipo empieza a funcionar.

### 5.1 Mantenimiento preventivo y correctivo

El marco distingue entre:

* mantenimiento de la infraestructura física existente
* mantenimiento de la nueva capa software específica de IA

Para el componente software, esto incluye revisar logs, detectar fallos, verificar comportamientos correctos de reinicio y asegurar la continuidad de la ingesta de datos, el preprocesado y la inferencia.

### 5.2 Gestión del ciclo de vida del modelo

La metodología también anticipa el ciclo de vida a largo plazo del modelo.

Esto incluye definir futuros procedimientos para:

* versionado del modelo
* criterios de reentrenamiento
* sustitución de modelos desactualizados
* actualización de dependencias y librerías
* retirada controlada de versiones obsoletas

Esto es especialmente relevante en IA industrial, donde un modelo que hoy funciona bien puede volverse menos fiable a medida que evolucionan los patrones operativos.

---

## Cómo se aplicó la metodología en este proyecto

La metodología no se dejó en un plano conceptual. Se aplicó a un caso de estudio real centrado en la **predicción de turbidez en la entrada de la planta potabilizadora de Gorbea**.

El flujo aplicado incluyó:

* definición de objetivos específicos de planta
* selección de variables predictivas
* integración de datos SCADA y meteorológicos
* preprocesado y sincronización
* entrenamiento comparativo de cinco arquitecturas neuronales
* evaluación bajo horizontes de 1 h, 3 h y 6 h
* validación de un prototipo funcional
* preparación de una lógica de despliegue orientada a piloto

Esta aplicación práctica es lo que da valor de portfolio a la metodología: no solo se propone, sino que se demuestra sobre un problema operativo real.

---

## Por qué esta metodología importa

Desde la perspectiva de portfolio técnico, esta metodología es una de las partes más potentes del proyecto porque demuestra la capacidad de:

* plantear un problema industrial de forma sistemática
* traducir necesidades de planta a requisitos de IA
* diseñar un flujo predictivo completo
* integrar fuentes de datos heterogéneas
* comparar familias de modelos con rigor
* pensar en el despliegue desde el principio
* incluir monitorización, trazabilidad y mantenimiento en el diseño de la solución

Esto hace que el proyecto sea más maduro que un notebook estándar de forecasting o que un experimento tipo benchmark.

---

## La metodología en una frase

Este proyecto propone una **metodología reutilizable de extremo a extremo para la predicción de calidad del agua basada en IA**, que cubre el planteamiento del problema, la integración de datos, el diseño del sistema, la comparación de modelos, la validación, la preparación de piloto, la estrategia de operación y la gestión del ciclo de vida.

---

## Próximos documentos

Para continuar con la lectura técnica, las siguientes secciones recomendadas son:

* [`data-pipeline.md`](data-pipeline.md)
* [`modeling.md`](modeling.md)
* [`system-architecture.md`](system-architecture.md)
* [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
* [`results.md`](results.md)
