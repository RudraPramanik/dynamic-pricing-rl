# Research Context — EG-DQN Dynamic Pricing Paper

**Purpose:** Onboarding document for AI agents. Read this first before editing `main.tex`, running experiments, or changing citations.

**Last updated:** 2025-06-24

---

## 1. One-paragraph summary

This project is an **IEEE-style academic paper** presenting **EG-DQN** (Econometric-Guided Deep Q-Network): a **hybrid retail dynamic pricing** method that combines a **GLM econometric demand model** with a **DQN reinforcement learning agent**. The econometric model supplies elasticity estimates, demand forecasts, and a GLM-optimal price; these signals are integrated into RL via **three mechanisms** — state augmentation, reward shaping, and early action masking. The work is evaluated as a **simulation study** (not a deployed production system): synthetic 3-product daily demand over 4 years, calibrated to M5/Walmart-style statistics, with a **120-day holdout** and **five-strategy comparison** (fixed-price, rule-based, GLM-only, pure DQN, EG-DQN). The goal is **publication-ready honest science** — defensible design + ablation + significance tests — **not** claiming a universally superior pricing product.

---

## 2. Research goal

| Goal | Detail |
|------|--------|
| **Scientific** | Show that econometric structure can **guide** RL pricing (faster convergence, lower variance, competitive revenue) without replacing model-free adaptivity |
| **Practical** | Offer a reproducible benchmark and ablated hybrid design that retail OR / revenue management researchers can compare against |
| **Publication** | Pass peer review as a **simulation + methods paper** with clear limitations (synthetic data, per-item RL, discrete actions) |
| **Not the goal** | Prove deployment readiness, beat all methods on real Walmart data, or claim MARL / inventory extensions (those are future work) |

---

## 3. Core research questions

1. Does injecting GLM-derived signals into DQN improve **revenue**, **convergence speed**, and **policy stability** vs pure DQN and vs GLM-only pricing?
2. Which integration mechanism (state / reward / masking) contributes most? (**Ablation**)
3. Are reported gains **statistically significant** on the holdout window? (**Mann–Whitney U**, bootstrap CI, Cohen's *d*)
4. Can the GLM recover **known ground-truth elasticities** in a calibrated synthetic environment? (validation of the prior)

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
- Outputs: **ê**, **p*\_GLM,t**, **D̂** for reward and state
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
| Horizon | T = 1,461 days (2021-01-01 → 2024-12-31) |
| Holdout | 120 days OOS test |
| Calibration | M5 / RetailSynth-inspired: price \$6–\$18, CV ≈ 0.25, seasonality |
| DGP | Log-linear multiplicative demand + trend + seasonality + cross-elasticities + Gaussian noise |
| Seed | 42 (reproducibility) |
| RL training | **Per-item** (not joint multi-product MDP) |

**Important:** Environment elasticities are **known by design** (Table I in paper). GLM recovery is a **sanity check**, not external validation.

---

## 6. Reported results (verify against notebook exports)

Numbers below are what **`main.tex` currently claims**. Agents must **not change these** without checking `Dynamic_pricing_hybrid_EG_DQN.ipynb` or saved tables/figures.

| Claim | Value | Source in paper |
|-------|-------|-----------------|
| EG-DQN vs fixed-price | **+18.3%** revenue | Table: revenue (Item 1, 120-day) |
| GLM-only vs FP | +13.9% | Same |
| Pure DQN vs FP | +11.9% | Same |
| EG-DQN total revenue | **\$151,940** | Same |
| EG-DQN vs GLM-only | +3.1 pp, p=0.003, d=0.31 | Significance table |
| Convergence 90% reward | EG-DQN ep **35** vs DQN ep **68** (~1.9×) | Convergence section |
| Reward variance (final 20 ep) | 0.31 vs 0.79 (**61%** reduction) | Same |
| Ablation: largest gain | **State augmentation** (+$3,130 vs pure DQN) | Ablation table |
| GLM elasticity bias | < **1.44%** on holdout | Elasticity table |
| Forecast MAPE (mean) | **7.83%** | Forecast table |

**Open risk (blueprint):** Abstract has **two competing drafts** (duplicate paragraph + `//` marker). Numbers may not match latest notebook run — **audit before submission** (see `blueprint.md` Claims Ledger).

---

## 7. Contributions (target framing)

Use this positioning in Intro / Conclusion:

1. **EG-DQN hybrid design** — three explicit, ablated integration mechanisms (state, reward, masking).
2. **Reproducible simulation benchmark** — known elasticities, five strategies, non-parametric inference.
3. **Empirical simulation evidence** — revenue, convergence, stability, ablation (scoped to synthetic holdout).

**Lead with:** design + benchmark + evaluation protocol.  
**Do not headline:** deployment, universal superiority, or “beats all baselines” unless tables confirm.

---

## 8. Limitations (must acknowledge)

- **Synthetic data only** — no real Corporación Favorita / M5 live pricing test yet
- **Single-product RL** — DQN trained per item; no joint cross-product action space
- **Discrete actions** — K=20 price grid
- **GLM–DGP alignment** — simulation favours log-linear structure; real demand may differ
- **λ scale** — econometric reward penalty not normalized to revenue units (noted in paper)
- **Adaptive methods may tie on holdout** — if tables show GLM ≈ DQN ≈ EG-DQN, say so honestly

---

## 9. Future work (already in paper)

1. Real-data validation (Corporación Favorita dataset mentioned)
2. Multi-agent / MARL joint pricing (`hadi2025`, `kumar2026`)
3. Inventory constraints + perishables (`yavuz2024`, `nomura2025`)
4. Field experiments / A/B tests (`chen2023`, `xia2024` pipeline)

---

## 10. Repository file map

```
dynamic pricing/
├── main.tex                          ← Manuscript (IEEEtran, manual bibliography)
├── Dynamic_pricing_hybrid_EG_DQN.ipynb ← Experiment source code
├── context.md                        ← This file (agent onboarding)
├── blueprint.md                      ← Polish plan, Claims Ledger, framing rules
├── citations.md                      ← Citation registry (27 refs, 2022–2026)
├── references/                       ← Source PDFs for verification
│   ├── mohit_apte.pdf
│   ├── Krishna Kumar.pdf
│   ├── Lev Razumovskiy_2026.pdf
│   ├── regis_Y_Chenavaz_25.pdf
│   ├── Liu_24.pdf
│   ├── youseke_Nomura_25.pdf
│   └── (optional / low relevance: Bae, Parishad)
└── Figures (linked from main.tex):
    eda.png, 8.png, 10.png, 15.png, 17.png, 20.png, 21.png
```

---

## 11. Agent workflow — what to read when

| Task | Read first | Then edit |
|------|------------|-----------|
| Any manuscript edit | `context.md` (this) | `main.tex` |
| Citation / bibliography | `citations.md` | `main.tex` § References |
| Numbers / claims / abstract | `blueprint.md` Claims Ledger | `main.tex` Results + notebook |
| Related work positioning | `citations.md` §5 | `main.tex` §2 |
| Re-run experiments | `Dynamic_pricing_hybrid_EG_DQN.ipynb` | export tables → update `main.tex` |

---

## 12. Project status (2025-06-24)

| Area | Status |
|------|--------|
| **Citations** | ✅ Cleaned — 27 refs, all cited, pre-2022 removed, hybrid neighbours added |
| **Citation errors** | ✅ Fixed (Razumovskiy, Pillai, Nomura, Li & Zheng, etc.) |
| **Abstract** | ⚠️ Needs merge — two versions, typos, `//` artifact |
| **Title** | ⚠️ Grammar: “With Integrating” → fix |
| **Claims vs notebook** | ❓ Not audited — fill Claims Ledger in `blueprint.md` |
| **Prose / de-AI tone** | 🔜 After evidence alignment |
| **Author block** | Placeholder `[Author Name(s)]` |

---

## 13. Rules for AI agents

### Do

- Treat this as a **simulation study** — use cautious language (“in our simulation”, “suggest”, “consistent with”).
- Trace every **number** in Abstract/Results to a table or figure before keeping it.
- Follow `citations.md` for new references (2022–2026, verified DOI/arXiv).
- Preserve EG-DQN's **three named mechanisms** in any rewrite.
- Match IEEE `\bibitem` style in `main.tex`.

### Do not

- Invent experimental results or citations.
- Claim deployment readiness or real-world validation not in the paper.
- Re-add removed low-credibility refs (`ammar2025`, pre-2022 orphans).
- Overclaim significance (“all baselines”) without table support.
- Expand scope to EV charging, transport pricing, or unrelated MARL without user request.
- Re-run the full notebook unless Claims Ledger shows mismatches or user asks.

### Voice template

> We propose EG-DQN, a hybrid that injects GLM-derived demand signals into DQN via state augmentation, reward shaping, and action masking. In a synthetic three-product retail simulation with a 120-day holdout, [state headline result from verified tables]. The main evidence for hybrid integration comes from [convergence / ablation / stability — per ledger].

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

---

## 16. Suggested next steps (priority)

1. **User:** Fill Claims Ledger in `blueprint.md` with exported table numbers.
2. **Agent:** Merge Abstract to single paragraph; fix title grammar.
3. **Agent:** Align Results/Conclusion language with verified ledger (soften if adaptive methods tied).
4. **Agent:** De-AI prose pass (remove “promising framework”, “comprehensive study”).
5. **User:** Fill author names and institution before submission.

---

*For citation details → `citations.md` · For polish checklist → `blueprint.md` · For code → `Dynamic_pricing_hybrid_EG_DQN.ipynb`*
