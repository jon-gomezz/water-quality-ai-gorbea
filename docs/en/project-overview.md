# Project Overview

## Executive summary

This project presents an **AI-based methodology for predicting raw water turbidity** in a real drinking water treatment context. The work was developed around the **Gorbea water treatment plant**, operated by **AMVISA (Aguas Municipales de Vitoria-Gasteiz)**, where sudden turbidity spikes can occur within a short time during intense rainfall events.

The project goes beyond building a single predictive model. Its main contribution is the design and validation of a **systematic, reusable methodology** for developing, evaluating, and preparing the deployment of AI solutions for water quality prediction in industrial environments.

The methodology was applied to a real case study by integrating **historical SCADA plant data** with **meteorological variables**, comparing multiple deep learning architectures, and validating a functional prototype for future pilot deployment.

---

## Why the project was needed

Turbidity is one of the most operationally sensitive variables in drinking water treatment.

At the Gorbea plant, raw water turbidity can move rapidly from low values to severe peaks during storm events. These episodes create direct operational consequences:

* increased coagulant consumption
* higher filter washing frequency
* lower process stability
* reduced reaction time for operators
* increased risk of non-compliance if the event is not managed properly

In practice, this means that a purely reactive strategy is often insufficient. Plant operators gain much more value from a system capable of **anticipating the event in advance**, so they can adapt dosing, filtration, and operational decisions before water quality deteriorates.

---

## Main project objective

The main objective was to create a **replicable engineering framework** for AI-based water quality prediction and to demonstrate its practical value through a real industrial case.

In this project, that meant:

* defining a complete methodological workflow
* applying it to turbidity prediction at the Gorbea plant
* identifying the most suitable model for different forecasting horizons
* validating the solution as a realistic prototype for future industrial use

So the project has a dual purpose:

1. solve a relevant predictive problem in a real plant
2. provide a methodology that can be reused in future digitalization initiatives

---

## Scope of the work

The project was structured as an engineering case study, not as a purely academic modeling exercise.

It included:

* definition of objectives and operational requirements
* identification of relevant variables to measure and integrate
* combination of SCADA process data with meteorological information
* preprocessing and synchronization of heterogeneous data sources
* comparative study of multiple neural network architectures
* evaluation across several prediction horizons
* validation of a functional prototype
* design considerations for pilot deployment, operation, and monitoring

The project was therefore aimed at **deployment readiness**, even though it remained at prototype level and did not involve full long-term production operation.

---

## Technical solution in one view

The technical logic of the project can be summarized as follows:

1. collect and prepare historical operational and environmental data
2. define a robust prediction target related to turbidity behavior
3. train and compare several sequence modeling approaches
4. evaluate both predictive performance and operational usefulness
5. propose a deployable prototype aligned with plant realities

This makes the project closer to an **industrial AI workflow** than to a standalone machine learning benchmark.

---

## Models evaluated

A comparative study was carried out using five neural network families:

* **MLP**
* **LSTM**
* **GRU**
* **TCN**
* **Temporal Fusion Transformer (TFT)**

The comparison was performed across three forecasting horizons:

* **1 hour**
* **3 hours**
* **6 hours**

This multi-horizon setup was important because operational value changes with time horizon. A short-range forecast may support immediate situational awareness, while a longer forecast is more useful for proactive process adjustment.

---

## Main result

The main result of the case study is that **forecasting horizon strongly influences model suitability**.

The project showed that:

* all models can provide useful short-term predictions
* model differences become more important as the horizon increases
* the **Temporal Fusion Transformer (TFT)** performed best for longer forecasting horizons
* an **LSTM-based approach** offers a strong practical balance for an initial pilot deployment thanks to its reliability and relative implementation simplicity

This is one of the most relevant engineering conclusions of the project: the best model is not only the one with the best metric, but the one that best fits the operational purpose and deployment constraints.

---

## Practical value

The practical value of the project lies in its potential to support:

* earlier response to turbidity events
* more efficient use of coagulants
* improved filtration management
* better operational planning during rainfall-driven disturbances
* stronger digital decision support for plant operators

More broadly, the project demonstrates how AI can be integrated into critical infrastructure environments in a structured and realistic way.

---

## What makes this a strong portfolio project

This project is especially relevant as a technical portfolio case because it combines:

* a **real industrial problem**
* **time-series forecasting** with operational consequences
* **heterogeneous data integration**
* **deep learning model comparison**
* **prototype validation**
* **deployment-oriented engineering thinking**

It shows not only model development skills, but also the ability to frame an applied AI problem end to end: from context understanding and data preparation to architecture choices, validation, and pilot planning.

---

## Key takeaways

* The project is centered on **predicting turbidity before it becomes an operational problem**.
* Its main contribution is a **reusable AI methodology**, not only a single trained model.
* The solution integrates **SCADA data + meteorological variables** in a real plant context.
* Different model families were compared under **1 h, 3 h, and 6 h** forecasting scenarios.
* **TFT** delivered the strongest long-horizon performance, while **LSTM** emerged as a practical pilot-ready option.
* The project demonstrates the viability of **AI as an operational support tool** for water treatment digitalization.

---

## Next documents

To continue exploring the project, the recommended next sections are:

* [`context-and-problem.md`](context-and-problem.md)
* [`methodology.md`](methodology.md)
* [`data-pipeline.md`](data-pipeline.md)
* [`modeling.md`](modeling.md)
* [`results.md`](results.md)
