# Research Context — EG-DQN Dynamic Pricing Paper

**Purpose:** Onboarding document for AI agents. Read this **first** before editing `main.tex`, running experiments, or changing citations.

**Last updated:** 2026-07-06

**Document roles (do not duplicate):**

| File | Use for |
|------|---------|
| **`context.md`** (this) | What the project is, how agents should behave, **progress snapshot** |
| **`blueprint.md`** | Claim tiers, authoritative tables, step done-criteria, section-by-section edit plan |
| **`data.md` + `figures/`** | Source of truth for all numbers (`figures/17.png` overrides `data.md` on Pure DQN revenue) |

---

## 1. One-paragraph summary

This project is an **IEEE-style academic paper** presenting **EG-DQN** (Econometric-Guided Deep Q-Network): a **hybrid retail dynamic pricing** method that combines a **GLM econometric demand model** with a **DQN reinforcement learning agent**. The econometric model supplies elasticity estimates, demand forecasts, and a GLM-optimal price; these signals are integrated into RL via **three mechanisms** — state augmentation, reward shaping, and early action masking. The work is evaluated as a **simulation study** (not a deployed production system): synthetic 3-product daily demand over 4 years, calibrated to M5/Walmart-style statistics, with a **120-day holdout** and **five-strategy comparison** (fixed-price, rule-based, GLM-only, pure DQN, EG-DQN). The goal is **publication-ready honest science** — defensible design + benchmark + evaluation protocol — **not** claiming a universally superior pricing product or RL breakthrough.

**Headline finding (Branch C, locked):** On holdout, **GLM-optimal and EG-DQN tie** on revenue; EG-DQN does **not** significantly beat GLM (*p* = 0.50). Both beat naive baselines in this simulation.

---

## 2. Research goal

| Goal | Detail |
|------|--------|
| **Scientific** | Document how econometric structure can **guide** RL pricing; report when hybrid adds value vs when it **does not** beat GLM on holdout |
| **Practical** | Offer a reproducible benchmark and ablated hybrid design that retail OR / revenue management researchers can compare against |
| **Publication** | Pass peer review as a **simulation + methods paper** with clear limitations (synthetic data, per-item RL, discrete actions) |
| **Not the goal** | Prove deployment readiness, beat all methods on real data, or claim MARL / inventory extensions (future work) |

---

## 3. Core research questions

1. How does EG-DQN compare to GLM-only, pure DQN, and naive baselines on **holdout revenue** in simulation?
2. Does econometric **reward shaping** (vs full hybrid) account for holdout gains? (**3-config ablation** — not 5 variants)
3. Which comparisons are **statistically significant** on the holdout window? (**Mann–Whitney**; GLM row is NS — report honestly)
4. Can the GLM recover **known ground-truth elasticities** in the calibrated synthetic environment?

*Secondary (Tier B only, not abstract headlines):* training-episode reward trends; do **not** headline ep 35/68, 61% variance, or stable TD-loss (`Loss=nan` in exports).

---

## 4. Method — EG-DQN

### 4.1 Architecture

```
Historical demand + prices
        │
        ▼
   GLM (Poisson, log-link)
   → elasticity ε̂, forecast D̂, optimal price p*_GLM
        │
        ├─► State augmentation: s_t includes p*_GLM,t
        ├─► Reward shaping: bonus λ·exp(−|p_t − p*_GLM|/σ_p)
        └─► Action masking (episodes 1–30): restrict prices near p*_GLM
        │
        ▼
   DQN (2×128 ReLU, K=20 discrete prices)
   → adaptive pricing policy π
```

### 4.2 MDP formulation

| Component | Definition |
|-----------|------------|
| **State** `s_t ∈ ℝ⁵` | price, lag demand, sin(day-of-year), trend index, **p*\_GLM,t** |
| **Action** | One of K=20 prices in [p_min, p_max] |
| **Reward (pure DQN)** | `r_t = p_t · max(0, D̂_t)` |
| **Reward (EG-DQN)** | `r_t^EG = r_t + λ·exp(−|p_t − p*\_GLM|/σ_p)` |
| **Discount γ** | 0.99 |
| **Training** | 300 episodes, ε-greedy 1.0→0.05, replay 10k, batch 64, target sync every 15 ep |

### 4.3 GLM demand model

- Poisson GLM, log-link
- Features: log own-price, log cross-prices, trend, annual seasonality (sin), weekend
- Outputs: **ε̂**, **p*\_GLM,t**, **D̂** for reward and state
- HC3 robust standard errors

### 4.4 Baselines (5 strategies)

| Strategy | Description |
|----------|-------------|
| **Fixed-Price (FP)** | Always reference price — floor |
| **Rule-Based (RB)** | 1.15× 7-day rolling mean price |
| **GLM-Only** | Set price to p*\_GLM each day |
| **Pure DQN** | DQN without econometric guidance |
| **EG-DQN** | Full hybrid (all 3 integrations) |

---

## 5. Simulation environment

| Setting | Value |
|---------|-------|
| Products | n = 3 substitutes |
| Horizon | T = 1,460 days (2021-01-01 → 2024-12-30) |
| Holdout | 120 days OOS test (Item 1 for main revenue comparison) |
| Calibration | M5 / RetailSynth-inspired: price \$6–\$18, CV ≈ 0.25, seasonality |
| DGP | Log-linear multiplicative demand + trend + seasonality + cross-elasticities + Gaussian noise |
| Seed | 42 (reproducibility) |
| RL training | **Per-item** (not joint multi-product MDP) |

**Important:** Environment elasticities are **known by design**. GLM recovery is a **sanity check**, not external validation. DGP is **partially aligned** with GLM log-link structure — may favour econometric-guided methods (state in Limitations).

---

## 6. Authoritative results (snapshot)

**Rule:** Every number in `main.tex` must trace to `data.md` or `figures/`. Full tables, claim tiers, and delete-list → **`blueprint.md`**.

### Holdout revenue — Item 1, 120 days (`figures/17.png`)

| Strategy | Total (\$) | vs FP | Avg price (\$) |
|----------|------------|-------|----------------|
| Fixed-Price | 175,651 | — | 10.00 |
| Rule-Based | 138,864 | −20.9% | 16.00 |
| Pure DQN | 183,549 | +4.5% | 9.10 |
| GLM-Only | 226,764 | +29.1% | 6.00 |
| **EG-DQN** | **226,764** | **+29.1%** | **6.00** |

### Key statistical / design facts

- EG-DQN vs GLM: **not significant** (*p* = 0.50) — lead with this in abstract/conclusion
- EG-DQN vs FP/RB: *p* < 0.001 (large Cohen's *d* — caveat: constant-price baselines)
- Ablation (3 configs): reward shaping drives holdout lift; **EG-DQN = DQN+Reward** on test
- GLM elasticity bias: within **~3.7%** per product; forecast MAPE **~5.2%** on holdout
- **Do not claim:** ep 35/68, 61% variance, state augmentation as main driver, stable TD-loss, EG-DQN beats GLM

### Manuscript rewrite status (`index.tex`)

Steps 0–8d (through **Results — ablation**) are in `index.tex`. Next: **Step 8e** — statistical significance (3 rows: FP, RB, GLM NS; no Pure DQN *p*).

---

## 7. Contributions (target framing)

Use in Intro / Conclusion (match abstract):

1. **Describe** EG-DQN and three mechanisms for injecting GLM signals into DQN pricing.
2. **Implement** a reproducible simulation benchmark (known elasticities, five strategies, holdout evaluation, Mann–Whitney tests, ablation).
3. **Report** that GLM and EG-DQN **tie** on holdout while beating naive baselines; reward shaping aligns outcomes with GLM-guided prices — **without** claiming hybrid dominance over econometrics.

**Lead with:** design + benchmark + evaluation protocol.  
**Do not headline:** deployment, universal superiority, “beats GLM,” or “significant vs all baselines.”

---

## 8. Limitations (must acknowledge)

- **Synthetic data only** — no real Corporación Favorita / M5 live pricing test yet
- **Single-product RL** — DQN trained per item; no joint cross-product action space
- **Discrete actions** — K=20 price grid; GLM-optimal price **collapsed to \$6** (grid minimum) every holdout day
- **GLM–DGP alignment** — simulation favours log-linear structure; may advantage guided methods
- **GLM–EG-DQN tie** — hybrid adds **no** holdout revenue over GLM in this run
- **λ scale** — econometric reward penalty not normalized to revenue units
- **Rule-based heuristic underperforms** fixed-price — design flaw, not a general claim
- **Single seed, single holdout window** — no generalization claim

---

## 9. Future work (already in paper)

1. Real-data validation (Corporación Favorita dataset mentioned)
2. Multi-agent / MARL joint pricing (`hadi2025`, `kumar2026`)
3. Inventory constraints + perishables (`yavuz2024`, `nomura2025`)
4. Field experiments / A/B tests (`chen2023`, `xia2024` pipeline)
5. Fuller ablation (state-only, mask-only variants)

---

## 10. Repository file map

```
dynamic pricing/
├── main.tex                            ← Original manuscript (reference)
├── index.tex                           ← Clean rewrite (blueprint Steps 0–12)
├── Dynamic_pricing_hybrid_EG_DQN.ipynb ← Experiment source code
├── context.md                          ← This file (agent onboarding + progress snapshot)
├── blueprint.md                        ← Edit plan, claim tiers, step criteria, full tables
├── data.md                             ← Notebook stdout export (numbers)
├── citations.md                        ← Citation registry (27 refs, 2022–2026)
├── critic.md                           ← Reviewer-oriented framing notes
├── references/                         ← Source PDFs for verification
└── figures/                            ← Authoritative figures (7/7)
    eda.png, 8.png, 10.png, 15.png, 17.png, 20.png, 21.png
```

---

## 11. Agent workflow — what to read when

| Task | Read first | Then edit |
|------|------------|-----------|
| Any session start | `context.md` (this) | — |
| Which step to do next | `context.md` §12 | `blueprint.md` for that step's done-criteria |
| Numbers / claims / tiers | `blueprint.md` + `data.md` / `figures/` | `main.tex` |
| Citation / bibliography | `citations.md` | `main.tex` § References |
| Related work positioning | `citations.md` §5 | `main.tex` §2 |
| Re-run experiments | `Dynamic_pricing_hybrid_EG_DQN.ipynb` | export → `data.md` → update blueprint |

**Manuscript edit mode:** One blueprint step per session (e.g. “Step 2 — Introduction”). Do not skip ahead without user review.

---

## 12. Manuscript progress (snapshot)

*Detailed done-criteria per step → `blueprint.md` §Section completion tracker.*  
Mark: ⬜ not started · 🔄 in progress · ✅ done · 👁 user review

### Foundation

| Item | Status |
|------|--------|
| Evidence package (`data.md`, `figures/`) | ✅ |
| Claims Ledger + Branch C framing | ✅ |
| Citations cleaned (27 refs) | ✅ |
| Author block | ⬜ placeholder |

### Section-by-section rewrite (`index.tex`)

| Step | Unit | Status |
|------|------|--------|
| 0a | Title | ✅ |
| 0b | Keywords | ✅ |
| 1 | Abstract | ✅ |
| 2 | §Introduction | ✅ |
| 3a–3d | §Related Work (4 subsections) | ✅ |
| 4a–4c | §Problem Formulation | ✅ |
| 5a–5b | §Econometric Demand Model | ✅ |
| 6a | §RL — Network Architecture | ✅ |
| 6b | §RL — Training Procedure | ✅ |
| 6c | §RL — EG-DQN mechanisms | ✅ |
| 6d | §RL — Algorithm | ✅ |
| 7a–7c | §Simulation Environment | ✅ |
| 8a | Results — demand estimation | ✅ |
| 8b | Results — revenue (`17.png`) | ✅ |
| 8c | Results — training (`15.png`) | ✅ |
| 8d | Results — ablation (`20.png`) | ✅ |
| 8e | Results — significance | ⬜ **← next** |
| 8f | Results — rolling elasticity (`21.png`) | ⬜ |
| 9a–9c | §Discussion | ⬜ |
| 10 | §Conclusion | ⬜ |
| 11 | References | ⬜ |
| 14 | Abstract ↔ Results sync pass | ⬜ |
| 15 | Pre-submission checklist | ⬜ |

**Progress:** 17 / ~27 units complete in `index.tex` (~63%). Next: **8e** (significance table; **alone**).

---

## 13. Rules for AI agents

### Do

- Treat this as a **simulation study** — use cautious language (“in this simulation”, “on Item 1”, “suggest”, “consistent with”).
- Trace every **number** in Abstract/Results to `data.md` or `figures/` before keeping it.
- Follow `blueprint.md` Tier A–D claim policy; when in doubt, **downgrade one tier**.
- Follow `citations.md` for new references (2022–2026, verified DOI/arXiv).
- Preserve EG-DQN's **three named mechanisms** in any rewrite.
- Match IEEE `\bibitem` style in `main.tex`.
- Update **this file §12** when a step is marked ✅ in blueprint (after user 👁).

### Do not

- Invent experimental results or citations.
- Claim deployment readiness or real-world validation not in the paper.
- Re-add removed low-credibility refs (`ammar2025`, pre-2022 orphans).
- Overclaim significance (“all baselines”, “beats GLM”, “highest revenue”).
- Copy full blueprint content into `context.md` — keep this file lean.
- Re-run the full notebook unless user asks or Pure DQN significance re-run is needed.

### Voice template

> We describe EG-DQN, a hybrid that injects GLM-derived demand signals into DQN via state augmentation, reward shaping, and action masking. In a synthetic three-product simulation with a 120-day holdout on Item 1, GLM-optimal and EG-DQN attain identical cumulative revenue and do not differ significantly on daily revenue (*p* = 0.50). Relative to fixed-price and rule-based baselines, EG-DQN yields higher holdout revenue in this experiment. Econometric reward shaping accounts for the principal holdout improvement over pure DQN; the full hybrid does not exceed reward-only on test revenue.

---

## 14. Key related work (positioning)

| Paper | Role for us |
|-------|-------------|
| `apte2024` | Closest retail Q-learning baseline |
| `groeneveld2025`, `razumovskiy2025` | DP vs RL comparisons |
| `chenavaz2025` | AI + dynamic pricing SLR |
| `zheng2024` | Hybrid ML demand + dual-agent DRL (architectural neighbour) |
| `safonov2024` | ML vs econometric demand — motivates hybrid |
| `liu2024` | Retail omni-channel DRL + inventory |
| `xia2023`, `xia2024` | Synthetic retail data / simulation benchmarking |
| `cheung2023` | Nonstationary RL / guided exploration |

Full list: `citations.md`.

---

## 15. Compile & technical notes

- **Document class:** `IEEEtran` (journal, two-column)
- **Bibliography:** Manual `\begin{thebibliography}` (no `.bib` file)
- **Compile:** `pdflatex` → `pdflatex` (bibtex comment in header is legacy; no `.bib` used)
- **Implementation:** NumPy DQN (no PyTorch/GPU in paper description)
- **Hyperparameters:** See Table in `main.tex` §Simulation (`λ=30`, K=20, 300 episodes, seed=42)
- **Figure paths:** Prefer `figures/` prefix in `\includegraphics`

---

## 16. Current focus

1. **Next session:** Step **8e only** — Mann–Whitney table (FP, RB, GLM NS at *p*=0.50). Omit Pure DQN row until re-run. Note large *d* vs constant-price baselines as caveat for Discussion.
2. **After Results complete:** Step 14 sync pass on Abstract + Conclusion.
3. **User before submission:** Author names and institution.
4. **Optional (non-blocking):** Re-run Mann–Whitney EG-DQN vs Pure DQN; fix `data.md` Pure DQN row.
5. **Optional citation candidates:** See `proposal.md` (“Citation Parking Lot”).

---

*For claim tiers & step criteria → `blueprint.md` · For citation details → `citations.md` · For code → `Dynamic_pricing_hybrid_EG_DQN.ipynb`*
