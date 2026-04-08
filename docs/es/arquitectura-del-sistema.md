# Arquitectura del sistema

## Por qué la arquitectura del sistema importa en este proyecto

Este proyecto no se diseñó como un experimento aislado de machine learning. Desde el principio, el modelo predictivo se planteó como parte de un **sistema más amplio de apoyo a la decisión industrial**.

Eso significa que la arquitectura tenía que responder a preguntas que van más allá de la precisión del modelo:

* ¿De dónde vienen los datos?
* ¿Cómo se transfieren de forma segura?
* ¿Dónde se procesan?
* ¿Cómo se entregan las predicciones a los operadores?
* ¿Cómo puede validarse el sistema sin interferir en la operación de la planta?

Por esa razón, la arquitectura se diseñó como una **solución práctica orientada a piloto** que conecta datos de planta, información meteorológica, inferencia predictiva y monitorización de una forma controlada y trazable.

---

## Objetivos de diseño

La arquitectura se diseñó en torno a cinco objetivos principales:

1. **Aprovechar al máximo la infraestructura ya existente en planta**
2. **Mantener la solución lo bastante simple desde el punto de vista técnico para un despliegue piloto**
3. **Proteger la red de control industrial frente a exposiciones innecesarias**
4. **Permitir monitorización en tiempo real o casi en tiempo real de la calidad de predicción**
5. **Preparar un camino hacia una futura integración operativa sin forzarla demasiado pronto**

Por eso, el sistema resultante prioriza modularidad, seguridad y realismo de despliegue por encima de la complejidad arquitectónica.

---

## Arquitectura de alto nivel

A alto nivel, el sistema puede entenderse como cinco capas conectadas:

1. **Capa de sensorización de planta y datos operativos**
2. **Capa de comunicaciones y transferencia segura de datos**
3. **Capa de persistencia y preprocesado de datos**
4. **Capa de ejecución del modelo e inferencia**
5. **Capa de visualización y apoyo a la decisión**

La arquitectura enlaza la realidad física de la planta de tratamiento con un pipeline software capaz de generar predicciones de turbidez y exponerlas en un formato adecuado para evaluación piloto.

---

## La arquitectura en una sola visión

La lógica práctica del sistema es la siguiente:

* la planta de Gorbea genera datos operativos a través de su instrumentación existente y de su entorno de control basado en PLC
* las señales de planta se transmiten de forma segura hacia el centro de control
* los datos meteorológicos se obtienen de una fuente externa
* ambas fuentes se sincronizan y se preparan en un PC de despliegue
* el modelo de IA seleccionado se ejecuta en ese PC
* la predicción generada, el valor real de turbidez y el error de predicción se envían a un dashboard online para su monitorización y validación

Esta arquitectura es deliberadamente conservadora: permite una validación significativa sin cerrar directamente el lazo sobre acciones de control de planta.

---

## 1. Capa física y de infraestructura

El sistema propuesto está diseñado para operar dentro del entorno físico real de la **planta potabilizadora de Gorbea** y de la infraestructura de control asociada de AMVISA.

Un aspecto relevante de la disposición de la planta es que el proceso hidráulico se distribuye entre áreas físicas clave, entre ellas:

* el **edificio de captación de manantiales**, donde confluyen los caudales de varias fuentes
* el **edificio de filtros y cloración**, donde se encuentra el equipamiento de tratamiento y donde se concentra el futuro contexto hardware y software de apoyo con IA

Esto es importante desde el punto de vista arquitectónico porque cualquier solución realmente desplegable debe adaptarse a:

* la huella existente del equipamiento
* las restricciones reales de acceso
* el espacio disponible para instalación
* el suministro energético y el entorno operativo actuales de la planta

El diseño también asume que la planta ya dispone de un entorno eléctrico lo bastante robusto para un despliegue piloto, por lo que no es necesario rediseñar desde cero la infraestructura de alimentación.

<p align="center">
  <img src="/assets/images/overview/aerial-infrastructure.png" alt="Vista aérea de la infraestructura implicada en el caso de estudio de Gorbea" width="840">
</p>
<p align="center"><em>Figura. Contexto físico de la infraestructura de Gorbea utilizada en el caso de estudio.</em></p>

---

## 2. Capa de sensorización y datos de planta

La arquitectura está construida para aprovechar la **instrumentación existente ya instalada en la planta**.

Esta es una decisión de ingeniería importante porque evita introducir una complejidad hardware innecesaria en la fase piloto.

En lugar de rediseñar desde cero la capa de sensorización, el proyecto se apoya en la instrumentación actual de proceso asociada a variables como:

* turbidez
* caudal
* presión
* otras medidas de proceso relevantes para el comportamiento de la entrada y de la filtración

Esto hace que la propuesta sea mucho más realista como proyecto de portfolio, porque refleja cómo suelen introducirse en la práctica los sistemas de IA industrial: construyendo sobre activos operativos ya existentes en lugar de sustituirlos por completo.

---

## 3. Arquitectura de comunicaciones

Una de las decisiones arquitectónicas más importantes del proyecto tiene que ver con **cómo mover los datos de planta sin comprometer la integridad de la red de control industrial**.

<p align="center">
  <img src="/assets/images/architecture/plant-to-internet-data-flow.png" alt="Flujo de datos desde la planta hasta la capa de monitorización conectada a internet" width="760">
</p>
<p align="center"><em>Figura. Arquitectura piloto de comunicaciones desde la adquisición de datos en planta hasta la capa cloud de monitorización.</em></p>

### Comunicaciones en el lado de planta

En la planta, un **PLC** central recopila las señales de los sensores y realiza un tratamiento básico a nivel de proceso.

Para transportar la información desde la planta de Gorbea hacia el entorno de control, el sistema utiliza un **radioenlace** basado en el **protocolo industrial Modbus**.

Esto significa que los datos de planta permanecen dentro de una lógica de comunicaciones industriales antes de llegar a la siguiente etapa arquitectónica.

### Lógica de gateway en el centro de control

Una vez que la señal alcanza las **instalaciones de control de Araka**, se distribuye hacia:

* el entorno principal de **SCADA**
* un segundo **PLC** que actúa como elemento gateway

Este segundo PLC desempeña un papel arquitectónico clave porque ayuda a separar el control operativo del pipeline de apoyo con IA.

### Aislamiento orientado a ciberseguridad

Para evitar exponer directamente el entorno de control industrial a componentes conectados a internet, la arquitectura introduce un paso adicional de aislamiento.

El PLC gateway se conecta a un **microcontrolador ESP32** mediante un **enlace serie RS232 configurado como estrictamente unidireccional**.

Esta es una decisión de diseño muy potente desde la perspectiva de ciberseguridad:

* los datos pueden salir hacia la rama de apoyo con IA
* los sistemas externos no pueden enviar comandos de vuelta a la capa de control industrial a través del mismo camino

El ESP32 es el único componente de esta rama que dispone de conectividad a internet, y su papel es intencionadamente mínimo: reenviar la información de planta de forma segura hacia la capa cloud.

---

## 4. Capa de datos externos y cloud

La arquitectura no se basa únicamente en datos de planta.

Como la turbidez está fuertemente influida por la lluvia y por las condiciones ambientales, el sistema incorpora también **información meteorológica**.

Para este caso de estudio, la fuente meteorológica se integra junto con los datos de planta para que ambas corrientes alimenten después el flujo predictivo.

### ThingSpeak como puente cloud para el piloto

Una de las decisiones prácticas centrales de la arquitectura es el uso de **ThingSpeak** como plataforma cloud IoT ligera.

ThingSpeak cumple dos papeles complementarios:

1. actúa como puente accesible vía web para intercambiar información durante la fase piloto
2. actúa como historiador online ligero para la monitorización de predicciones

Esto resulta especialmente útil en un piloto orientado a portfolio porque proporciona:

* una vía rápida de implementación
* comunicación simple basada en REST
* capacidad de visualización online
* un entorno de validación con baja fricción

Por tanto, la arquitectura favorece en esta etapa la simplicidad y la observabilidad frente a la complejidad de escala empresarial.

---

## 5. Capa de persistencia de datos

La estrategia de persistencia es deliberadamente simple y adaptada al alcance piloto.

En lugar de desplegar desde el primer día un historiador empresarial completo y una pila de data lake, el sistema utiliza el **propio PC de despliegue como nodo principal de persistencia local**.

### Persistencia local

En el PC de despliegue, los datos y artefactos se almacenan como ficheros planos, principalmente:

* registros brutos y periódicamente descargados desde las APIs de planta y meteorología
* el fichero del modelo entrenado
* el scaler ajustado utilizado en inferencia
* salidas derivadas necesarias para reproducibilidad e inspección

Este enfoque de almacenamiento local es práctico para un prototipo porque mantiene la arquitectura ligera y, al mismo tiempo, preserva los elementos clave necesarios para:

* reproducibilidad
* auditabilidad
* preparación para reentrenamiento
* depuración

### Historial online de predicciones

La monitorización continua de predicciones se gestiona en la nube y no solo en disco local.

Después de cada ciclo de predicción, el PC de despliegue envía a ThingSpeak los siguientes valores:

* turbidez predicha
* turbidez real
* error absoluto de predicción

En la práctica, esto significa que la plataforma actúa también como historiador online ligero para la evaluación piloto.

---

## 6. Capa de preprocesado de datos

La arquitectura incluye una etapa dedicada de preprocesado entre la adquisición de datos brutos y la ejecución del modelo.

Esto es necesario porque las dos fuentes principales de entrada no llegan ni en el mismo formato ni con la misma frecuencia:

* los registros SCADA se manejan con **resolución horaria**
* los registros meteorológicos están disponibles cada **diez minutos**

Para poder utilizarlos conjuntamente, el pipeline:

* agrega la serie meteorológica a resolución horaria
* alinea ambas fuentes sobre una estructura temporal compartida
* las fusiona en un único dataset unificado
* elimina registros incompletos o claramente erróneos
* reduce el efecto de distorsiones extremas cuando está justificado
* prepara las variables requeridas por la familia de modelos seleccionada

La ingeniería de variables adicional también forma parte de esta capa, incluyendo:

* valores retardados de turbidez
* representaciones cíclicas del tiempo como hora del día y mes del año cuando resultan útiles
* escalado numérico para mejorar la estabilidad de las redes neuronales

Desde el punto de vista arquitectónico, este bloque de preprocesado es esencial porque transforma registros brutos y heterogéneos en un dataset operativo listo para predicción.

---

## 7. Capa de ejecución del modelo

El modelo predictivo se ejecuta en un **PC instalado en el centro de control de AMVISA**.

Esta máquina es el nodo central de ejecución de la arquitectura piloto.

Sus responsabilidades incluyen:

* obtener información de planta y meteorológica
* ejecutar la lógica de preprocesado
* cargar en memoria el modelo entrenado y el scaler
* generar predicciones de turbidez
* comparar valores predichos y observados
* enviar las salidas a la plataforma de monitorización

Este diseño centraliza la carga de trabajo de IA en un entorno de computación estándar en lugar de intentar ejecutar toda la inferencia en un dispositivo embebido limitado.

Esta es una decisión muy sólida para una fase piloto porque mantiene la arquitectura más mantenible y más fácil de depurar.

---

## 8. Arquitectura de entrenamiento

La arquitectura incluye también una lógica de entrenamiento separada de la inferencia en vivo.

El flujo de entrenamiento se implementa como un **script reproducible único en Python** que:

* carga el dataset unificado
* realiza el escalado y la partición cronológica del dataset
* construye la arquitectura candidata seleccionada
* aplica optimización de hiperparámetros
* entrena el modelo con monitorización de validación y early stopping
* evalúa el rendimiento final sobre el conjunto de test
* almacena el modelo entrenado y los artefactos de preprocesado

Esto es importante porque muestra que la arquitectura del proyecto no trata solo de inferencia en tiempo de ejecución, sino también de cómo se producen, seleccionan y preparan los modelos para su despliegue.

Los artefactos resultantes incluyen:

* el modelo final entrenado
* el scaler Min-Max ajustado
* gráficas de evaluación
* curvas de pérdida
* salidas de interpretabilidad cuando son relevantes

---

## 9. Capa de visualización orientada a operador

La arquitectura está diseñada para exponer el comportamiento del modelo de una forma útil para ingenieros y operadores.

Durante la fase piloto, el sistema envía tres valores principales al entorno de dashboard:

* turbidez predicha
* turbidez real
* error absoluto

Esto permite que la interfaz de monitorización superponga predicción y realidad en una única vista.

Ese diseño es intencionadamente simple pero muy valioso, porque permite al personal técnico evaluar:

* si el modelo sigue la dinámica real de la planta
* si el momento de los picos se capta correctamente
* si los errores crecen con el tiempo
* si empieza a aparecer deriva de rendimiento

Esta capa de monitorización es una de las partes más prácticas de la arquitectura porque convierte el modelo en algo observable en lugar de opaco.

---

## 10. Filosofía de despliegue piloto

Una característica definitoria de la arquitectura es que está construida para **validación piloto sin interferencia directa sobre el control de planta**.

En este proyecto, el modelo **no** gobierna directamente la dosificación, la filtración ni ninguna otra acción automática de la planta.

En su lugar, el despliegue sigue una lógica de **shadow mode**:

* las predicciones se generan en paralelo
* su calidad se monitoriza a lo largo del tiempo
* el modelo se evalúa en condiciones operativas reales
* la operación de planta sigue bajo los procedimientos humanos y de control ya existentes

Esta es una de las decisiones arquitectónicas más potentes del proyecto porque reduce el riesgo de despliegue y al mismo tiempo crea un camino realista hacia una futura integración.

---

## 11. Seguridad y fiabilidad operativa

Desde una perspectiva de ingeniería, uno de los aspectos más maduros de la arquitectura es el esfuerzo por mantener el sistema **desacoplado de la red de control industrial**.

Los siguientes principios definen ese enfoque:

* reutilizar comunicaciones industriales existentes siempre que sea posible
* separar las capas de control y las capas conectadas a internet
* evitar exponer directamente sistemas PLC/SCADA a plataformas externas
* usar lógica de transferencia unidireccional cuando sea apropiado
* limitar el dispositivo conectado a la nube a un papel muy reducido de reenvío

Esto muestra que la arquitectura no solo es funcional, sino también consciente de la ciberseguridad industrial y de la confianza operativa.

---

## 12. Camino hacia una futura evolución a producción

Aunque el sistema propuesto está orientado a piloto, la arquitectura sugiere de forma natural una hoja de ruta futura.

Una evolución preparada para producción podría incluir:

* versionado más sólido de modelos y artefactos
* una configuración más formal de historiador y data lake
* disparadores automáticos de reentrenamiento
* monitorización más avanzada y orquestación de alertas
* integración con flujos de notificación del lado SCADA
* dashboards con roles y políticas operativas de alertas más desarrolladas

En otras palabras, la arquitectura es intencionadamente mínima para el piloto, pero no es un callejón sin salida. Es un paso válido hacia un despliegue de IA industrial más maduro.

---

## Por qué esta arquitectura tiene valor desde la perspectiva de portfolio

Este documento es especialmente importante dentro del repositorio porque muestra que el proyecto se abordó como un **sistema completo de IA aplicada**, y no solo como experimentación en notebooks.

Demuestra la capacidad de pensar en conjunto sobre:

* infraestructura de planta
* comunicaciones
* integración consciente de la ciberseguridad
* preprocesado y persistencia
* ejecución del modelo
* monitorización y visualización
* estrategia de despliegue

Eso es precisamente lo que hace que el proyecto sea más sólido como caso técnico de portfolio.

---

## Resumen de la arquitectura

En una sola frase, la arquitectura del sistema conecta **la instrumentación existente de planta, comunicaciones seguras, integración de datos meteorológicos, inferencia centralizada del modelo y monitorización online de predicciones** dentro de una configuración segura para piloto, diseñada para validar la predicción de turbidez basada en IA sin alterar la operación de la planta.

---

## Próximos documentos

Las siguientes secciones recomendadas son:

* [`results.md`](results.md)
* [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
* [`lessons-learned.md`](lessons-learned.md)
