# System Architecture

## Why system architecture matters in this project

This project was not designed as an isolated machine learning experiment. From the beginning, the predictive model was framed as part of a broader **industrial decision-support system**.

That means the architecture had to answer questions beyond model accuracy:

* Where does the data come from?
* How is it moved safely?
* Where is it processed?
* How are predictions delivered to operators?
* How can the system be validated without interfering with plant operation?

For that reason, the architecture was designed as a **practical pilot-oriented solution** that connects plant data, meteorological information, predictive inference, and monitoring in a controlled and traceable way.

---

## Design goals

The architecture was designed around five main goals:

1. **Use the existing plant infrastructure as much as possible**
2. **Keep the solution technically simple enough for a pilot deployment**
3. **Protect the industrial control network from unnecessary exposure**
4. **Allow real-time or near-real-time monitoring of prediction quality**
5. **Prepare a path toward future operational integration without forcing it too early**

This is why the resulting system prioritizes modularity, security, and deployment realism over architectural complexity.

---

## High-level architecture

At a high level, the system can be understood as five connected layers:

1. **Plant sensing and operational data layer**
2. **Communications and secure data transfer layer**
3. **Data persistence and preprocessing layer**
4. **Model execution and inference layer**
5. **Visualization and decision-support layer**

The architecture links the physical reality of the treatment plant with a software pipeline capable of generating turbidity predictions and exposing them in a form suitable for pilot evaluation.

---

## Architecture in one view

The practical logic of the system is the following:

* the Gorbea plant generates operational data through its existing instrumentation and PLC-based control environment
* plant signals are transmitted securely toward the control center
* meteorological data is retrieved from an external weather source
* both sources are synchronized and prepared on a deployment PC
* the selected AI model runs on that PC
* the resulting prediction, the real turbidity value, and the prediction error are sent to an online dashboard for monitoring and validation

This architecture is intentionally conservative: it enables meaningful validation without directly closing the loop over plant control actions.

---

## 1. Physical and infrastructure layer

The proposed system is designed to operate within the real physical environment of the **Gorbea water treatment plant** and the associated AMVISA control infrastructure.

A relevant aspect of the plant layout is that the hydraulic process is distributed across key physical areas, including:

* the **spring-collection building**, where the flows from several sources converge
* the **filters and chlorination building**, where treatment equipment is located and where the future AI-support hardware and software context is concentrated

This matters architecturally because any deployable solution must adapt to:

* the existing equipment footprint
* real access constraints
* available installation space
* the current plant energy supply and operational environment

The design also assumes that the plant already has a sufficiently robust power environment for pilot deployment, which makes it unnecessary to redesign the electrical backbone from scratch.

---

## 2. Sensing and plant data layer

The architecture is built to take advantage of the **existing instrumentation already installed in the plant**.

This is an important engineering decision because it avoids introducing unnecessary hardware complexity in the pilot phase.

Instead of redesigning the sensing layer from scratch, the project leverages the current process instrumentation associated with variables such as:

* turbidity
* flow
* pressure
* other process measurements relevant to intake and filtration behavior

This makes the proposal much more realistic as a portfolio project: it reflects how industrial AI systems are often introduced in practice, by building on top of existing operational assets rather than replacing them.

---

## 3. Communications architecture

One of the most important architectural decisions in the project concerns **how plant data is moved without compromising the integrity of the industrial control network**.

### Plant-side communications

At the plant, a central **PLC** collects sensor signals and performs basic process-level handling.

To transport the information from the Gorbea plant toward the control environment, the system uses a **radio link** based on the **Modbus industrial protocol**.

This means the plant data remains within an industrial communications logic before it reaches the next architectural stage.

### Control-center gateway logic

Once the signal reaches the **Araka control facilities**, it is distributed to:

* the main **SCADA** environment
* a second **PLC** that acts as a gateway element

This second PLC plays a key architectural role because it helps separate operational control from the AI-support pipeline.

### Cybersecurity-oriented isolation

To avoid exposing the industrial control environment directly to internet-connected components, the architecture introduces an additional isolation step.

The gateway PLC is connected to an **ESP32 microcontroller** through a **serial RS232 link configured as strictly unidirectional**.

This is a strong design choice from a cybersecurity perspective:

* data can flow outward toward the AI-support path
* external systems cannot push commands back into the industrial control layer through the same path

The ESP32 is the only component in this branch that has internet connectivity, and its role is intentionally minimal: it forwards plant information securely toward the cloud layer.

---

## 4. External data and cloud layer

The architecture does not rely only on plant data.

Because turbidity is strongly influenced by rainfall and environmental conditions, the system also incorporates **meteorological information**.

For the case study, the weather source is integrated alongside plant data so that both streams can later feed the predictive workflow.

### ThingSpeak as pilot cloud bridge

A central practical choice in the architecture is the use of **ThingSpeak** as a lightweight IoT cloud platform.

ThingSpeak plays two complementary roles:

1. it acts as a web-accessible bridge for exchanging information during the pilot phase
2. it acts as a lightweight online historian for monitoring predictions

This is particularly useful in a portfolio-oriented pilot because it provides:

* a fast implementation path
* simple REST-based communication
* online visualization capability
* a low-friction validation environment

The architecture therefore favors simplicity and observability over enterprise-scale complexity at this stage.

---

## 5. Data persistence layer

The persistence strategy is deliberately simple and adapted to pilot scope.

Rather than deploying a full enterprise historian and data lake stack from day one, the system uses the **deployment PC itself as the main local persistence node**.

### Local persistence

On the deployment PC, data and artifacts are stored as flat files, mainly:

* raw and periodically downloaded records from plant and weather APIs
* the trained model file
* the fitted scaler used in inference
* derived outputs needed for reproducibility and inspection

This local storage approach is practical for a prototype because it keeps the architecture lightweight while still preserving the key elements required for:

* reproducibility
* auditability
* retraining readiness
* debugging

### Online prediction history

Continuous prediction monitoring is handled in the cloud rather than only on local disk.

After each prediction cycle, the deployment PC sends the following values to ThingSpeak:

* predicted turbidity
* real turbidity
* absolute prediction error

In practice, this means the platform also acts as a lightweight online historian for pilot evaluation.

---

## 6. Data preprocessing architecture

The architecture includes a dedicated preprocessing stage between raw data acquisition and model execution.

This is necessary because the two main input sources do not arrive in the same format or frequency:

* SCADA records are handled at **hourly resolution**
* meteorological records are available every **ten minutes**

To make them usable together, the pipeline:

* aggregates the weather series to hourly resolution
* aligns both sources to a shared timestamp structure
* merges them into one unified dataset
* removes incomplete or clearly erroneous records
* reduces the effect of extreme distortions when justified
* prepares the features required by the selected model family

Additional feature engineering is also part of this layer, including:

* lagged turbidity values
* cyclic time representations such as hour-of-day and month-of-year when useful
* numerical scaling for neural network stability

Architecturally, this preprocessing block is essential because it transforms raw heterogeneous records into a prediction-ready operational dataset.

---

## 7. Model execution layer

The predictive model runs on a **PC installed in the AMVISA control center**.

This machine is the core execution node of the pilot architecture.

Its responsibilities include:

* retrieving plant and meteorological information
* executing the preprocessing logic
* loading the trained model and scaler into memory
* generating turbidity predictions
* comparing predicted and observed values
* sending outputs to the monitoring platform

This design centralizes the AI workload in a standard computing environment rather than trying to run the full inference workflow on a constrained embedded device.

That is a strong pilot-stage decision because it keeps the architecture maintainable and easier to debug.

---

## 8. Training architecture

The architecture also includes a separate training logic, distinct from live inference.

The training workflow is implemented as a **single reproducible Python script** that:

* loads the unified dataset
* performs scaling and chronological dataset splitting
* builds the selected candidate architecture
* applies hyperparameter optimization
* trains the model with validation monitoring and early stopping
* evaluates final performance on the test set
* stores the trained model and preprocessing artifacts

This is important because it shows that the project architecture is not only about runtime inference, but also about how models are produced, selected, and prepared for deployment.

The resulting artifacts include:

* the final trained model
* the fitted Min-Max scaler
* evaluation plots
* loss curves
* interpretability outputs where relevant

---

## 9. Visualization and operator-facing layer

The architecture is designed to expose model behavior in a way that is usable for engineers and operators.

During the pilot phase, the system sends three main values to the dashboard environment:

* predicted turbidity
* real turbidity
* absolute error

This allows the monitoring interface to superimpose prediction and reality in a single view.

That design is intentionally simple but highly valuable, because it lets technical staff evaluate:

* whether the model follows the real plant dynamics
* whether peak timing is captured correctly
* whether errors grow over time
* whether performance drift begins to appear

This monitoring layer is one of the most practical parts of the architecture because it turns the model into something observable rather than opaque.

---

## 10. Pilot deployment philosophy

A defining feature of the architecture is that it is built for **pilot validation without direct interference in plant control**.

In this project, the model does **not** directly command dosing, filtration, or any automatic plant action.

Instead, the deployment follows a **shadow-mode logic**:

* predictions are generated in parallel
* their quality is monitored over time
* the model is evaluated in real operational conditions
* plant operation remains under existing human and control-system procedures

This is one of the strongest architectural choices in the project because it reduces deployment risk while still creating a realistic path toward future integration.

---

## 11. Security and operational reliability

From an engineering perspective, one of the most mature aspects of the architecture is the effort to keep the system **decoupled from the industrial control network**.

The following principles define that approach:

* reuse existing industrial communications where possible
* separate control and internet-connected layers
* avoid exposing PLC/SCADA systems directly to external platforms
* use one-way transfer logic where appropriate
* keep the cloud-connected device limited to a narrow forwarding role

This shows that the architecture is not only functional, but also mindful of industrial cybersecurity and operational trust.

---

## 12. Path toward future production evolution

Although the proposed system is pilot-oriented, the architecture naturally suggests a future roadmap.

A production-ready evolution could include:

* stronger model and artifact versioning
* a more formal historian and data lake setup
* automated retraining triggers
* more advanced monitoring and alert orchestration
* integration with SCADA-side notification workflows
* role-based dashboards and operational alert policies

In other words, the architecture is intentionally minimal for the pilot, but it is not a dead end. It is a valid stepping stone toward a more mature industrial AI deployment.

---

## Why this architecture is valuable from a portfolio perspective

This document is especially important in the repository because it shows that the project was approached as a **full applied AI system**, not just as notebook-based experimentation.

It demonstrates the ability to think across:

* plant infrastructure
* communications
* cybersecurity-aware integration
* preprocessing and persistence
* model execution
* monitoring and visualization
* deployment strategy

That is exactly what makes the project stronger as a technical portfolio case.

---

## Architecture summary

In one sentence, the system architecture connects **existing plant instrumentation, secure communications, weather data integration, centralized model inference, and online prediction monitoring** in a pilot-safe setup designed to validate AI-based turbidity forecasting without disrupting plant operation.

---

## Next documents

The next recommended sections are:

* [`results.md`](results.md)
* [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
* [`lessons-learned.md`](lessons-learned.md)
