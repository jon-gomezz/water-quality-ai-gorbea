# Predicción de la calidad del agua basada en IA para la planta potabilizadora de Gorbea

## Visión general

Este repositorio documenta un proyecto técnico de portfolio centrado en la **predicción de la turbidez del agua bruta mediante inteligencia artificial** en un contexto real de tratamiento de agua potable.

El trabajo se basa en un caso de estudio desarrollado con **AMVISA (Aguas Municipales de Vitoria-Gasteiz)** y se centra en la **planta potabilizadora de Gorbea**, donde los aumentos bruscos de turbidez pueden comprometer la estabilidad del proceso, incrementar el consumo de productos químicos y dificultar la operación de la planta durante episodios de lluvias intensas.

En lugar de presentar el trabajo como una tesis académica tradicional, este repositorio lo reformula como un **proyecto de ingeniería preparado para portfolio**, centrado en:

* comprensión del problema
* diseño metodológico
* integración de datos
* desarrollo de modelos
* validación
* preparación para despliegue

<p align="center">
  <img src="/assets/images/overview/filters-gorbea.png" alt="Filtros de la planta potabilizadora de Gorbea" width="820">
</p>
<p align="center"><em>Figura. Filtros de la planta potabilizadora de Gorbea.</em></p>

---

## El problema

Las plantas de tratamiento de agua operan bajo condiciones ambientales y operativas cambiantes. En el caso de Gorbea, la turbidez en la entrada de la planta puede aumentar bruscamente en poco tiempo, especialmente durante episodios de lluvias intensas.

Estos eventos generan varios retos operativos:

* mayor incertidumbre en la operación de la planta
* mayor consumo de coagulante
* lavados de filtros más frecuentes
* menor estabilidad del proceso
* mayor riesgo de incumplimiento normativo

Una respuesta reactiva muchas veces no es suficiente. El verdadero valor está en poder **anticipar los eventos de turbidez con la antelación suficiente para apoyar una toma de decisiones proactiva**.

---

## Objetivo del proyecto

El objetivo principal del proyecto no fue simplemente entrenar un modelo, sino **diseñar, documentar y validar una metodología de IA reutilizable** para la predicción de calidad del agua en entornos industriales.

Esta metodología se aplicó después a un caso de estudio real:

> **predecir la turbidez en la entrada de la planta de Gorbea utilizando datos históricos del SCADA y variables meteorológicas**.

Por tanto, el proyecto tiene dos resultados complementarios:

1. una **solución predictiva práctica** para el caso de Gorbea
2. un **marco metodológico replicable** para futuros proyectos de digitalización del agua

---

## Por qué este proyecto es relevante

Es un proyecto de portfolio sólido porque combina varias dimensiones de alto valor en ingeniería:

* **contexto industrial real**
* **predicción de series temporales**
* **integración de datos de múltiples fuentes**
* **deep learning para apoyo a la decisión operativa**
* **validación de prototipo**
* **mentalidad orientada al despliegue**

No es solo un ejercicio de machine learning. También es un proyecto de pensamiento sistémico que implica pipelines de datos, consideraciones de infraestructura, monitorización y usabilidad operativa.

---

## Contexto del caso de estudio

El proyecto se desarrolló dentro del contexto de digitalización de **AMVISA**, la empresa pública de aguas de Vitoria-Gasteiz.

En la planta de Gorbea, la turbidez del agua bruta puede pasar de valores bajos de referencia a niveles muy altos en cuestión de horas durante eventos de tormenta. Estos episodios afectan a la filtración, a la dosificación de coagulantes y a la estabilidad general de la planta.

Esto hace que la predicción de turbidez sea especialmente útil como herramienta de apoyo operativo.

El proyecto también encaja dentro de un contexto más amplio de modernización de utilities, en el que los sistemas SCADA, la sensorización ambiental, las infraestructuras de comunicaciones y la analítica predictiva pasan a formar parte de un mismo ecosistema digital.

---

## Enfoque técnico

El proyecto se estructuró como un pipeline metodológico completo.

<p align="center">
  <img src="/assets/images/methodology/methodology-phases.png" alt="Fases metodológicas del marco de predicción de calidad del agua basado en IA" width="760">
</p>
<p align="center"><em>Figura. Fases de alto nivel de la metodología de IA propuesta.</em></p>

### 1. Definición de objetivos y requisitos

La primera fase se centró en comprender:

* qué variables debían medirse
* qué decisiones operativas podían beneficiarse de la predicción
* qué horizontes de predicción eran más útiles
* qué restricciones del sistema afectarían a un futuro despliegue piloto

### 2. Integración de datos

El flujo predictivo combina fuentes de información heterogéneas, principalmente:

* **datos históricos operativos del SCADA**
* **variables meteorológicas**
* variables de planta relacionadas con la filtración y con las condiciones de entrada

Una parte importante del trabajo consistió en gestionar:

* distintas granularidades temporales
* valores ausentes
* registros anómalos
* sincronización entre fuentes
* preprocesado orientado a dejar los datos listos para entrenamiento

### 3. Comparación de modelos

El estudio comparó cinco familias de redes neuronales:

* **MLP**
* **LSTM**
* **GRU**
* **TCN**
* **Temporal Fusion Transformer (TFT)**

La comparación se realizó para distintos horizontes de predicción:

* **1 hora**
* **3 horas**
* **6 horas**

Esto fue importante porque el modelo más útil no siempre es el mismo para todos los horizontes.

### 4. Evaluación y validación

Los modelos se evaluaron cuantitativamente mediante métricas de regresión y después se analizaron desde una perspectiva operativa.

El proyecto también fue más allá de la experimentación offline al tener en cuenta:

* validación funcional
* lógica de prototipo
* preparación para despliegue piloto
* implicaciones de alertas e interfaz
* monitorización del modelo y gestión de su ciclo de vida

---

## Hallazgos principales

El proyecto mostró que **el horizonte de predicción influye de forma muy significativa**.

* Para la **predicción a corto plazo**, varias arquitecturas lograron un rendimiento robusto.
* Para la **predicción proactiva a más largo plazo**, arquitecturas más avanzadas aportaron beneficios más claros.
* El **Temporal Fusion Transformer (TFT)** mostró el mejor rendimiento global para horizontes largos.
* Para un despliegue inicial orientado a piloto, **LSTM** se identificó como una opción práctica muy sólida por su equilibrio entre fiabilidad y simplicidad de implementación.

Esta distinción es importante desde una perspectiva de ingeniería: el “mejor modelo” no es solo el que obtiene la mejor métrica bruta, sino el que mejor se ajusta al caso de uso operativo previsto.

---

## Valor de ingeniería del proyecto

Más allá del caso de estudio concreto, este proyecto demuestra experiencia en:

* traducir un problema industrial real a un flujo de trabajo de IA
* diseñar una metodología estructurada en lugar de un experimento aislado
* tender un puente entre ciencia de datos y operaciones
* comparar arquitecturas para modelado secuencial
* pensar desde el principio en despliegue, monitorización y mantenibilidad

Esto hace que el proyecto sea relevante no solo para aplicaciones del sector del agua, sino también para **IA industrial**, **analítica predictiva** y **sistemas de apoyo a la decisión basados en series temporales**.

---

## Estructura del repositorio

Este repositorio está organizado como un portfolio técnico bilingüe.

```text
.
├── README.md
├── assets/
│   └── images/
├── docs/
│   ├── en/
│   │   ├── README.md
│   │   ├── project-overview.md
│   │   ├── context-and-problem.md
│   │   ├── methodology.md
│   │   ├── data-pipeline.md
│   │   ├── modeling.md
│   │   ├── system-architecture.md
│   │   ├── deployment-and-monitoring.md
│   │   ├── results.md
│   │   └── lessons-learned.md
│   └── es/
│       ├── README.md
│       ├── descripcion-del-proyecto.md
│       ├── contexto-y-problema.md
│       ├── metodologia.md
│       ├── proceso-de-datos.md
│       ├── modelado.md
│       ├── arquitectura-del-sistema.md
│       ├── implementacion-y-monitorizacion.md
│       ├── resultados.md
│       └── lecciones-aprendidas.md
└── references/
```

---

## Orden de lectura recomendado

Para entender el proyecto con rapidez, el mejor orden es:

1. [`descripcion-del-proyecto.md`](descripcion-del-proyecto.md)
2. [`contexto-y-problema.md`](contexto-y-problema.md)
3. [`metodologia.md`](metodologia.md)
4. [`proceso-de-datos.md`](proceso-de-datos.md)
5. [`modelado.md`](modelado.md)
6. [`arquitectura-del-sistema.md`](arquitectura-del-sistema.md)
7. [`resultados.md`](resultados.md)
8. [`implementacion-y-monitorizacion.md`](implementacion-y-monitorizacion.md)
9. [`lecciones-aprendidas.md`](lecciones-aprendidas.md)

---

## Nota sobre el alcance

Este repositorio está pensado como una **muestra técnica** del proyecto.

Por motivos de claridad y confidencialidad:

* el proyecto se presenta en **formato portfolio**, no como una copia de la tesis capítulo por capítulo
* el énfasis se pone en la **lógica de ingeniería, la metodología, la arquitectura y la interpretación de resultados**
* no se incluyen necesariamente en su totalidad los datos industriales sensibles ni los detalles de implementación específicos de producción

---

## Público objetivo

Este repositorio es especialmente relevante para:

* reclutadores que evalúan proyectos técnicos de portfolio
* ingenieros interesados en aplicaciones de IA industrial
* profesionales de datos que trabajan con predicción de series temporales
* utilities y equipos de infraestructuras que exploran analítica predictiva

---

## Autor

**Jon Gomez Olarte**
Ingeniero mecánico | Portfolio técnico orientado a IA
