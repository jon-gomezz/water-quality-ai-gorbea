# Data Pipeline

## Purpose of the data pipeline

A predictive system is only as reliable as the data workflow behind it. In this project, the data pipeline was designed to transform heterogeneous operational and environmental records into a unified dataset suitable for **turbidity forecasting**.

The main goal of the pipeline was not simply to gather data, but to make it **consistent, synchronized, interpretable, and ready for model training and future operational use**.

This is especially important in industrial AI projects, where the largest difficulties often come not from the model itself, but from the quality, structure, and integration of the data sources.

---

## Pipeline objectives

The data pipeline was designed to satisfy five main objectives:

1. **Integrate heterogeneous sources** relevant to turbidity behavior
2. **Clean and harmonize** historical records with different structures and quality conditions
3. **Align all variables in time** so they can be used consistently in forecasting tasks
4. **Prepare model-ready sequences** for different prediction horizons
5. **Preserve traceability** so results can later be interpreted, validated, and monitored

---

## Main data sources

The project combines multiple types of information because turbidity is not explained by a single variable.

### 1. SCADA operational data

The first core source is the plant’s **SCADA system**, which provides historical process records related to the treatment environment.

This type of data captures how the plant and its intake conditions evolve through time and offers the operational context needed to understand turbidity behavior.

Depending on the available instrumentation, these records may include:

* turbidity-related measurements
* intake variables
* filtration-related indicators
* process operation variables
* other plant measurements associated with water treatment conditions

SCADA data is essential because it reflects the real behavior of the plant in a time-resolved operational context.

### 2. Meteorological data

The second major source is **meteorological information**.

This source is particularly important because rainfall-driven disturbances are one of the main drivers of sudden turbidity spikes.

Meteorological variables can provide upstream information about environmental conditions before their full impact appears at the plant, which is why they are highly valuable for proactive forecasting.

Examples of useful variables include:

* precipitation
* temperature
* humidity
* atmospheric pressure
* wind-related indicators
* other weather-derived contextual variables

### 3. Contextual process information

In addition to direct measurements, the project also considers process-related and contextual variables that help describe the system state.

These variables improve prediction by providing information about how the plant is operating when environmental changes occur.

---

## Why multi-source integration is necessary

Turbidity is a dynamic variable influenced by both **external environmental factors** and **internal process conditions**.

Using only SCADA measurements would give an incomplete picture because rainfall and environmental disturbances may begin before their full effect appears at the intake.

Using only meteorological data would also be insufficient because the plant response depends on its own hydraulic and operational conditions.

The value of the data pipeline comes from combining both dimensions:

* **environmental drivers** of turbidity change
* **operational state** of the treatment system

This combination is one of the main reasons why the project is more robust than a simple univariate forecasting exercise.

---

## Main pipeline challenges

<p align="center">
  <img src="/assets/images/data/raw-turbidity-before-outlier-removal.png" alt="Raw turbidity series before outlier removal" width="860">
</p>
<p align="center"><em>Figure. Example of the raw turbidity series before preprocessing and outlier handling.</em></p>

Designing the pipeline required solving several data engineering problems.

### 1. Different temporal resolutions

The available data sources do not necessarily share the same sampling frequency.

For example, SCADA variables and meteorological series may be recorded at different intervals, which creates a synchronization challenge for forecasting.

A common time basis is therefore required before the variables can be used together.

### 2. Missing or incomplete data

Real industrial records often contain:

* missing timestamps
* incomplete rows
* sensor interruptions
* communication gaps
* partial measurement failures

These issues must be handled before model training, otherwise the model can learn distorted patterns or become unstable.

### 3. Noisy and anomalous values

Operational datasets may contain outliers, physically implausible values, or records produced by instrumentation and logging problems rather than by true process behavior.

This makes anomaly identification a necessary part of the pipeline.

### 4. Source misalignment

Even when two sources are recorded correctly, they may still be misaligned in practice because of timestamp formats, delays, aggregation conventions, or inconsistent recording windows.

### 5. Sequence preparation for forecasting

Forecasting models require structured temporal input windows rather than isolated rows.

The pipeline therefore needs to convert raw historical records into sequences that preserve temporal dependencies and support horizon-based prediction.

---

## Pipeline stages

The data pipeline can be understood as a sequence of engineering stages.

### Stage 1. Data acquisition

The first stage consists of obtaining the historical data from the relevant sources.

This includes gathering:

* plant process records from SCADA
* meteorological records from the selected weather source
* additional contextual variables when available

At this point, the main priority is not yet modeling, but ensuring that the raw material for the project is sufficiently broad and representative of the operating conditions of interest.

### Stage 2. Initial inspection and quality review

Before any transformation, the data must be inspected to understand its structure and reliability.

This review includes checking:

* available variables
* units and naming conventions
* timestamp consistency
* completeness of records
* visible anomalies
* general plausibility of the time series

This stage is critical because many downstream issues can be avoided if structural problems are detected early.

### Stage 3. Cleaning and anomaly handling

Once the data has been inspected, the next step is to reduce sources of distortion.

This stage may involve:

* removing unusable rows
* identifying clearly erroneous measurements
* reviewing suspicious values
* filtering impossible or inconsistent observations
* reducing the influence of extreme artifacts when justified

The goal is not to “beautify” the dataset artificially, but to remove records that do not reflect real process behavior.

### Stage 4. Time harmonization

A common timeline must then be created.

This is one of the central steps of the project because the predictive task depends on the temporal relationship between plant variables and weather conditions.

Time harmonization includes:

* converting timestamps into a consistent format
* aligning sources to a shared temporal grid
* deciding how to aggregate or interpolate variables when necessary
* ensuring that all observations can be interpreted on the same timeline

Without this step, multivariate forecasting would not be technically coherent.

### Stage 5. Source integration

After harmonization, the different sources are merged into a unified analytical dataset.

This integrated dataset becomes the base for model development.

At this stage, each time step contains a synchronized representation of:

* current process state
* recent environmental context
* turbidity-related measurements
* additional explanatory variables

This unified table is one of the most important outputs of the data pipeline.

### Stage 6. Feature preparation

Once the integrated dataset exists, the variables must be organized for forecasting.

This includes preparing the final feature set and ensuring that the model receives information in a meaningful structure.

Typical tasks at this stage include:

* selecting the most relevant variables
* defining target variables clearly
* preserving temporal order
* preparing lagged or window-based inputs when required
* organizing the data for multi-horizon experiments

The project does not treat this as a purely automatic step. Feature preparation is guided by both domain knowledge and forecasting logic.

### Stage 7. Train/validation/test preparation

Before model training, the data must be split in a way that respects temporal causality.

Unlike random tabular tasks, forecasting pipelines must avoid leakage from the future into the past.

For that reason, the dataset is organized into temporally coherent subsets for:

* training
* validation
* testing

This step is essential for producing realistic performance estimates.

### Stage 8. Sequence generation for forecasting models

Because the project compares sequence-oriented architectures, the final pipeline stage converts the prepared dataset into model-ready time windows.

This means constructing input-output samples where each prediction is based on a historical context window and evaluated against a future turbidity target.

This sequence generation stage allows the same unified data foundation to be used across different model families and across multiple prediction horizons.

---

## Forecast horizon preparation

A major characteristic of the project is that forecasting was evaluated at different horizons:

* **1 hour**
* **3 hours**
* **6 hours**

This has direct consequences for the data pipeline.

The target definition, sequence construction, and evaluation logic must all be adapted to the chosen horizon. A pipeline prepared for a 1-hour forecast is not identical to one prepared for a 6-hour forecast.

This is why the pipeline is not a fixed preprocessing script, but a configurable workflow capable of supporting multiple predictive scenarios.

---

## Traceability and reproducibility

Another important design goal of the pipeline is traceability.

In industrial AI, it is important to know:

* where each variable came from
* how timestamps were aligned
* what cleaning rules were applied
* what version of the prepared dataset was used
* which target and horizon definition correspond to each experiment

This is necessary not only for scientific reproducibility, but also for operational trust. If a model is later deployed or reviewed, the data preparation process must be understandable and auditable.

---

## Why the data pipeline matters in this project

From a portfolio perspective, the data pipeline is one of the strongest technical parts of the project because it shows the ability to work with **real-world, imperfect, multi-source time-series data**.

It demonstrates skills in:

* industrial data understanding
* temporal alignment
* anomaly-aware preprocessing
* multi-source integration
* forecasting-ready sequence preparation
* engineering thinking beyond model training

In many applied AI projects, this is where most of the real technical value is created.

---

## Pipeline summary

In one sentence, the data pipeline of this project transforms **heterogeneous SCADA and meteorological records** into a **clean, synchronized, traceable, and model-ready forecasting dataset** for turbidity prediction under multiple time horizons.

---

## Next documents

The next recommended sections are:

* [`modeling.md`](modeling.md)
* [`system-architecture.md`](system-architecture.md)
* [`results.md`](results.md)
* [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
