[README (2).md](https://github.com/user-attachments/files/26099961/README.2.md)
# Auto Insurance — Aggregate Loss Distribution

This project models the aggregate loss distribution of a motor third-party liability (MTPL) insurance portfolio using Monte Carlo simulation.  
It was built as part of a personal actuarial portfolio.

---

## Project Overview

The model follows the classical frequency-severity approach:

- **Frequency** : number of claims per policyholder ~ Negative Binomial
- **Severity** : claim payment amount ~ Lognormal (body) + Generalized Pareto Distribution (tail, threshold = €4,000)
- **Aggregate loss** : Monte Carlo simulation combining both components over the full portfolio

---

## Repository Structure

```
auto-loss-distribution/
│
├── README.md
├── frequency_analysis.ipynb       # Exploratory analysis and frequency model calibration
├── severity_analysis.ipynb        # Exploratory analysis and severity model calibration
└── aggregate_loss.ipynb           # Final aggregate loss simulation and results
```

Run the notebooks in order: frequency → severity → aggregate loss.

---

## Data

The dataset used is **freMTPL2** (French Motor Third-Party Liability), a publicly available benchmark dataset from the `CASdatasets` R package.

- `freMTPL2freq.csv` — policy and claim frequency data
- `freMTPL2sev.csv` — claim severity (payment amounts)

> Source: Charpentier, A. (ed.). *Computational Actuarial Science with R*. CRC Press, 2014.  
> Available via the `CASdatasets` R package: http://cas.uqam.ca/

---

## Methods

| Component | Distribution | Rationale |
|---|---|---|
| Claim frequency | Negative Binomial | Overdispersion: variance > mean |
| Claim severity (body) | Truncated Lognormal | Reasonable fit below €4,000 threshold |
| Claim severity (tail) | Generalized Pareto | Heavy tail above €4,000 (Mean Excess Plot) |
| Aggregate loss | Monte Carlo | N simulations of full portfolio |

---

## Model Limitations

- **Uniform λ and payment probability** : both are modelled at portfolio level. A future GLM per policyholder (using characteristics such as age, vehicle type, region) would improve individual pricing accuracy.
- **Multiple payments per claim** : since each payment is treated as an independent claim, the payment probability is overstated and the severity per claim is understated. These two effects partially offset each other in the aggregate, but the severity distribution may be underestimated in the tail.
- **Lognormal body fit** : the Lognormal does not fully capture the body profile — the empirical quantile curve suggests two distinct regimes, possibly driven by structural effects in the data (fixed tariffs, coverage caps, or a mix of claim types such as material vs bodily injury). Alternative distributions such as Burr or Weibull could be investigated.

---

## Requirements

```
pandas
numpy
matplotlib
scipy
scikit-learn
```

---

## Author

Built with Python (Jupyter). Actuarial portfolio project.
