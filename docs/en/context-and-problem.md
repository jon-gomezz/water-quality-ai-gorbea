# Context and Problem

## Industrial and environmental context

Safe drinking water treatment depends on the ability to maintain stable process conditions even when raw water quality changes rapidly. Among the variables that most directly affect treatment performance, **turbidity** is one of the most critical.

Turbidity is an indirect measure of suspended particles in water, such as clays, silts, organic matter, algae, or metallic particles. In operational terms, high turbidity is not just a water quality indicator; it is a direct source of treatment difficulty. When turbidity rises, coagulation becomes harder to control, filtration performance can deteriorate, and disinfection efficiency may be affected.

This makes turbidity especially relevant in drinking water treatment plants that operate under highly variable hydrological conditions.

---

## Why turbidity matters operationally

In a real plant, turbidity is not an abstract laboratory metric. It has immediate consequences for daily operation.

When raw water turbidity increases sharply:

* coagulant dosage often needs to be adjusted quickly
* filters may require more frequent washing
* process stability can deteriorate
* chemical and energy consumption can rise
* operators have less time to react safely and efficiently

From an engineering point of view, this means turbidity is tightly connected to both **water quality compliance** and **operational efficiency**.

It also has a public health dimension. If turbidity is not properly controlled, it can compromise treatment effectiveness and reduce the safety margin of downstream disinfection processes.

---

## Local case study: Gorbea water treatment plant

This project is framed around the **Gorbea water treatment plant**, operated by **AMVISA (Aguas Municipales de Vitoria-Gasteiz)**.

The Gorbea system receives water from a complex catchment context that includes multiple natural spring sources in the Gorbea massif, together with related hydrological infrastructure in the surrounding area. The project focuses on predicting **raw water turbidity at the plant intake**, just before water enters the treatment process.

This point is especially relevant because it reflects the quality of the incoming resource that the plant must condition in real time.

A key challenge in this environment is that turbidity can change very quickly during heavy rainfall episodes. In the Gorbea case, raw water turbidity may move from low baseline levels to severe peaks within a short time window, creating pressure on the plant’s treatment processes and on the operators responsible for keeping the system stable.

<p align="center">
  <img src="/assets/images/overview/water-origin-scheme.png" alt="Water origin scheme feeding the Gorbea treatment plant" width="760">
</p>
<p align="center"><em>Figure. Simplified scheme of the water sources feeding the Gorbea plant.</em></p>

---

## Climate variability and operational pressure

The problem is further intensified by the growing variability of rainfall events.

More intense storm episodes increase the probability of sudden sediment mobilization, which in turn drives abrupt turbidity spikes. In this type of context, reacting only after the event is detected is often not enough. By the time turbidity rises clearly at the plant, the available response window may already be too short for optimal process adjustment.

This is why the problem is not only about measuring turbidity accurately, but about **anticipating it early enough to support proactive decisions**.

---

## AMVISA digitalization context

Another important part of the context is the broader digital transformation of the water utility.

The case study sits within a modernization environment where water infrastructure is becoming increasingly instrumented and data-driven. SCADA systems, continuous monitoring, communications networks, and digital platforms are creating the conditions for more advanced decision-support tools.

In this setting, the project is not an isolated AI experiment. It fits into a larger transition toward:

* better operational visibility
* stronger real-time monitoring
* predictive rather than purely reactive management
* more integrated use of process and environmental data

This makes the Gorbea case especially suitable for a portfolio project focused on **applied industrial AI**.

---

## The core problem

At first glance, the challenge may look simple: predict future turbidity values.

In practice, the real problem is broader.

The main issue is not only the absence of a predictive model, but the lack of a **systematic and reusable methodology** for designing, validating, and preparing AI-based water quality prediction systems for real industrial use.

Many predictive projects fail to move beyond the experimental stage because they focus only on model training while underestimating the rest of the engineering workflow.

In this case, the project addresses a more complete question:

> How can an AI-based turbidity prediction system be designed in a way that is technically reliable, operationally useful, and realistically deployable in a drinking water treatment environment?

---

## Why the problem is difficult

Several factors make this problem non-trivial.

<p align="center">
  <img src="/assets/images/data/raw-turbidity-before-outlier-removal.png" alt="Raw turbidity signal before outlier removal" width="860">
</p>
<p align="center"><em>Figure. Raw turbidity signal before outlier handling, illustrating the presence of spikes and irregular behavior.</em></p>

### 1. Heterogeneous data sources

Reliable prediction requires combining different types of information, including:

* plant operational records from the SCADA system
* meteorological variables
* process-related variables linked to filtration and intake behavior

These sources do not naturally arrive in a clean, unified format. They may have different time resolutions, missing values, noisy measurements, and synchronization issues.

### 2. Strong temporal dynamics

Turbidity behavior is highly time-dependent. Rainfall effects may appear with delays, process responses may unfold over several hours, and different patterns may dominate at different forecasting horizons.

This means the solution must capture both short-term dynamics and longer-range dependencies.

### 3. Model lifecycle issues

Even a model that performs well in offline experiments may degrade over time if operating conditions evolve. Changes in environmental patterns, plant behavior, or data quality can reduce predictive reliability.

So the challenge is not only model selection, but also monitoring, updating, and maintaining the system through time.

### 4. Operational usability

A mathematically accurate model is not enough if the plant staff cannot trust it or use it effectively.

To be useful in practice, the system must produce outputs that are:

* timely
* understandable
* actionable
* aligned with real operational decisions

In other words, the project must bridge the gap between **data science performance** and **plant-floor usefulness**.

---

## Why prediction is valuable

If turbidity can be forecast with enough anticipation, operators can respond before the event becomes critical.

That creates several potential benefits:

* more proactive coagulant adjustment
* better filter management
* lower risk of unnecessary overdosing
* improved process stability during rainfall-driven events
* stronger support for water quality compliance

This is why the project should be understood as a **decision-support system**, not only as a forecasting exercise.

---

## Problem statement for the portfolio project

This repository presents the problem in the following way:

A drinking water treatment plant operating under variable environmental conditions needs a reliable way to anticipate raw water turbidity spikes before they become an operational risk. The challenge is not only to predict the variable itself, but to develop a reproducible AI methodology capable of integrating heterogeneous data, comparing suitable forecasting architectures, and preparing the solution for realistic industrial deployment.

---

## Why this problem makes a strong technical case

From a portfolio perspective, this problem is valuable because it combines:

* a real critical-infrastructure use case
* measurable operational consequences
* multivariate time-series forecasting
* environmental and industrial data integration
* deployment-oriented engineering thinking
* practical decision-support value

That makes it a strong example of how AI can be applied beyond laboratory benchmarks, in a context where reliability, interpretability, and operational usefulness matter as much as raw predictive accuracy.

---

## Next document

The next recommended section is [`methodology.md`](methodology.md), which explains how the project translates this problem into a structured AI workflow.
