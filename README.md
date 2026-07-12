# World Cup 2026 Prediction Model

A prediction model for the remaining matches of the 2026 FIFA World Cup (semifinals, 3rd place, final), built as a project for a Master's in Big Data & Data Science. Combines Elo ratings, a Poisson attack/defense model, and Monte Carlo simulation.

## Results (as of the July 11, 2026 quarterfinals)

| Match | Prediction |
|---|---|
| Semifinal 1 — France vs Spain (July 14) | Spain ~57% · France ~43% |
| Semifinal 2 — England vs Argentina (July 15) | Argentina ~55% · England ~45% |
| **Champion probability** | Spain 31.6% · Argentina 27.1% · France 21.5% · England 19.8% |

## Methodology

1. **Elo ratings** — fit on 49,500+ international matches back to 1872, weighted by competition importance (World Cup > continental championships > friendlies) and goal difference.
2. **Poisson attack/defense model** (Dixon-Coles style) — each team gets a latent attack and defense strength, fit by maximum likelihood on matches since 2014 with a 3-year exponential time-decay so recent form and current squads dominate. Ridge-regularized to avoid overfitting teams with sparse data.
3. **Monte Carlo simulation** — 20,000 simulated runs through the rest of the bracket. Regulation goals are drawn from the fitted Poisson rates; drawn matches go to a compressed extra-time simulation, then to an Elo-weighted penalty shootout if still level.

Full derivation, code, and commentary are in [`wc2026_predictor.ipynb`](./wc2026_predictor.ipynb).

## How to run

```bash
pip install pandas numpy scipy matplotlib jupyter
jupyter notebook wc2026_predictor.ipynb
```

Run all cells — `results.csv` and `shootouts.csv` are already included in this repo, so no external downloads are needed.

## Data source

[martj42/international_results](https://github.com/martj42/international_results) — a community-maintained dataset of international match results, updated through the current tournament.

## Limitations

- Hyperparameters (time-decay half-life, data cutoff year, regularization strength) are reasonable defaults, not cross-validated against past tournaments.
- The penalty-shootout model is a simplified heuristic, not a fitted model — real shootout samples are small and close to random.
- No player-level data (injuries, suspensions, squad value) is included.

## Disclaimer

Built for educational purposes as part of a data science curriculum. Not betting advice.
