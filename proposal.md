# Citation Parking Lot (Review Later)

These are optional sources we discussed but did not add to `index.tex` yet.
Keep them here for later review in case we want extra methodological support.

## Candidate sources

1) **Interpretable modelling of retail demand and price elasticity for passenger flights using booking data** (2022, *Statistical Modelling*, DOI: 10.1177/1471082X221083343)
- **Why relevant:** Uses Poisson-type demand intensity with interpretable elasticity structure.
- **Why not added now:** Domain is airline bookings, not retail SKU pricing.
- **Best fit if used later:**  
  - `§V Econometric Demand Model` (brief support for count-demand + log-link motivation), or  
  - `§IX Discussion` (cross-domain methodological precedent).

2) **Dynamic pricing using flexible heterogeneous sales response models** (2024, *OR Spectrum*, DOI: 10.1007/s00291-024-00756-0)
- **Why relevant:** Strong OR framing for dynamic pricing with flexible demand response and dynamic optimization.
- **Why not added now:** Method focus (Bayesian semiparametric + DP) differs from current GLM + DQN setup.
- **Best fit if used later:**  
  - `§II Related Work` (one sentence in Dynamic Pricing and RL / forecasting bridge), or  
  - `§IX Discussion` (future extension toward richer demand response models).

## Where we likely do NOT need them

- `Abstract` / `Introduction`: already citation-dense and reviewer-safe.
- `§VIII Results`: results should stay tied to our own simulation evidence (`data.md`, `figures/`), not extra literature.

## Decision rule before adding

Only add one of these if a reviewer-facing sentence needs explicit support that is **not already covered** by current references (`kopalle2023`, `zhang2025`, `bu2023`, `safonov2024`, `groeneveld2025`, `liu2024`, etc.).
