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
- `§VII Simulation Environment`: already supported by `nambiar2022`, `xia2023`, `xia2024`, `chen2023`.
- `§VIII Results`: results stay tied to our own simulation evidence (`data.md`, `figures/`), not extra literature.

## Decision rule before adding

Only add one of these if a reviewer-facing sentence needs explicit support that is **not already covered** by current references (`kopalle2023`, `zhang2025`, `bu2023`, `safonov2024`, `groeneveld2025`, `liu2024`, etc.).

---

## Manuscript staging

| Unit | Status | Notes |
|------|--------|-------|
| **7a–7c** Simulation | ✅ in `index.tex` | Defense cites; ε matrix; hyperparams; 5 baselines |
| **8a** Demand estimation | ✅ in `index.tex` | Tables + `figures/eda.png`, `8.png`, `10.png` |
| **8b** Revenue | ✅ in `index.tex` | `17.png`; GLM=EG-DQN tie @ \$226,764 / \$6; Pure DQN \$183,549 |
| **8c** Training | ✅ in `index.tex` | `15.png`; +25.5%/+26.9%; **no** loss / ep 35/68 |
| **8d** Ablation | ✅ in `index.tex` | `20.png`; 3 rows; reward shaping = lift; EG-DQN = DQN+Reward |
| **8e** Significance | ✅ in `index.tex` | 3 rows; GLM *p*=0.50; large *d* caveat |
| **8f** Rolling elasticity | ✅ in `index.tex` | `21.png`; descriptive; no deployment / 94% claims |
| **9** Discussion | ✅ in `index.tex` | 9a–9c; limitations Tier C; deployment = future only |
| **10** Conclusion | ✅ in `index.tex` | Tier-A; GLM tie; future work cites added |
| **11** References | ✅ | 20 keys; arXiv 35%; 0 orphans |
| **14** Sync pass | ✅ | Abstract ↔ Conclusion ↔ Results aligned (2026-07-08) |

### Step 8a locked numbers (authoritative)

| Item | Value | Source |
|------|-------|--------|
| Own-price ê Item 1/2/3 | −1.519 / −1.770 / −1.245 | `data.md` cell 7 |
| Rel. bias | 1.2% / 1.7% / **3.7%** | same |
| Holdout MAPE / R² (Item 1) | **5.20%** / **0.965** | `data.md` cell 9 |
| Mean test residual | ≈ −4.57 | `figures/10.png` Panel B |
| Caption bans | No “structural identification”; no “unbiased / mean near zero” | blueprint |

**Figures in 8a:** `figures/eda.png`, `figures/8.png`, `figures/10.png`.

### Step 8b locked numbers (authoritative — `figures/17.png` Panel D)

| Strategy | Total | vs FP | Avg price |
|----------|-------|-------|-----------|
| Fixed-Price | \$175,651 | — | \$10.00 |
| Rule-Based | \$138,864 | −20.9% | \$16.00 |
| Pure DQN | \$183,549 | +4.5% | \$9.10 |
| GLM-Only | \$226,764 | +29.1% | \$6.00 |
| EG-DQN | \$226,764 | +29.1% | \$6.00 |

**Framing locked:** lead with **tie** + overlap in Panel A; corner at \$6 noted; no “diverges/dominates”; Pure DQN descriptive only (no *p*).  
**Do not trust** `data.md` Pure DQN row (\$226k) — figure overrides.

### Step 8c locked (training)

| Metric | Pure DQN | EG-DQN |
|--------|----------|--------|
| First-20 avg | \$1,564,257 | \$1,579,526 |
| Last-20 avg | \$1,963,282 | \$2,003,676 |
| Improvement | +25.5% | +26.9% |
| Best episode | \$1,976,095 | \$2,017,148 |

**Banned in 8c:** ep 35/68, 61% variance, “stable/faster loss decay,” interpreting centre panel. Cite `cheung2023` only for guided-exploration design context.

### Step 8d locked (ablation — `figures/20.png` Panel B)

| Variant | Test revenue | vs Pure DQN |
|---------|--------------|-------------|
| Pure DQN | \$173,830 | — |
| DQN + Reward | \$226,764 | +30.5% |
| Full EG-DQN | \$226,764 | +30.5% |

**Framing:** reward shaping = principal holdout gain; full hybrid **does not** beat reward-only; state/mask → future work. No 5-variant table.

### Step 8e locked (significance — `data.md` cell 18)

| Baseline | *p* | Cohen's *d* | Bootstrap 95% CI |
|----------|-----|-------------|------------------|
| Fixed-Price | <0.001 | 2.50 | [+421.5, +437.4] |
| Rule-Based | <0.001 | 4.63 | [+725.6, +751.7] |
| GLM-Only | **0.50** | 0.00 | [+0.00, +0.00] |

**Omitted:** Pure DQN row (stale tied series in export). **Lead with GLM NS** in prose.

### Step 8f locked (rolling elasticity — `figures/21.png`)

- Protocol: 12-month rolling GLM, 30-day step, Item 1 own-price ε.
- **Say:** estimates drift; elastic range; truth sometimes inside/outside CI; simulation diagnostic only.
- **Do not say:** deployment-ready, 94% coverage, CV 2.9%, “confirming reliability,” outperform.

### Step 9 locked (Discussion)

- **9a:** Reward shaping aligns RL with GLM; no hybrid > GLM; ablation = reward channel; `cheung2023`, `apte2024`; Cohen's *d* caveat.
- **9b:** Synthetic; \$6 corner; GLM=EG-DQN tie; DGP alignment; level misfit; per-item/discrete/single seed; Loss=nan; rule-based flaw; λ scale; incomplete ablation; `nambiar2022`, `xia2024`.
- **9c:** Field path as future work only; `chen2023`, `xia2024`, `zhang2025`; not a deployment recommendation.

### Step 10 locked (Conclusion)

- Recap EG-DQN + simulation protocol; **GLM tie**; reward shaping ablation; no deployment.
- Future work: real data, MARL (`hadi2025`, `kumar2026`), perishables (`yavuz2024`, `nomura2025`), fuller ablations.
- **Banned:** 18.3%, ep 35/68, “significant vs all,” outperform GLM, % lifts as headlines.
