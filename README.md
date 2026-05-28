# Premier League Match Uncertainty as a Relegation Signal

**A Bayesian Hierarchical Analysis of Shannon Entropy from Bookmaker Odds**

Presented at the Connecticut Sports Analytics Symposium (CSAS) 2026.

## Overview

Can the uncertainty implied by betting markets predict which teams get relegated? This project tests whether **Shannon entropy** — a measure of match-level uncertainty derived from Bet365 odds — serves as a predictive signal for relegation in the English Premier League.

Using a Bayesian hierarchical logistic regression across 22 seasons (2004–2025), I find that a one-standard-deviation increase in a team's average match entropy raises the odds of relegation by approximately **55%**, with a 100% posterior probability of a positive effect. The result is robust across multiple prior specifications.

## Structure

```
├── data/
│   ├── raw/              # Bet365 odds, match results, seasonal standings
│   └── processed/        # Fitted Bayesian models (.rds)
├── notebooks/            # Reproducible analysis pipeline (Quarto)
│   ├── 01-data-exploration/
│   ├── 02-odds-transformation/
│   ├── 03-entropy-exploration/
│   └── 04-bayesian-model/
├── poster/               # CSAS conference poster
├── paper/                # Working paper + references
└── theory/               # Theoretical background
```

## Pipeline

| Step | Notebook | What it does |
|------|----------|-------------|
| 1 | `01-data-exploration` | Cleans 22 seasons of EPL match data into a team-level panel |
| 2 | `02-odds-transformation` | Converts raw bookmaker odds to calibrated probabilities (power method) |
| 3 | `03-entropy-exploration` | Computes Shannon entropy per match, visualizes team-level patterns |
| 4 | `04-bayesian-model` | Fits hierarchical logistic regression; posterior analysis + sensitivity checks |

## Methods

- **Data**: Bet365 odds for 5,860 EPL matches across 22 seasons
- **Probability calibration**: Power transformation (Buchdahl, 2019) via the `implied` R package — removes bookmaker overround while preserving relative odds structure
- **Entropy**: Per-match Shannon entropy $H_i = -\sum_{k} p_k \log_3 p_k$ (base 3 normalizes to $[0,1]$), then aggregated to team-season mean and variance
- **Model**: Bayesian hierarchical logistic regression (`brms`/Stan):
  - $\text{logit}(p_{c,s}) = \alpha + \beta_1 \bar{H}_{c,s} + \beta_2 \text{Var}(H)_{c,s} + \gamma_s$
  - $\gamma_s \sim \mathcal{N}(0, \sigma_{\text{season}})$ — partial pooling across seasons
  - Weakly informative priors: $\mathcal{N}(0, 1.5)$ for slopes, $\mathcal{N}^+(0, 1)$ for season variance

## Tools

R • Quarto • brms (Stan) • tidyverse • posterdown

## Author

**Naren Prakash** — naren.prakash@uconn.edu
