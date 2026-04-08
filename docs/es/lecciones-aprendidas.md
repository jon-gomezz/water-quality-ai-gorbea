# Lecciones aprendidas

## Por qué importa esta sección

Un buen proyecto de ingeniería no termina con reportar buenos resultados. También explica:

* qué funcionó bien
* qué fue más difícil de lo esperado
* qué compromisos de diseño dieron forma a la solución final
* qué aspectos habría que mejorar antes de escalar más el sistema

Esto es especialmente importante en un proyecto de IA aplicada. En entornos industriales reales, el éxito técnico no depende solo del rendimiento del modelo, sino también de la calidad de los datos, del realismo del despliegue, de la utilidad operativa y de la mantenibilidad a largo plazo.

Esta sección resume las principales lecciones aprendidas durante el diseño, desarrollo, validación y despliegue orientado a piloto del sistema de predicción de turbidez de Gorbea.

---

## 1. La parte más difícil rara vez es solo el modelo

Una de las lecciones más claras del proyecto es que construir un sistema predictivo útil no es únicamente un problema de modelado.

A primera vista, puede parecer que el principal reto consiste en elegir la mejor arquitectura neuronal. En la práctica, una parte muy grande del esfuerzo del proyecto está en otros aspectos:

* comprender correctamente el problema operativo
* identificar variables relevantes
* integrar fuentes heterogéneas
* alinear timestamps y resoluciones
* detectar registros erróneos
* preparar un flujo de despliegue coherente

Esta es una lección de ingeniería muy importante. En IA industrial, un buen modelo no puede compensar una base débil de datos y de diseño del sistema.

---

## 2. La integración de datos multifuente genera gran parte del valor técnico real

Una segunda lección clave es que combinar datos de planta e información meteorológica fue esencial.

Si el proyecto se hubiera apoyado solo en medidas históricas del lado de planta, el sistema habría capturado la dinámica local del proceso, pero habría tenido menos capacidad para anticipar los desencadenantes ambientales que están detrás de los eventos de turbidez.

Al integrar variables meteorológicas, el proyecto ganó una perspectiva de forecasting más proactiva.

Esto refuerza una lección general importante:

> en forecasting industrial, la señal más valiosa muchas veces surge de combinar el comportamiento interno del proceso con información contextual externa.

Esta idea es reutilizable mucho más allá del sector del agua.

---

## 3. El horizonte de predicción lo cambia todo

Otra lección central es que la idoneidad del modelo depende enormemente del horizonte de predicción.

Esto puede parecer obvio en teoría, pero en la práctica la diferencia es decisiva.

A 1 hora, varias arquitecturas ya eran capaces de producir resultados muy sólidos. A 6 horas, el problema se volvió mucho más difícil y la diferencia entre modelos se amplió de forma significativa.

Esto lleva a una conclusión importante:

* no existe un modelo universalmente mejor en abstracto
* solo existe un mejor modelo para un horizonte operativo y un propósito concretos

Esta lección importa porque desplaza el proyecto desde una lógica de benchmark hacia una lógica de ingeniería.

---

## 4. El mejor modelo de investigación no siempre es el mejor modelo para un primer despliegue

El proyecto también dejó claro que la superioridad predictiva y la idoneidad para despliegue no son lo mismo.

El **Temporal Fusion Transformer (TFT)** ofreció el mejor rendimiento de forecasting a largo horizonte, especialmente en los escenarios más proactivos.

Sin embargo, eso no lo convierte automáticamente en la mejor primera opción de despliegue real.

Para un piloto inicial, **LSTM** siguió siendo muy atractivo porque ofrecía:

* un comportamiento predictivo sólido
* menor complejidad de implementación
* mayor facilidad de mantenimiento
* una adopción operativa más sencilla

Esta es una de las lecciones más maduras del proyecto:

> las decisiones de ingeniería deben equilibrar rendimiento con simplicidad, confianza y mantenibilidad.

---

## 5. Un piloto seguro vale más que una automatización prematura

Una de las decisiones prácticas más fuertes del proyecto fue adoptar un **piloto en shadow mode** en lugar de intentar automatizar acciones de planta demasiado pronto.

Esa elección generó varias ventajas:

* menor riesgo operativo
* aceptación más sencilla por parte del personal técnico
* validación más clara del comportamiento del modelo en campo
* separación más segura entre experimentación y control de planta

Esta experiencia refuerza una lección más amplia para proyectos de IA industrial:

* primero demostrar que el sistema es fiable
* después demostrar que resulta útil en la práctica
* y solo a partir de ahí considerar una integración operativa más estrecha

Intentar saltarse estas etapas habría hecho que la solución fuera menos creíble y más difícil de desplegar de forma responsable.

---

## 6. La simplicidad suele ser una fortaleza en la arquitectura piloto

La arquitectura piloto evitó deliberadamente una complejidad de escala empresarial.

En lugar de construir desde el inicio una infraestructura pesada, el proyecto utilizó:

* un PC dedicado de despliegue
* monitorización cloud ligera a través de ThingSpeak
* almacenamiento local de artefactos de ejecución
* supervisión visual sencilla

Esto no fue un atajo en sentido negativo. Fue un principio de diseño consciente.

Para una primera etapa de despliegue, un sistema más simple suele ser mejor porque es:

* más fácil de depurar
* más fácil de entender
* más fácil de mantener
* más fácil de validar paso a paso

Esta es también una lección importante para proyectos de portfolio: el realismo suele resultar más convincente que el sobreingeniería.

---

## 7. La monitorización no es opcional

Otra lección importante es que el despliegue es solo el comienzo del ciclo de vida del sistema.

Aunque el modelo funcione bien durante la validación offline, las condiciones reales pueden cambiar debido a:

* efectos estacionales
* nuevos patrones de lluvia
* deriva de sensores
* cambios en la operación de planta
* problemas de calidad de datos

Esto significa que la monitorización debe tratarse como parte de la solución y no como una idea añadida al final.

El piloto utiliza una estrategia de monitorización relativamente simple, pero incluso eso basta para demostrar un principio crítico:

> un modelo que no puede observarse no puede generar confianza.

Esta idea es especialmente relevante en entornos regulados y de infraestructura crítica.

---

## 8. La trazabilidad genera credibilidad

El proyecto también mostró la importancia de preservar la trazabilidad a lo largo de todo el flujo de trabajo.

Para que un sistema predictivo sea útil en la práctica, debe ser posible reconstruir:

* qué datos entraron al modelo
* qué versión del modelo produjo la salida
* qué predicción se generó
* qué ocurrió realmente después
* cómo evolucionó el error con el tiempo

Sin esa trazabilidad, se vuelve mucho más difícil depurar problemas, explicar fallos o justificar mejoras futuras.

Esta es una lección importante para cualquier sistema de IA que aspire a ir más allá de la experimentación.

---

## 9. Comprender el dominio importa tanto como saber de machine learning

El proyecto reforzó la idea de que los buenos resultados no dependen únicamente de conocer arquitecturas de redes neuronales.

También dependen de entender:

* por qué la turbidez es una variable operativamente sensible
* cómo afecta la lluvia a las condiciones de entrada de planta
* por qué importan los retardos en la respuesta de la planta
* cómo usarían realmente los operadores una predicción
* qué riesgos son aceptables en un entorno de tratamiento de agua

Esta lección es especialmente valiosa desde una perspectiva profesional. Muestra que el trabajo de IA aplicada se vuelve mucho más sólido cuando el modelado técnico está conectado con una comprensión profunda del problema.

---

## 10. La metodología es un entregable de primer nivel por sí misma

Una lección especialmente importante de este proyecto es que la **metodología en sí** es uno de los principales resultados.

El proyecto no solo ofrece una comparación de modelos para una planta concreta. También aporta un marco estructurado para:

* planteamiento del problema
* selección de variables
* integración de datos
* comparación de modelos
* preparación de piloto
* diseño de monitorización
* pensamiento orientado al mantenimiento

Eso significa que el trabajo tiene valor más allá del caso específico de Gorbea.

Esta es una de las razones más fuertes por las que el proyecto funciona bien como caso técnico de portfolio: demuestra no solo ejecución, sino también pensamiento de ingeniería reutilizable.

---

## 11. Todavía existen limitaciones importantes

Una buena sección de lecciones aprendidas también debe reconocer lo que el proyecto todavía no resuelve por completo.

Algunas de las principales limitaciones son:

* el piloto sigue siendo un prototipo, no un despliegue completo en producción
* la monitorización sigue siendo en gran medida manual y visual
* todavía no existe una capa completa de detección automática de deriva
* la lógica desplegada todavía no activa alertas ni acciones
* la robustez a largo plazo bajo condiciones cambiantes requeriría una validación de campo más prolongada
* una futura escalabilidad necesitaría una gestión más sólida de artefactos y una gobernanza operativa más formal

Estas limitaciones no debilitan el proyecto. Al contrario, lo hacen más creíble porque separan claramente lo que ya se ha conseguido de lo que pertenece a una fase futura de mayor madurez.

---

## 12. Qué mejoraría a continuación

Si el proyecto continuara hacia una etapa operativa más avanzada, las siguientes mejoras probablemente incluirían:

* una validación piloto más larga bajo distintas condiciones estacionales
* una monitorización más formal de deriva y rendimiento
* criterios más claros de reentrenamiento
* una gestión más sólida de artefactos y versiones
* umbrales de alerta estructurados para uso operativo
* integración gradual con flujos de trabajo de operadores o con notificaciones del lado SCADA
* una evaluación más profunda de la explicabilidad y de la confianza de los usuarios en las predicciones

Estos pasos moverían el sistema desde un piloto validado hacia una herramienta de apoyo con IA más preparada para producción.

---

## Principales lecciones en formato compacto

Las lecciones más importantes del proyecto pueden resumirse así:

* **la ingeniería de datos y el diseño del sistema importan tanto como la elección del modelo**
* **la integración multifuente es esencial para una predicción proactiva**
* **el horizonte de predicción determina qué modelo es realmente adecuado**
* **el mejor modelo en benchmark no siempre es el mejor candidato para despliegue**
* **la validación en shadow mode es un primer paso muy inteligente en IA industrial**
* **las arquitecturas piloto simples pueden ser más sólidas que otras sobreingenierizadas**
* **la monitorización y la trazabilidad son necesarias para generar confianza**
* **la comprensión del dominio es inseparable del éxito en IA aplicada**
* **una metodología reutilizable puede ser tan valiosa como el modelo final en sí**

---

## Por qué estas lecciones refuerzan el proyecto de portfolio

Desde la perspectiva de portfolio, estas lecciones fortalecen el proyecto porque muestran madurez.

El repositorio no presenta el trabajo como si el problema estuviera completamente resuelto. En su lugar, muestra:

* qué se consiguió
* qué compromisos se tomaron
* qué riesgos se gestionaron con cuidado
* qué haría falta para la siguiente etapa

Eso le da al proyecto mucha más credibilidad que un repositorio que solo presenta una historia de éxito pulida y sin matices.

---

## Conclusión principal

La lección más profunda del proyecto es que **la IA industrial útil no se construye persiguiendo solo la precisión del modelo**. Surge de la combinación de una metodología sólida, una integración de datos fiable, realismo de despliegue, validación segura y monitorización continua.

Eso es lo que convierte a un modelo de forecasting en una solución de ingeniería creíble.

---

## Nota de cierre

Este proyecto demuestra no solo que la IA puede aplicarse a la predicción de turbidez en un contexto real de tratamiento de agua, sino también que hacerlo de forma responsable exige una perspectiva de sistema completo.

Esa perspectiva de sistema completo es, en última instancia, la lección más valiosa de todo el trabajo.
