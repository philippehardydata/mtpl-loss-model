# MTPL Aggregate Loss Distribution

Monte Carlo simulation of the **aggregate loss distribution** for a French motor third-party liability (MTPL) portfolio.  
Part of a non-life quantitative risk portfolio — see [featured projects](https://github.com/philippehardydata/philippehardydata).

---

## Project overview

Classical **frequency–severity** approach:

- **Frequency** — claims per policyholder ~ Negative Binomial (overdispersion)
- **Severity** — Lognormal (body) + Generalized Pareto (tail, threshold €4,000)
- **Aggregate loss** — Monte Carlo over the full portfolio → VaR & TVaR at 99.5%

---

## Repository structure

```
mtpl-loss-model/
├── README.md
├── frequency_analysis.ipynb    # Frequency calibration
├── severity_analysis.ipynb     # Severity / EVT threshold
├── aggregate_loss.ipynb        # Monte Carlo + risk measures
├── freMTPL2freq.csv
└── freMTPL2sev.csv
```

Run in order: **frequency → severity → aggregate loss**.

---

## Data

**freMTPL2** — French MTPL benchmark (600k+ policies, 36k+ claims):

| File | Content |
|------|---------|
| `freMTPL2freq.csv` | Policy-level frequency data |
| `freMTPL2sev.csv` | Claim payment amounts |

> Source: Charpentier, A. (ed.). *Computational Actuarial Science with R*. CRC Press, 2014.  
> Also available via the [CASdatasets](http://cas.uqam.ca/) R package.

---

## Methods

| Component | Distribution | Rationale |
|-----------|-------------|-----------|
| Claim frequency | Negative Binomial | Variance > mean (overdispersion) |
| Severity (body) | Truncated Lognormal | Fit below €4,000 threshold |
| Severity (tail) | Generalized Pareto | Heavy tail above threshold (Mean Excess Plot) |
| Aggregate loss | Monte Carlo | Full portfolio simulation |

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

## Model limitations

- **Portfolio-level λ and payment probability** — no per-policy GLM yet
- **Multiple payments per claim** — freMTPL2 structure; severity may be understated in the tail
- **Lognormal body** — two-regime profile; Burr/Weibull alternatives worth exploring
- **Simulation count** — default `N_SIMULATIONS = 10_000` in `aggregate_loss.ipynb` (~50 tail obs. at 99.5%); increase to `100_000` for regulatory-style runs

---

## Related projects

| Project | Focus | Repo |
|---------|-------|------|
| **Claim frequency** | GLM vs ML for P(claim) | [EDA-GLM-RF-XGB](https://github.com/philippehardydata/EDA-GLM-RF-XGB) |
| **Aggregate loss (this repo)** | Frequency–severity Monte Carlo, VaR/TVaR | [mtpl-loss-model](https://github.com/philippehardydata/mtpl-loss-model) |
| **Reserve risk (Solvency II)** | Tweedie GLM + double bootstrap CDR | [CDRboot](https://github.com/philippehardydata/CDRboot) |

Full portfolio overview: [philippehardydata](https://github.com/philippehardydata/philippehardydata)

---

## Author

**Philippe le Hardÿ** — Actuarial & Quantitative Risk Consultant  
[GitHub](https://github.com/philippehardydata)
