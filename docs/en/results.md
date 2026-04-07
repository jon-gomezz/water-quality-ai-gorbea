# Results

## What this section covers

This section summarizes the main outcomes of the project from a **technical** and **practical** perspective.

The results are not limited to reporting model metrics. They are interpreted in terms of:

* forecasting quality across different horizons
* effect of meteorological information
* ability to follow critical turbidity peaks
* suitability for pilot deployment
* operational value for a real treatment plant

This is important because, in an applied industrial AI project, the final value of the solution depends not only on benchmark performance, but also on whether it can support useful real-world decisions.

---

## Evaluation framework

The comparative validation was carried out across:

* **five neural architectures**: MLP, LSTM, GRU, TCN, and TFT
* **two data scenarios**:

  * plant data only
  * plant data + meteorological variables
* **three prediction horizons**:

  * 1 hour
  * 3 hours
  * 6 hours

The main quantitative metrics used were:

* **MAE** (Mean Absolute Error)
* **RMSE** (Root Mean Squared Error)
* **R²** (coefficient of determination)

The interpretation of results was complemented with visual validation of predicted versus real series on the test set.

---

## Overall result in one sentence

The clearest global result of the project is that **forecast horizon strongly determines model suitability**: short-term prediction can be solved reliably by several architectures, while long-horizon proactive forecasting clearly benefits from the **Temporal Fusion Transformer (TFT)**.

---

## Quantitative results by horizon

## 1-hour horizon

At the **1-hour prediction horizon**, the overall performance was excellent across almost all architectures.

This is one of the most relevant findings of the study because it shows that **short-term turbidity behavior is highly predictable** when recent plant history is available.

### Main observations

* All evaluated models achieved strong results in the short-term setting.
* R² values were generally around or above **0.90**, showing a very good fit.
* The **TFT achieved the lowest MAE** in the meteorology-enhanced configuration.
* **LSTM, GRU, and TFT** all delivered very strong short-term behavior.

### Headline metrics

With meteorological variables included:

* **TFT** reached an **MAE of 0.1773 NTU**
* **LSTM** reached an **R² of 0.9065**
* **GRU** reached an **R² of 0.9064**
* **TFT** reached an **R² of 0.9050**

### Interpretation

From an engineering perspective, this means that for **immediate situational awareness**, multiple architectures are viable.

The choice of model at this horizon can therefore be guided not only by raw performance, but also by:

* implementation simplicity
* computational cost
* ease of deployment
* maintainability

In other words, at 1 hour the project is not constrained to a single “winner.”

---

## 3-hour horizon

At the **3-hour horizon**, the forecasting task becomes substantially more difficult.

This is reflected in the general degradation of the metrics:

* MAE increases
* RMSE increases
* R² decreases compared with the 1-hour case

This was expected, but it is still important because it marks the point where architecture choice begins to matter much more.

### Main observations

* The performance gap between models becomes more visible.
* **GRU** and **TFT** emerge as the most competitive architectures in the meteorology-enhanced scenario.
* The task is no longer dominated by recent local dynamics only; stronger temporal modeling begins to matter more.

### Headline metrics

With meteorological variables included:

* **GRU** achieved the highest **R² = 0.6635**
* **TFT** remained highly competitive with **R² = 0.6322**
* **LSTM** also performed strongly with **R² = 0.6540**

### Interpretation

This horizon is especially interesting because it sits between **immediate awareness** and **genuinely proactive forecasting**.

At this point, the study suggests that:

* simple baselines begin to lose relative strength
* recurrent and attention-based models offer clearer benefits
* the contribution of meteorological variables becomes more meaningful

The 3-hour horizon therefore acts as the turning point of the study: it is where the problem stops behaving like an easy short-term forecasting task and starts rewarding richer temporal architectures.

---

## 6-hour horizon

The **6-hour horizon** is the most demanding and also the most operationally valuable scenario.

This is where the most important result of the project appears.

### Main observations

* Most architectures suffer a strong drop in performance.
* The gap between the best model and the rest becomes decisive.
* **TFT clearly outperforms all other candidates**.

### Headline metrics

With meteorological variables included:

* **TFT** achieved **R² = 0.6314**
* **TFT** achieved **MAE = 0.4072 NTU**

By contrast, the rest of the models remained at much lower explained variance levels, with R² values only slightly above **0.35** in most cases.

### Interpretation

This is one of the strongest technical conclusions of the entire project.

For **long-horizon proactive forecasting**, the problem requires a model that can:

* handle longer-range temporal relationships
* integrate heterogeneous information effectively
* extract useful signals from meteorology and process context
* remain robust when the prediction target is several hours ahead

The TFT is the only evaluated architecture that clearly satisfies those requirements in this case study.

For that reason, it becomes the strongest model when the objective is **early anticipation of turbidity events several hours in advance**.

---

## Effect of meteorological variables

One of the central hypotheses of the project was that adding weather information would improve predictive performance.

The results support that hypothesis.

### Main finding

Meteorological variables improve the forecasting setup, and their value becomes more evident as the horizon increases.

### Why this makes sense

This behavior is coherent with the physical and operational logic of the system:

* plant-only signals reflect what is already happening locally
* meteorological variables provide information about what may happen next
* that extra anticipation becomes more useful the earlier the forecast must react

### Practical implication

This confirms that the project benefits from treating turbidity forecasting as a **multi-source prediction problem**, not as a purely plant-internal time series.

That design choice is one of the reasons why the project is stronger than a narrow univariate forecasting exercise.

---

## Qualitative validation

The numerical metrics were complemented with a visual analysis of predicted versus real values on the test set.

This is especially important in this type of problem because a model can have acceptable average error while still failing to reproduce the events that matter most operationally: the **abrupt turbidity peaks**.

### Main qualitative findings

* The more competitive models were able to follow the general dynamic evolution of turbidity with good fidelity.
* **LSTM** and especially **TFT** showed better ability to track both timing and magnitude of many of the critical peaks.
* **MLP**, despite its simplicity, performed surprisingly well at the 1-hour horizon.
* However, when the horizon increased to 6 hours, simpler architectures such as MLP became much less capable of reproducing critical peaks with sufficient accuracy.

### Why this matters

From the point of view of plant operation, this qualitative behavior is as important as the metric table itself.

In a treatment plant, the most valuable prediction is not the one that minimizes average error on quiet periods, but the one that gives a useful warning when turbidity is rising sharply.

This is why the better peak-following behavior of **LSTM** and **TFT** is especially meaningful.

---

## From best research model to best pilot model

An important result of the project is that the **best model for maximum predictive performance** is not automatically the same as the **best model for initial deployment**.

### Best model for long-horizon forecasting

For pure predictive strength, especially at 6 hours, the best option is clearly:

* **Temporal Fusion Transformer (TFT)**

### Best model for an initial pilot

For a first pilot-oriented deployment, the project identifies a strong practical candidate:

* **LSTM**

### Why LSTM remains important

Even though TFT obtained the strongest long-horizon results, LSTM remains highly relevant because it offers:

* strong short-term performance
* reliable temporal modeling
* lower implementation complexity
* easier maintenance and debugging
* lower adoption friction for an initial real-world setup

This is one of the most mature engineering conclusions of the project.

The final deployment recommendation depends not only on who wins the benchmark, but on the balance between:

* performance
* simplicity
* maintainability
* deployment risk

---

## Prototype validation result

Another important outcome of the project is that the work does not stop at offline training and testing.

The methodology was taken far enough to validate a **functional prototype**, which means the project achieved more than a pure laboratory comparison of neural architectures.

### What this means in practice

The project demonstrated the feasibility of a pilot-oriented pipeline capable of:

* collecting and integrating relevant data
* preprocessing it consistently
* executing inference with the selected model
* comparing predictions with real observations
* exposing outputs for monitoring and visual validation

This is a strong result from a portfolio perspective because it shows that the project moved from **model experimentation** toward **deployment readiness**.

---

## Operational meaning of the results

The results are relevant not only because the metrics are good, but because they support a credible operational use case.

### Potential operational benefits

If the forecasting system is used effectively, it can support:

* earlier response to turbidity spikes
* more informed coagulant dosing decisions
* improved filtration planning
* reduced overreaction during uncertain periods
* stronger operator awareness during rainfall-driven disturbances

### Strategic meaning for AMVISA-type environments

More broadly, the results show that AI-based turbidity prediction is not just technically feasible, but also operationally meaningful in the context of water utility digitalization.

This reinforces the project’s value as both:

* a **case-specific solution** for the Gorbea plant
* a **reusable reference** for future predictive systems in similar infrastructures

---

## Main takeaways

The main conclusions that emerge from the results are the following:

### 1. Short-term turbidity prediction is highly feasible

At 1 hour, all major architectures achieved strong performance, which confirms that near-term forecasting can already provide useful operational awareness.

### 2. Forecast horizon is the key differentiator

As the horizon increases, the problem becomes much harder and model choice matters much more.

### 3. Meteorological variables are worth integrating

Their contribution becomes more valuable as the need for anticipation increases.

### 4. TFT is the strongest model for proactive long-horizon prediction

It is the clearest winner when the goal is to predict several hours ahead.

### 5. LSTM is a highly credible pilot candidate

It offers an excellent balance between reliability and implementation practicality.

### 6. The project demonstrates deployment-oriented viability

The work validates not only a forecasting idea, but a prototype path toward industrial application.

---

## Results summary

In one sentence, the results show that **AI-based turbidity forecasting is technically viable and operationally meaningful**, with **multiple models performing well at short horizons, meteorological integration improving anticipation, and TFT emerging as the best architecture for long-horizon proactive support**.

---

## Next documents

The next recommended sections are:

* [`deployment-and-monitoring.md`](deployment-and-monitoring.md)
* [`lessons-learned.md`](lessons-learned.md)
