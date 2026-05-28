# Premier League Match Uncertainty as a Relegation Signal

**A Bayesian Hierarchical Analysis of Shannon Entropy from Bookmaker Odds**

Presented at the Conference on Statistical and Actuarial Sciences (CSAS) 2026.

## Overview

This project investigates whether match-level **Shannon entropy** — derived from Bet365 bookmaker probabilities — can serve as a predictive signal for **relegation** in the English Premier League. Using a Bayesian hierarchical logistic regression model, we test the hypothesis that teams with higher match-level uncertainty (entropy) face elevated relegation risk.

**Key result:** A one-standard-deviation increase in mean entropy raises the odds of relegation by ~55%, with strong Bayesian evidence (100% posterior probability of a positive effect). This effect persists across prior sensitivity specifications.

## Repository Structure

```
data/
├── raw/                      # Original CSV datasets (games, seasonal, promoted/relegated)
└── processed/                # Fitted Bayesian model objects (.rds)

notebooks/
├── 01-data-exploration/      # Initial data cleaning and team-level aggregation
├── 02-odds-transformation/   # Power-transformation of bookmaker odds to probabilities
├── 03-entropy-exploration/   # Shannon entropy calculation and exploratory analysis
└── 04-bayesian-model/        # Bayesian hierarchical model fitting and convergence diagnostics

poster/                       # CSAS conference poster (Quarto/posterdown)

paper/                        # Working paper, references, bibliography

theory/                       # Theoretical background on entropy, Bayesian modeling

docs/                         # Data dictionary and documentation
```

## Methods

1. **Data**: Bet365 odds for 22 EPL seasons (2004–2025), combined with match results
2. **Probability Calibration**: Power transformation (Buchdahl, 2019) via the `implied` R package
3. **Entropy Features**: Per-match Shannon entropy ($H_i = -\sum p_k \log_3 p_k$), then team-season aggregates (mean, variance)
4. **Model**: Bayesian hierarchical logistic regression with `brms`:
   - $\text{logit}(p_{c,s}) = \alpha + \beta_1 \bar{H}_{c,s} + \beta_2 \text{Var}(H)_{c,s} + \gamma_s$
   - Weakly informative priors: $\mathcal{N}(0, 1.5)$ for slopes, $\mathcal{N}^+(0, 1)$ for season variance

## Tools

- **R + Quarto** for analysis and reproducible reports
- **brms** for Bayesian hierarchical modeling (Stan backend)
- **posterdown** for conference poster rendering

## Author

Naren Prakash
