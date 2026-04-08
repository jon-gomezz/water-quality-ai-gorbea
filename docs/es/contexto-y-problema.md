# Contexto y problema

## Contexto industrial y ambiental

El tratamiento seguro del agua potable depende de la capacidad de mantener condiciones estables de proceso incluso cuando la calidad del agua bruta cambia rápidamente. Entre las variables que más afectan directamente al rendimiento del tratamiento, la **turbidez** es una de las más críticas.

La turbidez es una medida indirecta de las partículas en suspensión presentes en el agua, como arcillas, limos, materia orgánica, algas o partículas metálicas. Desde el punto de vista operativo, una turbidez alta no es solo un indicador de calidad del agua; es una fuente directa de dificultad en el tratamiento. Cuando la turbidez aumenta, la coagulación se vuelve más difícil de controlar, el rendimiento de la filtración puede deteriorarse y la eficacia de la desinfección puede verse afectada.

Esto hace que la turbidez sea especialmente relevante en plantas de tratamiento de agua potable que operan bajo condiciones hidrológicas muy variables.

---

## Por qué la turbidez es importante desde el punto de vista operativo

En una planta real, la turbidez no es una métrica abstracta de laboratorio. Tiene consecuencias inmediatas en la operación diaria.

Cuando la turbidez del agua bruta aumenta bruscamente:

* la dosificación de coagulante suele tener que ajustarse con rapidez
* los filtros pueden requerir lavados más frecuentes
* la estabilidad del proceso puede deteriorarse
* el consumo de productos químicos y energía puede aumentar
* los operadores disponen de menos tiempo para reaccionar de forma segura y eficiente

Desde una perspectiva de ingeniería, esto significa que la turbidez está estrechamente relacionada tanto con el **cumplimiento de la calidad del agua** como con la **eficiencia operativa**.

También tiene una dimensión de salud pública. Si la turbidez no se controla adecuadamente, puede comprometer la eficacia del tratamiento y reducir el margen de seguridad de los procesos de desinfección posteriores.

---

## Caso de estudio local: planta potabilizadora de Gorbea

Este proyecto se enmarca en la **planta potabilizadora de Gorbea**, operada por **AMVISA (Aguas Municipales de Vitoria-Gasteiz)**.

El sistema de Gorbea recibe agua de un contexto de captación complejo que incluye múltiples manantiales naturales del macizo de Gorbea, junto con la infraestructura hidrológica asociada de la zona. El proyecto se centra en predecir la **turbidez del agua bruta en la entrada de la planta**, justo antes de que el agua entre en el proceso de tratamiento.

Este punto es especialmente relevante porque refleja la calidad del recurso entrante que la planta debe acondicionar en tiempo real.

Un reto clave en este entorno es que la turbidez puede cambiar muy rápido durante episodios de lluvias intensas. En el caso de Gorbea, la turbidez del agua bruta puede pasar de niveles bajos de referencia a picos severos en una ventana de tiempo reducida, generando presión tanto sobre los procesos de tratamiento como sobre los operadores responsables de mantener estable el sistema.

<p align="center">
  <img src="/assets/images/overview/water-origin-scheme.png" alt="Esquema del origen del agua que alimenta la planta de Gorbea" width="760">
</p>
<p align="center"><em>Figura. Esquema simplificado de las fuentes de agua que alimentan la planta de Gorbea.</em></p>

---

## Variabilidad climática y presión operativa

El problema se intensifica todavía más por la creciente variabilidad de los episodios de lluvia.

Los eventos de tormenta más intensos aumentan la probabilidad de movilización repentina de sedimentos, lo que a su vez provoca picos bruscos de turbidez. En este tipo de contexto, reaccionar únicamente después de detectar el evento muchas veces no es suficiente. Para cuando la turbidez aumenta claramente en planta, la ventana de respuesta disponible puede ser ya demasiado corta para un ajuste óptimo del proceso.

Por eso, el problema no consiste solo en medir la turbidez con precisión, sino en **anticiparla con suficiente antelación como para apoyar decisiones proactivas**.

---

## Contexto de digitalización en AMVISA

Otra parte importante del contexto es la transformación digital más amplia de la empresa gestora del agua.

El caso de estudio se sitúa en un entorno de modernización en el que la infraestructura hidráulica está cada vez más instrumentada y orientada a los datos. Los sistemas SCADA, la monitorización continua, las redes de comunicaciones y las plataformas digitales están creando las condiciones para herramientas de apoyo a la decisión más avanzadas.

En este contexto, el proyecto no es un experimento aislado de IA. Encaja dentro de una transición más amplia hacia:

* mayor visibilidad operativa
* monitorización en tiempo real más sólida
* gestión predictiva en lugar de puramente reactiva
* un uso más integrado de los datos de proceso y ambientales

Esto hace que el caso de Gorbea sea especialmente adecuado para un proyecto de portfolio centrado en **IA industrial aplicada**.

---

## El problema central

A primera vista, el reto puede parecer simple: predecir valores futuros de turbidez.

En la práctica, el problema real es más amplio.

La cuestión principal no es solo la ausencia de un modelo predictivo, sino la falta de una **metodología sistemática y reutilizable** para diseñar, validar y preparar sistemas de predicción de calidad del agua basados en IA para un uso industrial real.

Muchos proyectos predictivos no consiguen pasar de la fase experimental porque se centran únicamente en el entrenamiento del modelo y subestiman el resto del flujo de trabajo de ingeniería.

En este caso, el proyecto aborda una pregunta más completa:

> ¿Cómo puede diseñarse un sistema de predicción de turbidez basado en IA de manera que sea técnicamente fiable, operativamente útil y realmente desplegable en un entorno de tratamiento de agua potable?

---

## Por qué el problema es difícil

Varios factores hacen que este problema no sea trivial.

<p align="center">
  <img src="/assets/images/data/raw-turbidity-before-outlier-removal.png" alt="Señal bruta de turbidez antes de eliminar outliers" width="860">
</p>
<p align="center"><em>Figura. Señal bruta de turbidez antes del tratamiento de outliers, mostrando la presencia de picos e irregularidades.</em></p>

### 1. Fuentes de datos heterogéneas

Una predicción fiable requiere combinar distintos tipos de información, entre ellos:

* registros operativos de planta procedentes del sistema SCADA
* variables meteorológicas
* variables de proceso relacionadas con la filtración y con el comportamiento en la entrada de planta

Estas fuentes no llegan de forma natural en un formato limpio y unificado. Pueden tener diferentes resoluciones temporales, valores ausentes, medidas ruidosas y problemas de sincronización.

### 2. Dinámicas temporales fuertes

El comportamiento de la turbidez depende mucho del tiempo. Los efectos de la lluvia pueden aparecer con retraso, las respuestas del proceso pueden desarrollarse a lo largo de varias horas y distintos patrones pueden dominar según el horizonte de predicción considerado.

Esto significa que la solución debe capturar tanto dinámicas de corto plazo como dependencias de más largo alcance.

### 3. Problemas del ciclo de vida del modelo

Incluso un modelo que funciona bien en experimentos offline puede degradarse con el tiempo si las condiciones operativas cambian. Cambios en los patrones ambientales, en el comportamiento de planta o en la calidad de los datos pueden reducir la fiabilidad predictiva.

Por tanto, el reto no es solo seleccionar un modelo, sino también monitorizarlo, actualizarlo y mantenerlo a lo largo del tiempo.

### 4. Utilidad operativa

Un modelo matemáticamente preciso no es suficiente si el personal de planta no puede confiar en él o utilizarlo de forma efectiva.

Para ser útil en la práctica, el sistema debe generar salidas que sean:

* oportunas
* comprensibles
* accionables
* alineadas con decisiones operativas reales

En otras palabras, el proyecto debe cerrar la brecha entre el **rendimiento de ciencia de datos** y la **utilidad real en la operación de planta**.

---

## Por qué la predicción aporta valor

Si la turbidez puede predecirse con suficiente antelación, los operadores pueden actuar antes de que el evento se vuelva crítico.

Eso genera varios beneficios potenciales:

* ajuste más proactivo del coagulante
* mejor gestión de los filtros
* menor riesgo de sobredosificación innecesaria
* mayor estabilidad del proceso durante eventos provocados por la lluvia
* mejor apoyo al cumplimiento de la calidad del agua

Por eso, el proyecto debe entenderse como un **sistema de apoyo a la decisión**, y no solo como un ejercicio de forecasting.

---

## Planteamiento del problema para este proyecto de portfolio

Este repositorio presenta el problema del siguiente modo:

Una planta de tratamiento de agua potable que opera bajo condiciones ambientales variables necesita una forma fiable de anticipar picos de turbidez del agua bruta antes de que se conviertan en un riesgo operativo. El reto no consiste solo en predecir la variable en sí, sino en desarrollar una metodología de IA reproducible capaz de integrar datos heterogéneos, comparar arquitecturas de predicción adecuadas y preparar la solución para un despliegue industrial realista.

---

## Por qué este problema constituye un caso técnico sólido

Desde la perspectiva de portfolio, este problema tiene mucho valor porque combina:

* un caso real de infraestructura crítica
* consecuencias operativas medibles
* predicción multivariable de series temporales
* integración de datos ambientales e industriales
* mentalidad de ingeniería orientada al despliegue
* valor práctico como herramienta de apoyo a la decisión

Eso lo convierte en un ejemplo sólido de cómo la IA puede aplicarse más allá de benchmarks de laboratorio, en un contexto donde la fiabilidad, la interpretabilidad y la utilidad operativa importan tanto como la precisión predictiva bruta.

---

## Siguiente documento

La siguiente sección recomendada es [`methodology.md`](methodology.md), donde se explica cómo el proyecto traduce este problema en un flujo de trabajo de IA estructurado.
