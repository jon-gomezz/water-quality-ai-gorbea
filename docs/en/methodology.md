# Methodology

## Purpose of the methodology

The core contribution of this project is not only a trained turbidity prediction model, but a **structured methodology** for designing, validating, and preparing the deployment of AI-based water quality prediction systems in real industrial environments.

The methodology was conceived as a **reusable engineering framework**. In other words, the goal was to define a workflow that could be applied beyond the specific Gorbea case, adapting it to other plants, other water quality variables, or other predictive use cases within utility digitalization programs.

This is why the project was organized as a full lifecycle methodology rather than as a narrow machine learning experiment.

---

## Methodological principles

The methodology is built around several practical principles.

### 1. End-to-end engineering focus

The project treats prediction as only one part of the system. A useful industrial AI solution must also address:

* data acquisition
* preprocessing
* model selection
* validation
* operational integration
* monitoring
* maintenance

### 2. Reusability

The framework is intended to be transferable. The aim is not to solve only one isolated plant problem, but to define a repeatable process that reduces the need to redesign the entire workflow from scratch in future projects.

### 3. Operational usefulness over benchmark thinking

The methodology does not optimize only for offline metrics. It also considers whether the resulting system can support real operational decisions, whether it can be integrated into existing workflows, and whether plant staff could realistically trust and use it.

### 4. Lifecycle awareness

A model is not treated as a static artifact. The methodology assumes that environmental conditions, plant behavior, and data distributions can evolve over time, so monitoring, maintenance, and model lifecycle management are considered part of the design from the beginning.

---

## High-level workflow

The methodology is organized into five major phases:

1. **Definition of objectives and requirements**
2. **System design**
3. **Implementation and validation**
4. **Operation strategy**
5. **Maintenance and lifecycle management**

This structure makes the framework suitable for both experimental development and pilot-oriented industrial preparation.

---

## Phase 1. Definition of objectives and requirements

The first phase establishes what the system must achieve and under which constraints it should operate.

### 1.1 Problem framing

The methodology begins by translating the real operational problem into a prediction task.

In the Gorbea case, this meant defining the central challenge as:

> anticipating raw water turbidity at the plant intake early enough to support proactive operational decisions.

This step is important because it aligns the AI task with the actual plant need rather than with an abstract modeling objective.

### 1.2 Identification of relevant variables

A prediction system is only as good as the information it receives. For that reason, the methodology includes an early analysis of which variables should be measured and integrated.

For this case, the relevant information includes:

* turbidity-related plant variables
* process variables from the SCADA system
* filtration-related operational indicators
* meteorological variables
* hydrological context variables when relevant

The goal is to identify a set of signals that can explain both the current plant state and the upstream conditions that may drive future turbidity changes.

### 1.3 Forecast horizon definition

Prediction value depends strongly on time horizon. A 1-hour prediction and a 6-hour prediction do not serve the same operational purpose.

The methodology therefore defines prediction horizons explicitly rather than treating forecasting as a single generic task.

In this project, the selected horizons were:

* **1 hour**
* **3 hours**
* **6 hours**

This made it possible to evaluate which models were better for immediate situational awareness and which were better for more proactive anticipation.

### 1.4 Economic and sustainability considerations

The methodology also includes an early reflection on practical value.

This means asking questions such as:

* can the system reduce unnecessary coagulant overdosing?
* can it improve filtration management?
* can it reduce operational inefficiencies during extreme events?
* is the expected value of prediction consistent with deployment effort?

This step helps ensure that the project remains tied to real engineering benefit.

---

## Phase 2. System design

Once the objectives are clear, the next phase defines the architecture of the solution.

This phase is broader than model design. It includes infrastructure, software logic, data workflow, and hardware considerations.

### 2.1 Infrastructure analysis

Before building the predictive workflow, the methodology requires understanding the real deployment context.

This includes:

* site characteristics
* risk conditions
* existing instrumentation
* data availability
* communications constraints
* energy and computing availability

In portfolio terms, this is one of the most important aspects of the methodology because it shows that the system is designed with the real environment in mind.

### 2.2 Software architecture

The software layer is designed around the complete data and inference workflow.

Its main components are:

#### Communications with decision centers

The methodology defines how information should move between the prediction system and the operational decision environment.

In the Gorbea case, the proposed logic connects prediction outputs with AMVISA’s control context through a lightweight digital architecture oriented toward web-based information exchange and pilot evaluation.

#### Data preprocessing

Preprocessing is a central methodological block.

The project integrates **SCADA operational records** and **meteorological series**, which do not naturally share the same temporal resolution or quality conditions. The methodology therefore defines a cleaning and harmonization process that includes:

* time alignment across sources
* removal of incomplete rows when necessary
* detection of clearly erroneous records
* manual review of suspicious values
* mitigation of extreme-value distortion
* creation of a unified training-ready dataset

This step is especially important because heterogeneous data integration is one of the main failure points in industrial AI projects.

#### Persistence and data storage

The methodology also considers how the processed information and prediction outputs should be stored.

This includes a historian or data lake logic capable of preserving:

* model inputs
* generated predictions
* observed real values
* evaluation traces
* model version references

This is essential for later analysis, monitoring, and traceability.

#### AI algorithm selection

Rather than assuming a single best model from the beginning, the methodology defines model selection as a comparative process.

In this project, five neural network families were selected for evaluation:

* **MLP**
* **LSTM**
* **GRU**
* **TCN**
* **Temporal Fusion Transformer (TFT)**

These models were chosen because they represent different ways of handling temporal dependencies, complexity, and interpretability.

#### Training pipeline design

The software methodology also includes a formal training pipeline.

This means the project does not stop at “trying models”, but defines a consistent workflow for:

* dataset preparation
* train/validation/test separation
* model training
* hyperparameter tuning
* comparison across architectures
* final model selection

### 2.3 Hardware and sensing design

The methodology also includes the physical side of the system.

This involves:

* sensor selection and configuration logic
* process measurement strategy
* communications architecture
* edge-computing considerations where relevant

Even when the project is portfolio-oriented, keeping this block is valuable because it shows that the AI solution is conceived as part of a broader cyber-physical system rather than as isolated code.

---

## Phase 3. Implementation and validation

After the design phase, the methodology moves into implementation.

### 3.1 Model training

The first step is the actual training of the selected architectures using the prepared dataset.

The methodology requires evaluating performance rigorously across the different forecast horizons and comparing models under the same problem definition.

This comparative design is important because it avoids choosing the final model based only on intuition or architectural popularity.

### 3.2 Functional laboratory validation

Before any field-oriented deployment, the methodology includes validation in a controlled setting.

The purpose of this stage is to verify that:

* the data flow works correctly
* preprocessing behaves as expected
* inference can be executed reliably
* outputs are coherent and interpretable
* the system can operate as a functional prototype

This stage acts as a bridge between offline modeling and real pilot preparation.

### 3.3 Pilot deployment preparation

The methodology then extends toward a pilot-oriented field setup.

In the Gorbea case, the proposal is not full autonomous control, but a **pilot-mode decision-support configuration**. The model generates predictions for evaluation and visualization purposes, without directly intervening in plant control loops.

This is a strong methodological choice because it reduces operational risk while still enabling meaningful real-world assessment.

### 3.4 Interface and alert logic

A model only becomes useful when its output can be interpreted and acted upon.

For that reason, the methodology includes the design of:

* user-facing visualization
* comparison between predicted and real values
* error tracking views
* future alerting logic based on thresholds and severity levels

This makes the framework closer to a deployable support system than to an academic experiment.

---

## Phase 4. Operation strategy

The methodology also defines how the system should behave during operation.

### 4.1 Model monitoring

Once the system is running, its behavior must be observed continuously.

The methodology therefore includes monitoring of:

* prediction quality over time
* differences between predicted and observed turbidity
* possible performance drift
* anomalies in the input pipeline
* execution failures or communication issues

This is critical because water treatment conditions are dynamic and model degradation is always a realistic possibility.

### 4.2 Action policy

An operational AI system must define what happens after a prediction is produced.

In the pilot scope of this project, the model runs in **shadow mode**, meaning that predictions are generated and evaluated but do not trigger automatic control actions.

This makes the methodology safer and more realistic for an initial implementation. It allows the utility to build confidence in the system before considering tighter operational integration.

### 4.3 Compliance and traceability

The methodology explicitly includes traceability as a design requirement.

For an industrial prediction system, it must be possible to reconstruct:

* which inputs were used
* which model version generated the output
* what prediction was produced
* what the real observed value was
* how the error evolved

This is especially important in regulated environments and in any pilot that may later evolve into production use.

---

## Phase 5. Maintenance and lifecycle management

A major strength of the methodology is that it does not end once the first prototype is working.

### 5.1 Preventive and corrective maintenance

The framework distinguishes between:

* maintenance of the existing physical infrastructure
* maintenance of the new AI-specific software layer

For the software component, this includes checking logs, detecting failures, verifying correct restart behavior, and ensuring continuity of data ingestion, preprocessing, and inference.

### 5.2 Model lifecycle management

The methodology also anticipates the long-term lifecycle of the model.

This includes defining future procedures for:

* model versioning
* retraining criteria
* replacement of outdated models
* dependency and library updates
* controlled retirement of obsolete versions

This is especially relevant in industrial AI, where a model that performs well today may become less reliable as operational patterns evolve.

---

## How the methodology was applied in this project

The methodology was not left at a conceptual level. It was applied to a real case study focused on **predicting turbidity at the intake of the Gorbea water treatment plant**.

The applied workflow included:

* definition of plant-specific objectives
* selection of predictive variables
* integration of SCADA and meteorological data
* preprocessing and synchronization
* comparative training of five neural architectures
* evaluation under 1 h, 3 h, and 6 h horizons
* validation of a functional prototype
* preparation of a pilot-oriented deployment logic

This practical application is what gives the methodology portfolio value: it is not only proposed, but demonstrated on a real operational problem.

---

## Why this methodology matters

From a technical portfolio perspective, this methodology is one of the strongest parts of the project because it demonstrates the ability to:

* frame an industrial problem systematically
* translate plant needs into AI requirements
* design a full predictive workflow
* integrate heterogeneous data sources
* compare model families rigorously
* think about deployment from the start
* include monitoring, traceability, and maintenance in the solution design

This makes the project more mature than a standard forecasting notebook or benchmark experiment.

---

## Methodology in one sentence

This project proposes a **reusable end-to-end methodology for AI-based water quality prediction**, covering problem framing, data integration, system design, model comparison, validation, pilot preparation, operation strategy, and lifecycle management.

---

## Next documents

To continue with the technical reading path, the next recommended sections are:

* [`data-pipeline.md`](data-pipeline.md)
* [`modeling.md`](modeling.md)
* [`system-architecture.md`](system-architecture.md)
* [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
* [`results.md`](results.md)
