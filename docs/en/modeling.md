# Modeling

## Purpose of the modeling stage

The objective of the modeling stage was not simply to train a forecasting algorithm, but to determine **which neural architecture was most suitable for turbidity prediction under different operational horizons**.

This is an important distinction. In an industrial setting, the “best model” is not only the one with the strongest metric in isolation, but the one that provides the most useful balance between:

* predictive quality
* temporal horizon suitability
* robustness
* implementation complexity
* deployment practicality

For that reason, the modeling stage was designed as a **comparative study** rather than as a single-model experiment.

---

## Modeling objective

The practical modeling objective of the project was to predict **raw water turbidity at the intake of the Gorbea treatment plant** using historical plant data and, when available, meteorological variables.

The comparison was structured around three forecast horizons:

* **1 hour**
* **3 hours**
* **6 hours**

These horizons were chosen because they correspond to different operational needs:

* **1 hour** supports immediate situational awareness
* **3 hours** supports short-term anticipation and adjustment planning
* **6 hours** supports more proactive decision-making before the event becomes critical

---

## Why multiple architectures were compared

Turbidity forecasting is a multivariate time-series problem with several challenges:

* sudden peaks during rainfall events
* delayed effects between precipitation and plant response
* non-linear relationships between variables
* both short-term and longer-range temporal dependencies

Because of this, it would have been too restrictive to assume that one model family would be optimal in all cases.

The modeling strategy therefore compared five architectures with different inductive biases:

* **MLP** as a simple feed-forward baseline
* **LSTM** as a long-memory recurrent model
* **GRU** as a lighter recurrent alternative
* **TCN** as a temporal convolutional architecture
* **Temporal Fusion Transformer (TFT)** as an attention-based sequence model for multi-horizon reasoning

This comparison makes the project much stronger from a portfolio perspective, because it shows systematic model selection rather than architecture guessing.

---

## Models evaluated

## 1. MLP

The Multilayer Perceptron was included as a simple reference architecture.

In time-series problems, an MLP does not model sequence order intrinsically. Instead, temporal dynamics must be encoded through lagged inputs or sliding windows. Its main advantages are:

* simplicity
* fast training
* lightweight inference
* low implementation overhead

This makes it useful as a practical baseline and as a reminder that not every forecasting problem requires a highly complex architecture.

Its main limitation is that its temporal memory is fixed by the chosen input window, and it does not explicitly model sequential structure the way recurrent or attention-based models do.

---

## 2. LSTM

The Long Short-Term Memory network was included because it is one of the most established architectures for sequence modeling with long-range dependencies.

LSTM is particularly relevant in this project because turbidity changes are not always immediate. Rainfall and upstream disturbances may take time to propagate into the intake conditions, and plant behavior itself unfolds over time.

Its main strengths are:

* explicit temporal memory
* ability to capture delayed effects
* strong track record in water-quality forecasting
* relatively mature and deployment-friendly implementation patterns

Its main drawback is higher complexity and slower training compared with simpler alternatives.

---

## 3. GRU

The Gated Recurrent Unit was evaluated as a lighter recurrent alternative to LSTM.

GRU retains the main idea of gated memory while using a simpler internal structure and fewer parameters. This can be valuable when:

* the dataset is more limited
* training efficiency matters
* similar recurrent performance can be achieved with a smaller model

In the context of this project, GRU is especially interesting because it offers a middle ground between expressiveness and computational simplicity.

---

## 4. TCN

The Temporal Convolutional Network was included to test a non-recurrent sequence model based on:

* causal convolutions
* dilated convolutions
* residual blocks

This architecture is attractive because it can process sequences in parallel and build large receptive fields efficiently. In practice, this can make training more stable and faster than recurrent models, while still preserving strong temporal modeling capability.

Its main limitation is that its effective temporal field is determined by design. If the relevant dependencies extend beyond the model’s receptive field, performance may suffer.

---

## 5. Temporal Fusion Transformer (TFT)

The Temporal Fusion Transformer was included as the most advanced architecture in the study.

TFT combines multiple mechanisms that are especially relevant for this project:

* recurrent sequence encoding
* gated residual processing
* variable selection
* attention mechanisms
* strong suitability for multi-horizon forecasting

This makes it particularly attractive for a problem like turbidity prediction, where different variables may matter at different times and where longer-range proactive forecasting is especially valuable.

Its main trade-off is greater implementation and computational complexity.

---

## Experimental design

The modeling study was not limited to one dataset configuration.

Each architecture was evaluated under two input scenarios:

1. **Plant data only**
2. **Plant data + meteorological variables**

This comparison was important because one of the central hypotheses of the project is that weather information improves predictive performance, especially when the goal is to anticipate rainfall-driven turbidity events before their full effect becomes visible at the plant.

The evaluation was then repeated for each of the three forecast horizons:

* 1 h
* 3 h
* 6 h

As a result, the modeling stage compares architectures across:

* different temporal assumptions
* different information availability scenarios
* different operational use cases

---

## Modeling workflow

The modeling stage followed a consistent workflow.

### 1. Problem definition

The target variable was defined as future turbidity at the selected forecast horizon.

This means that each model learned to map a historical multivariate context window into a future turbidity estimate.

### 2. Sequence preparation

The cleaned and synchronized dataset from the data pipeline was converted into model-ready temporal windows.

Each input sample contained a past sequence of explanatory variables, while the output corresponded to turbidity at the future horizon of interest.

### 3. Train / validation / test split

The dataset was partitioned respecting temporal causality.

This is essential in forecasting: the model must be trained on past data and evaluated on later unseen periods, rather than on randomly shuffled records.

### 4. Training and tuning

Each architecture was trained under the same general problem definition, allowing a fairer comparison across model families.

The study focused on comparing architectural suitability rather than only maximizing one isolated run.

### 5. Evaluation

The evaluation combined:

* **quantitative metrics**
* **visual qualitative inspection**
* **practical operational interpretation**

This broader evaluation logic is important because a model can have acceptable metrics while still failing to track peaks in a useful way.

---

## Evaluation criteria

The main quantitative metrics used in the project were:

* **MAE** (Mean Absolute Error)
* **RMSE** (Root Mean Squared Error)
* **R²** (coefficient of determination)

These metrics provide complementary information:

* **MAE** reflects average absolute deviation
* **RMSE** penalizes large errors more strongly
* **R²** measures how much of the variance is explained by the model

This combination is suitable for turbidity forecasting because both overall fit and large peak errors matter operationally.

---

## Main modeling results by horizon

## 1-hour horizon

At the **1-hour horizon**, all architectures performed strongly.

This suggests that short-term turbidity behavior is sufficiently predictable for a wide range of model families when the recent past is available. Even relatively simple architectures can deliver useful short-term forecasting performance in this setting.

Still, the **TFT achieved the lowest MAE** in the 1-hour scenario, while **LSTM, GRU, and TFT** all showed very strong overall performance.

From an engineering point of view, this means that for immediate situational awareness, architecture choice is important but not yet decisive in the same way it becomes at longer horizons.

---

## 3-hour horizon

At the **3-hour horizon**, the problem becomes substantially harder.

Errors increase and explained variance decreases across all architectures, which is expected in a more demanding forecasting task.

In this middle-range scenario:

* **GRU** achieved the strongest R² in the meteorology-enhanced setting
* **TFT** remained highly competitive and robust
* the separation between stronger and weaker architectures became more visible than at 1 hour

This shows that medium-range forecasting already begins to reward architectures with stronger temporal representation capacity.

---

## 6-hour horizon

At the **6-hour horizon**, the difference between architectures becomes decisive.

This is where the **Temporal Fusion Transformer clearly stands out**. With meteorological variables included, it delivered the strongest overall behavior and the highest explained variance by a wide margin.

This is one of the most important technical findings of the project:

* short-horizon prediction can be handled well by several models
* long-horizon proactive forecasting requires a more expressive architecture
* **TFT becomes the most suitable option when the goal is early proactive support several hours ahead**

This result is especially relevant because the 6-hour horizon is one of the most operationally valuable for plant management.

---

## Effect of meteorological variables

One of the key modeling questions in the project was whether weather information materially improves turbidity forecasting.

The results show that **including meteorological variables is beneficial**, particularly as the forecast horizon increases.

This makes sense from a process perspective:

* plant-only signals reflect what is already happening locally
* meteorological variables help anticipate what may happen next
* their contribution becomes more important the earlier the system is expected to react

This is why the combined **SCADA + meteorology** setup is one of the defining strengths of the project.

---

## Quantitative highlights

Some of the clearest quantitative conclusions of the study are:

* At **1 hour**, the **TFT** achieved the lowest MAE in the meteorology-enabled scenario.
* At **3 hours**, **GRU** and **TFT** were the most competitive architectures.
* At **6 hours**, **TFT** clearly outperformed the rest, reaching the strongest R² and the lowest MAE among the evaluated models.

This confirms that model suitability is **horizon-dependent**, which is one of the central engineering conclusions of the project.

---

## Qualitative interpretation of the predictions

The project also included visual analysis of predicted versus real turbidity trajectories.

This is particularly important in a problem with sharp peaks, because global metrics alone may hide whether the model can actually:

* follow the timing of turbidity spikes
* capture their magnitude
* reproduce abrupt changes instead of over-smoothing them

The qualitative analysis showed that the more competitive models, especially **LSTM** and **TFT**, were better able to follow the dynamic behavior of turbidity and align with many of the critical peaks.

This matters more than average performance alone, because in a real treatment plant the difficult events are precisely the ones that operators care about most.

---

## Why LSTM was still important for deployment

Although the **TFT** delivered the strongest long-horizon performance, the project does not reduce the conclusion to “TFT wins everything.”

For an **initial pilot-oriented deployment**, **LSTM** remains a very attractive option because it offers:

* strong short-term performance
* robust temporal modeling
* lower implementation complexity than TFT
* easier operational adoption in an early-stage deployment context

This is a very important portfolio message: the final engineering recommendation depends not only on predictive superiority, but also on deployment maturity, maintainability, and operational simplicity.

---

## Modeling conclusions

The modeling stage leads to several major conclusions.

### 1. Forecast horizon is a decisive factor

The ranking of architectures changes as the prediction horizon increases.

### 2. Architecture complexity becomes more valuable at longer horizons

When the task moves from immediate awareness to proactive anticipation, stronger temporal modeling capacity becomes more important.

### 3. Meteorological variables improve forecasting value

The integration of weather information strengthens the model’s ability to anticipate future disturbances.

### 4. TFT is the strongest model for proactive forecasting

For the most operationally valuable long-horizon case, TFT provides the clearest advantage.

### 5. LSTM is a strong practical deployment candidate

For an early pilot, LSTM offers an effective compromise between predictive quality and engineering simplicity.

---

## Why the modeling stage is strong from a portfolio perspective

This part of the project demonstrates the ability to:

* frame forecasting as an engineering comparison problem
* evaluate multiple deep learning architectures systematically
* reason about model suitability by use case, not only by raw metrics
* connect predictive behavior with plant operations
* distinguish between research-best and deployment-best choices

That makes the modeling stage much stronger than a typical one-model benchmark notebook.

---

## Modeling summary

In one sentence, the modeling stage shows that **turbidity forecasting performance depends strongly on prediction horizon**, with **TFT emerging as the best architecture for long-horizon proactive prediction** and **LSTM standing out as a practical pilot-oriented option**.

---

## Next documents

The next recommended sections are:

* [`results.md`](results.md)
* [`system-architecture.md`](system-architecture.md)
* [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
