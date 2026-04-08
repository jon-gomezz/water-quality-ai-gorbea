# AI-Based Water Quality Prediction for the Gorbea Water Treatment Plant

## Overview

This repository documents a technical portfolio project focused on **predicting raw water turbidity using artificial intelligence** in a real drinking water treatment context.

The work is based on a case study carried out with **AMVISA (Aguas Municipales de Vitoria-Gasteiz)** and centers on the **Gorbea water treatment plant**, where sudden turbidity spikes can compromise process stability, increase chemical consumption, and make plant operation more difficult during intense rainfall events.

Rather than presenting the work as a traditional academic thesis, this repository reframes it as a **portfolio-ready engineering project** focused on:

* problem understanding
* methodology design
* data integration
* model development
* validation
* deployment readiness

<p align="center">
  <img src="/assets/images/overview/filters-gorbea.png" alt="Filters at the Gorbea water treatment plant" width="820">
</p>
<p align="center"><em>Figure. Filters at the Gorbea water treatment plant.</em></p>

---

## The problem

Water treatment plants operate under changing environmental and operational conditions. In the Gorbea case, turbidity at the plant intake can rise sharply in a short period of time, especially during heavy rainfall episodes.

These events create several operational challenges:

* increased uncertainty in plant operation
* higher coagulant consumption
* more frequent filter washing
* reduced process stability
* greater risk of regulatory non-compliance

A reactive response is often not enough. The real value comes from being able to **anticipate turbidity events early enough to support proactive decision-making**.

---

## Project goal

The main goal of the project was not just to train a model, but to **design, document, and validate a reusable AI methodology** for water quality prediction in industrial environments.

This methodology was then applied to a real case study:

> **predicting turbidity at the intake of the Gorbea treatment plant using historical SCADA data and meteorological variables**.

The project therefore has two complementary outputs:

1. a **practical predictive solution** for the Gorbea case
2. a **replicable methodological framework** for future water digitalization projects

---

## Why this project is relevant

This is a strong portfolio project because it combines several high-value engineering dimensions:

* **real industrial context**
* **time-series forecasting**
* **multi-source data integration**
* **deep learning for operational decision support**
* **prototype validation**
* **deployment-oriented thinking**

It is not only a machine learning exercise. It is also a systems-thinking project involving data pipelines, infrastructure considerations, monitoring, and operational usability.

---

## Case study context

The project was developed around the digitalization context of **AMVISA**, the public water utility of Vitoria-Gasteiz.

At the Gorbea treatment plant, raw water turbidity can move from low baseline values to very high levels within hours during storm events. These episodes affect filtration, coagulant dosage, and general plant stability.

This makes turbidity forecasting especially useful as an operational support tool.

The project also aligns with a broader utility modernization context, where SCADA systems, environmental sensing, communications infrastructure, and predictive analytics are becoming part of the same digital ecosystem.

---

## Technical approach

The project was structured as a full methodological pipeline.

<p align="center">
  <img src="/assets/images/methodology/methodology-phases.png" alt="Methodology phases for the AI-based water quality prediction framework" width="760">
</p>
<p align="center"><em>Figure. High-level phases of the proposed AI methodology.</em></p>

### 1. Objective and requirement definition

The first stage focused on understanding:

* which variables should be measured
* which operational decisions could benefit from prediction
* which prediction horizons were most useful
* which system constraints would affect a future pilot deployment

### 2. Data integration

The predictive workflow combines heterogeneous information sources, mainly:

* **historical SCADA operational data**
* **meteorological variables**
* process-related plant variables linked to filtration and intake conditions

A major part of the work involved handling:

* different temporal granularities
* missing values
* anomalous records
* synchronization across sources
* preprocessing for training readiness

### 3. Model comparison

The study compared five neural network families:

* **MLP**
* **LSTM**
* **GRU**
* **TCN**
* **Temporal Fusion Transformer (TFT)**

The comparison was carried out for different forecasting horizons:

* **1 hour**
* **3 hours**
* **6 hours**

This was important because the most useful model is not always the same for every horizon.

### 4. Evaluation and validation

The models were evaluated quantitatively using regression metrics and then analyzed from an operational point of view.

The project also went beyond offline experimentation by considering:

* functional validation
* prototype logic
* pilot deployment readiness
* alerting and interface implications
* model monitoring and lifecycle management

---

## Main findings

The project showed that **forecasting horizon matters significantly**.

* For **short-term forecasting**, several architectures achieved robust performance.
* For **longer-term proactive forecasting**, more advanced architectures provided clearer benefits.
* The **Temporal Fusion Transformer (TFT)** showed the strongest overall performance for longer horizons.
* For an initial pilot-oriented deployment, **LSTM** was identified as a strong practical option because of its balance between reliability and implementation simplicity.

This distinction is important from an engineering perspective: the “best model” is not only the one with the strongest raw metric, but the one that best fits the intended operational use case.

---

## Engineering value of the project

Beyond the specific case study, this project demonstrates experience in:

* translating a real industrial problem into an AI workflow
* designing a structured methodology instead of an isolated experiment
* building a bridge between data science and operations
* comparing architectures for sequence modeling
* thinking about deployment, monitoring, and maintainability from the beginning

This makes the project relevant not only for water sector applications, but also for broader **industrial AI**, **predictive analytics**, and **time-series decision-support systems**.

---

## Repository structure

This repository is organized as a bilingual technical portfolio.

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
```

---

## Recommended reading order

To understand the project quickly, the best order is:

1. [`project-overview.md`](project-overview.md)
2. [`context-and-problem.md`](context-and-problem.md)
3. [`methodology.md`](methodology.md)
4. [`data-pipeline.md`](data-pipeline.md)
5. [`modeling.md`](modeling.md)
6. [`system-architecture.md`](system-architecture.md)
7. [`results.md`](results.md)
8. [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
9. [`lessons-learned.md`](lessons-learned.md)

---

## Scope note

This repository is intended as a **technical showcase**.

For clarity and confidentiality reasons:

* the project is presented in a **portfolio format**, not as a chapter-by-chapter thesis copy
* the emphasis is placed on **engineering logic, methodology, architecture, and results interpretation**
* sensitive industrial data and production-specific implementation details are not necessarily included in full

---

## Intended audience

This repository is especially relevant for:

* recruiters evaluating technical portfolio projects
* engineers interested in industrial AI applications
* data professionals working on time-series forecasting
* utilities and infrastructure teams exploring predictive analytics

---

## Author

**Jon Gomez Olarte**
Mechanical Engineer | AI-focused technical portfolio
