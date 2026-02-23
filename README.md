# Geo Experiment with Differences-in-Differences (DiD)

A Jupyter notebook demonstrating how to run a **geo experiment** using the **Differences-in-Differences (DiD)** approach, from a statistics perspective. The notebook covers the full workflow: notation, parallel trends check, estimation (2×2 and regression), cluster-robust inference, and power analysis.

---

## What is this?

In **geo experiments**, treatment is randomized at the geography level (e.g., regions, DMAs, countries)—not at the user level. Some geos receive a treatment (e.g., a new feature or campaign); others remain in control. We compare aggregate outcomes (revenue per user, conversion rate, etc.) between treatment and control geos over time.

**Differences-in-Differences (DiD)** estimates the treatment effect by comparing the *change* in the outcome (from pre to post) in treatment geos to the change in control geos. The control geos' change serves as the counterfactual: what would have happened to treatment geos without the intervention.

**Key assumption (parallel trends):** In the absence of treatment, the average outcome would have evolved in the same way for treatment and control geos.

---

## What the notebook covers

| Section | Description |
|--------|-------------|
| **1. Setup and Notation** | Geos, periods, treatment indicator $D_g$, outcome $Y_{gt}$, post indicator. |
| **2. The DiD Estimand** | The 2×2 formula and the regression formulation. |
| **3. Synthetic Data Generation** | Simulates geos over pre/post periods with a known treatment effect; parallel trends hold by construction. |
| **4. Checking Parallel Trends** | Visual plot (outcome by period for treatment vs control) and a formal regression test (pre-period only: high p-value on period×treated supports parallel trends). |
| **5. Estimating the Effect: 2×2 Table** | Computes the four cell means and the DiD estimate by hand. |
| **6. Estimating the Effect: Regression** | Fits `y ~ treated * post`; the coefficient on `treated:post` is $\hat{\tau}$. |
| **7. Equivalence** | Shows that the 2×2 DiD and the regression coefficient are numerically identical. |
| **8. Inference: Cluster-Robust SEs** | Uses cluster-robust standard errors by geo (Bertrand et al., 2004); reports 95% CI. |
| **9. Power Analysis** | Simulation-based power: repeatedly generates data, runs the DiD test with clustered SEs, and reports empirical power and Type I error. |
| **10. Assumptions and Best Practices** | Randomization, parallel trends, no spillover, clustering, aggregation; references. |

**2×2 DiD formula:**

$$\tau_{\text{DiD}} = (\bar{Y}^{\text{treat}}_{\text{post}} - \bar{Y}^{\text{treat}}_{\text{pre}}) - (\bar{Y}^{\text{control}}_{\text{post}} - \bar{Y}^{\text{control}}_{\text{pre}})$$

**Regression formulation:**

$$Y_{gt} = \alpha + \beta_1 D_g + \beta_2 \text{Post}_t + \tau (D_g \times \text{Post}_t) + \varepsilon_{gt}$$

The coefficient $\tau$ on the interaction $D_g \times \text{Post}_t$ is the average treatment effect on the treated (ATT).

---

## Why cluster-robust standard errors?

Treatment is assigned at the **geo** level, so outcomes within a geo are correlated across periods. Standard OLS standard errors assume independent errors and can be severely biased. We use **cluster-robust** standard errors with clusters = geo to obtain valid inference.

---

## Requirements

```
numpy
pandas
statsmodels
matplotlib
```

Install with: `pip install numpy pandas statsmodels matplotlib`

---

## How to run

1. Open `simulation.ipynb` in Jupyter or a compatible environment.
2. Run all cells in order (Kernel → Run All).
3. The notebook uses synthetic data; you can modify parameters (number of geos, periods, true effect) in the data generation cell.

---

## References

- Angrist & Pischke, *Mostly Harmless Econometrics*
- Bertrand, Duflo & Mullainathan (2004), "How Much Should We Trust Differences-In-Differences?"
- Cunningham, *Causal Inference: The Mixtape*
