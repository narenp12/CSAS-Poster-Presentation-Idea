# Premier League Match Uncertainty as a Relegation Signal

Bayesian hierarchical analysis of Shannon entropy from bookmaker odds.

Produced for the Connecticut Sports Analytics Symposium (CSAS) 2026.

## The idea

Bookmakers set odds that reflect match uncertainty. If a team's matches are consistently hard to call (high uncertainty week after week), maybe that tells us something about their quality. I tested this with 22 seasons of Premier League Bet365 odds: calibrated each match's probabilities via the power method, computed Shannon entropy per match, then fed team-level entropy features into a Bayesian logistic regression to predict relegation.

Main finding: a one-standard-deviation bump in average match entropy raises relegation odds by about 55%, with a 100% posterior probability of a positive effect. It holds up across different priors too.

## Project layout

```
├── data/
│   ├── raw/              Bet365 odds, match results, 22 seasons of standings
│   └── processed/        Fitted models (.rds)
├── notebooks/            Quarto pipeline, runnable top to bottom
│   ├── 01-data-exploration/
│   ├── 02-odds-transformation/
│   ├── 03-entropy-exploration/
│   └── 04-bayesian-model/
├── poster/               CSAS conference poster
├── paper/                Working paper + references
└── theory/               Background reading
```

## Pipeline

| Step | Notebook | What happens |
|------|----------|-------------|
| 1 | data-exploration | Clean 22 seasons of EPL data into a team-level panel |
| 2 | odds-transformation | Convert raw odds to probabilities using the power method (Buchdahl 2019) via the `implied` package |
| 3 | entropy-exploration | Compute Shannon entropy per match, visualize patterns across teams and seasons |
| 4 | bayesian-model | Fit the hierarchical model, inspect posteriors, check prior sensitivity |

## Model

Outcome is binary: did a team get relegated in season s? (1 = yes)

The model pools information across seasons with a random intercept:

```
relegated ~ Bernoulli(p)

logit(p) = intercept + b1 * mean_entropy + b2 * entropy_variance + season_effect

season_effect ~ Normal(0, sigma_season)
```

Priors are weakly informative — meant to regularize, not drive results:

- b1, b2 ~ Normal(0, 1.5) — allows moderate effects on the logit scale
- sigma_season ~ Normal+(0, 1) — season-to-season variation shouldn't be huge
- intercept ~ Normal(0, 2) — covers plausible baseline relegation rates

## Tools

R, Quarto, brms (Stan backend), tidyverse, posterdown.
