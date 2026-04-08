# Deployment and Monitoring

## Why deployment and monitoring matter

A forecasting model only becomes meaningful in an industrial context when it can run reliably outside the training environment and be observed under real operating conditions.

For that reason, this project was not limited to offline experimentation. It also defined and validated a **pilot-oriented deployment and monitoring strategy** for the Gorbea turbidity prediction system.

The objective was not to automate plant control immediately, but to answer a more practical question:

> Can the model operate continuously in a real utility environment, preserve its predictive usefulness over time, and be monitored safely before any tighter operational integration is considered?

This is why deployment was framed as a **shadow-mode validation stage** rather than as a production automation rollout.

---

## Deployment philosophy

The deployment strategy follows a deliberately conservative philosophy.

The system is designed to:

* run in parallel to the existing plant operation
* avoid interference with control loops
* expose predictions transparently to technical staff
* validate reliability before introducing alerts or operational actions
* generate a traceable history of model behavior in real conditions

This approach is especially appropriate for a first industrial pilot because it reduces risk while still producing meaningful evidence about practical viability.

---

## Pilot scope

The implemented deployment corresponds to a **functional pilot prototype** rather than to a full production system.

Its purpose is to validate three things:

1. **continuous technical operation**
2. **stable predictive behavior in the field**
3. **observability of the model through a simple monitoring interface**

In other words, the deployment asks whether the system can remain active, process live data correctly, and maintain acceptable real-world accuracy over time.

---

## Pilot deployment setup

The operational core of the pilot is a **dedicated PC located in the AMVISA control environment in Araka**.

This PC executes the deployed inference workflow continuously and acts as the runtime node for the turbidity prediction prototype.

The live setup is intentionally lightweight and built around a few simple components:

* a deployed trained model
* a preprocessing script aligned with the training pipeline
* access to the required data APIs
* a monitoring connection to ThingSpeak
* local storage of downloaded data and runtime artifacts

This architecture makes the pilot practical, low-cost, and easy to supervise.

---

## Model used in the pilot

Although the broader study compared several architectures and identified **TFT** as the strongest option for longer forecasting horizons, the first deployment pilot was intentionally based on an **LSTM model with a 1-hour prediction horizon**.

This was a pragmatic engineering decision.

The pilot prioritizes:

* robustness
* implementation simplicity
* operational reliability
* immediate situational awareness

rather than maximum architectural sophistication.

This distinction is important because the project explicitly separates the **best research model for long-horizon forecasting** from the **most practical model for a first field-ready deployment**.

---

## Runtime cycle

The deployed system follows a repeated operational cycle.

Each hour, the PC:

1. downloads the most recent plant and meteorological data
2. applies the same preprocessing logic used during training
3. generates a turbidity prediction with the deployed model
4. retrieves the real observed turbidity value when available
5. publishes the prediction, the real value, and the point error to the monitoring platform

This hourly cycle is important for two reasons:

* it preserves consistency between offline experimentation and live inference
* it allows the model to be evaluated under realistic continuous operation conditions

The deployed workflow therefore acts as a real-world extension of the experimental pipeline, not as an unrelated demo.

---

## Data sources during operation

During pilot operation, the live workflow combines the same two information families used in the project’s modeling stage:

* **plant-related data**, accessed through the live operational ecosystem
* **meteorological data**, retrieved from the weather source used in the solution design

The runtime pipeline keeps the same methodological principle established during development: predictions should rely on a coherent integration of **plant state** and **environmental drivers**, not on a single isolated signal.

---

## Monitoring platform

The pilot uses **ThingSpeak** as the main monitoring interface.

This choice supports the project’s pilot philosophy very well because it provides:

* a fast implementation path
* web-based accessibility
* lightweight historical storage
* simple visual inspection of model behavior over time

Rather than building a complex enterprise dashboard for the first validation stage, the project favors a simpler environment that makes the prototype easy to observe and assess.

---

## Dashboard design

The monitoring dashboard is intentionally simple and focused on the most important outputs.

<p align="center">
  <img src="/assets/images/deployment/thingspeak-dashboard.png" alt="ThingSpeak dashboard used for pilot monitoring" width="820">
</p>
<p align="center"><em>Figure. ThingSpeak dashboard used to monitor predicted turbidity, real turbidity, and point error during the pilot.</em></p>

The pilot interface includes:

* a line chart for **real turbidity**
* a line chart for **predicted turbidity**
* a line chart for **point error**
* a combined panel where **real and predicted series are overlaid**

The overlaid view is the most important panel because it makes it possible to assess, at a glance:

* whether the model follows the real plant dynamics
* whether peaks are detected with the right timing
* whether the predicted magnitude is credible
* whether performance starts to drift over time

The interface does not attempt to expose every internal feature or input variable during the pilot. The goal is to keep the visualization understandable and useful for transparent validation.

---

## What is monitored during the pilot

The monitoring strategy is intentionally narrower than what a full production MLOps environment would require.

The pilot focuses on two main validation dimensions:

### 1. Operational stability

The first requirement is that the deployed script should remain active and autonomous without critical failures for a sustained period.

In practical terms, the prototype is expected to run continuously for at least **one full week** without major runtime interruption.

### 2. Sustained predictive quality

The second requirement is that the **hourly prediction error** should remain within an acceptable range during live operation.

The key question is whether the performance observed offline is preserved when the model is exposed to a continuous real-world data stream.

This means the pilot evaluates not only whether the model can predict, but whether it can do so **consistently in production-like conditions**.

---

## Monitoring approach

The project adopts a **manual and visual monitoring strategy** for the pilot phase.

This is a conscious simplification.

### Real-time visual supervision

The first level of supervision is the dashboard itself. By comparing real values, predicted values, and point error, engineers can quickly detect obvious anomalies such as:

* missing predictions
  n- persistent divergence between curves
* abnormal spikes in the error series
* behavior that no longer matches plant dynamics

### Periodic historical review

In addition to live inspection, the project proposes a **weekly manual review** of historical charts stored in the platform.

The purpose of this review is to identify gradual degradation patterns, such as:

* systematically growing error
* reduced ability to follow peaks
* changes in model behavior linked to new plant or environmental conditions

This weekly review acts as a simplified form of performance surveillance during the pilot.

---

## Drift handling philosophy

The methodology behind the project recognizes that industrial AI systems should eventually include more formal monitoring mechanisms, such as:

* data drift detection
* concept drift detection
* statistical monitoring of performance metrics
* service-level monitoring for latency and availability

However, those mechanisms were **not implemented in full during the pilot**.

This is an important scope decision.

The pilot does **not** include automated drift detectors such as DDM or ADWIN, nor a full statistical MLOps layer. Instead, it relies on visual supervision and manual review to validate the prototype first.

That keeps the deployment aligned with its real purpose: proving viability before adding operational complexity.

---

## Metrics used during live monitoring

Unlike the offline evaluation phase, the pilot does not revolve around recalculating large summary tables of MAE, RMSE, or R² in real time.

Instead, the monitoring logic emphasizes **point-by-point operational behavior**.

The most relevant live variables are:

* the predicted turbidity value
* the real measured turbidity value
* the absolute error between both

This is a very practical decision. In a plant setting, what matters most during a pilot is often not a single global metric, but whether the model is continuing to follow the real process hour by hour.

---

## Alerts during the pilot

A defining characteristic of the deployment is that **the pilot does not activate a live operational alerting system**.

No notifications are sent during this stage, and no intervention is requested from operators as a result of the model output.

This is fully consistent with the shadow-mode philosophy:

* the pilot exists to validate the technology
* it does not yet attempt to influence operations
* it avoids introducing decision pressure before trust has been earned

So, even though the project discusses future alerting logic, the pilot deliberately keeps alerting disabled.

---

## Automatic actions during the pilot

The system also performs **no automatic control actions**.

This is true both for methodological and practical reasons.

### Methodological reason

The goal of the pilot is validation, not control.

### Practical reason

At the current stage, the Gorbea setup does not yet include the actuator layer needed for fully automatic reagent adjustment.

For that reason, the model output remains observational and evaluative only.

The system predicts turbidity and records its behavior, but all operational decisions remain with existing plant procedures and human staff.

---

## What future operational integration could look like

Although the pilot remains non-intrusive, the project clearly outlines a future direction for integration.

Once the system is proven reliable and the physical control infrastructure evolves, predictions could support or trigger:

* proactive coagulant dose adjustment
* smarter filter-washing scheduling
* early warnings for prolonged turbidity episodes
* notifications through SCADA, email, or SMS
* severity-based alert policies

In later phases, the project also contemplates tighter coupling between the forecast and a controllable dosing valve, creating a more automated decision-support loop.

This future path is important because it shows that the pilot is not isolated; it is a stepping stone toward a more operationally integrated system.

---

## Compliance and operational safety

One of the strongest aspects of the deployment design is that it preserves compliance and operational safety by construction.

Because the model runs strictly in **shadow mode**:

* it has no authority over plant control loops
* it cannot alter the water treatment process
* it cannot introduce risk into the regulatory performance of the plant

This separation is critical in a water treatment context. It ensures that the pilot can be evaluated safely without affecting the technical-sanitary requirements of the real process.

---

## Traceability

The pilot also places strong emphasis on traceability.

Even though it is a lightweight deployment, it preserves enough information to reconstruct what happened during each inference cycle.

### Input traceability

The live data downloaded for each cycle is stored locally with timestamps, allowing the project to recover the information that fed the model at a given moment.

### Output traceability

ThingSpeak stores, with time references:

* the prediction
* the real turbidity value
* the point error

### Model traceability

The deployed inference uses a specific model artifact, which makes it possible to identify which model version was active during the pilot period.

This level of traceability is essential because it enables later review of:

* what data was used
* what the model predicted
* how far prediction and reality diverged
* which deployed artifact generated the output

That is a strong foundation for any future move toward a more formal production system.

---

## What the pilot does not yet monitor formally

To keep the pilot focused and lightweight, some production-grade observability elements remain out of scope.

The prototype does **not yet** implement a full monitoring layer for:

* inference latency statistics
* CPU or memory consumption dashboards
* service-level agreements
* automatic operator feedback collection
* fully automated drift alarms
* orchestrated rollback or retraining triggers

This is not a weakness in the context of the repository. On the contrary, it makes the scope more credible because the project clearly separates:

* what was already implemented
* what belongs to a later production stage

---

## Why this deployment design is strong from a portfolio perspective

This deployment and monitoring strategy adds major value to the project because it shows that the work goes beyond model training.

It demonstrates the ability to think about:

* how a model is actually executed in the field
* how to validate it without operational risk
* how to expose it through a usable interface
* how to monitor it under real conditions
* how to preserve traceability and future integration paths

That makes the project much stronger than a purely offline forecasting study.

---

## Deployment and monitoring summary

In one sentence, the project validates an **LSTM-based shadow-mode pilot** that runs continuously on a dedicated AMVISA PC, retrieves live plant and weather data, publishes predictions and errors to ThingSpeak, and uses simple but effective visual monitoring to assess field reliability before any operational automation is introduced.

---

## Next document

The next recommended section is [`lessons-learned.md`](lessons-learned.md), which closes the project with limitations, trade-offs, and future improvements.
