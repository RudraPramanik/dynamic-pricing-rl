# Citation Expansion Plan — EG-DQN Paper

**Purpose:** Review queue for adding **6–10** references to `index.tex`.  
**Current:** 20 cited · 7 arXiv (35%) · **Target:** 26–30 cited · arXiv ≤35%.  
**Rule:** Every new cite supports **one sentence**; nothing new in Abstract or Results.

**Last reviewed:** 2026-07-09 (`references/new/` batch)

---

## 1. Verdict summary — `references/new/` (10 imports)

| # | File | Paper (short) | Verdict | Tier | Best section | Proposed key |
|---|------|---------------|---------|------|--------------|--------------|
| 1 | `Fabian_Lange_25.pdf` | RL vs data-driven DP, finite-horizon pricing (*J. Revenue Pricing Manag.*) | **DUPLICATE** | A | — | Already `groeneveld2025` — **fix authors to Lange et al.** |
| 2 | `Xi_chen_25.pdf` | Utility fairness in contextual dynamic pricing + demand learning (Chen, Simchi-Levi, Wang) | **CITE** | A | §II.A Related Work; optional §Discussion (fairness) | `chenfair2023` |
| 3 | `di_yuang.md` | ISCO-DQ: green supply-chain inventory via DQN (*Sustainable Computing*) | **CITE** | A | §II.D RL for Pricing; §Discussion / future work | `yuan2024` |
| 4 | `loukili_25.pdf` | DQN adaptive pricing in digital marketing (*Stat., Optim. Inf. Comput.*) | **CITE** | B | §II.D RL for Pricing | `loukili2025` |
| 5 | `arnoud_v_den_24.pdf` | Q-learning and algorithmic collusion (law & economics) | **CITE** | B | §Limitations / §Path to Deployment (caution only) | `denboer2024` |
| 6 | `yu_wang_25.pdf` | DRL dynamic pricing vs customer loyalty / perceived unfairness | **CITE** | B | §Limitations (fairness); not Results | `wang2025loyalty` |
| 7 | `m_chen-26.pdf` | AI finite-horizon pricing for high-value manufacturing assets | **MAYBE** | C | §II.A (finite horizon) — add only if §II still thin | `meilan2026` |
| 8 | `junxin_shen_24.pdf` | DQN pricing for **data products** (*IEEE Access*) | **SKIP** | C | — | Wrong product domain |
| 9 | `Zhenhua_25.pdf` | Hotel DRL pricing + customer satisfaction (*IJHSES*) | **SKIP** | C | — | Low-tier venue; overlaps `chen2023` |
| 10 | `xiuen_qin_25.md` | DDQN **stock trading** + sentiment (*Information Sciences*) | **SKIP** | — | — | Finance trading, not retail pricing |

**Score:** **5 CITE** · **1 MAYBE** · **1 DUPLICATE (bib fix)** · **3 SKIP**

### Critical bib fix (not a new reference)

`Fabian_Lange_25.pdf` is the published version of the paper already cited as `groeneveld2025` (DOI `10.1057/s41272-025-00519-8`). **Authors are Lange, Dreessen, and Schlosser** — not Groeneveld. Rename key to `lange2025` or keep key and correct `\bibitem` + in-text “Lange et al.”

---

## 2. Legacy parking lot (pre-import)

| Paper | Verdict | Section | Key |
|-------|---------|---------|-----|
| OR Spectrum 2024 — flexible heterogeneous sales response (DOI `10.1007/s00291-024-00756-0`) | **CITE** | §II.A + §Discussion (richer demand vs GLM) | `orspectrum2024` |
| Airline Poisson elasticity (*Statistical Modelling* 2022) | **SKIP** | — | Wrong domain |

---

## 3. Registry picks (`citations.md` — not in `references/new/`)

| Key | Verdict | Section | Why |
|-----|---------|---------|-----|
| `hadi2025` | **CITE** | §Conclusion future work | MARL with `kumar2026` |
| `liang2023` | **CITE** | §II.D | DQN variant in pricing literature |
| `mohamadi2024` | **MAYBE** | §Discussion / future work | Perishables — overlaps `yavuz2024` |
| `razumovskiy2025` | **MAYBE** | §II.A | DP vs RL — overlaps `lange2025` |
| `husna2023` / `ramos2023` | **PICK ONE** | §II.B | Forecasting breadth |
| `nataraj2025` | **SKIP** | — | Airline DQN |

---

## 4. Recommended implementation waves

### Wave 1 — **7 adds** (20 → 27 refs) — recommended default

| Priority | Key | Source | Edit location | Draft sentence role |
|----------|-----|--------|---------------|---------------------|
| 1 | `lange2025` | Fix existing | §II.A + bib | Correct author; same DP-vs-RL point |
| 2 | `orspectrum2024` | Parking lot | §II.A + §Limitations | Flexible demand response beyond log-link GLM |
| 3 | `chenfair2023` | `Xi_chen_25.pdf` | §II.A | Contextual pricing with demand learning + fairness constraints |
| 4 | `yuan2024` | `di_yuang.md` | §II.D + §Discussion | DQN for supply-chain inventory (adjacent to retail RL) |
| 5 | `loukili2025` | `loukili_25.pdf` | §II.D | DQN for adaptive e-commerce pricing |
| 6 | `liang2023` | Registry | §II.D | Dueling DQN in presale pricing |
| 7 | `hadi2025` | Registry | §Conclusion | MARL supply-chain extension |

**arXiv budget:** +2 arXiv (`chenfair2023`, `hadi2025`) → 9/27 = **33%** ✓

### Wave 2 — **+3 adds** (27 → 30 refs) — Discussion depth

| Priority | Key | Source | Edit location |
|----------|-----|--------|---------------|
| 8 | `denboer2024` | `arnoud_v_den_24.pdf` | §Limitations — deployment / competitive RL risks |
| 9 | `wang2025loyalty` | `yu_wang_25.pdf` | §Limitations — fairness / loyalty concerns |
| 10 | `mohamadi2024` OR `meilan2026` | Registry / `m_chen-26.pdf` | §Future work — pick **one** finite-horizon or perishable angle |

**Do not add** Wave 2 items to Abstract or Results.

---

## 5. Section placement map

```
§II.A Dynamic Pricing & RL     → orspectrum2024, chenfair2023, lange2025 (fix)
§II.B Demand Forecasting       → husna2023 OR ramos2023 (optional, pick one)
§II.D RL for Pricing           → yuan2024, loukili2025, liang2023
§Limitations                   → orspectrum2024 (GLM rigidity), denboer2024, wang2025loyalty
§Path to Deployment            → denboer2024 (regulatory caution, 1 clause)
§Conclusion / future work      → hadi2025 (+ existing kumar2026, yavuz2024, nomura2025)
```

**Never cite in:** Abstract · Results tables · Figure captions (interpretation stays in body).

---

## 6. Per-paper cards (new imports)

### `chenfair2023` — **CITE** (strongest new add)

- **Title:** Utility Fairness in Contextual Dynamic Pricing with Demand Learning  
- **Authors:** X. Chen, D. Simchi-Levi, Y. Wang  
- **ID:** arXiv:2311.16528 (2023)  
- **Supports:** Personalized/contextual pricing with online demand learning — top-tier OR neighbour to EG-DQN.  
- **Sentence sketch:** “Contextual dynamic pricing with joint demand learning and fairness constraints has been studied in bandit settings [chenfair2023]; we instead inject a fixed GLM block into DQN state and reward.”

### `yuan2024` — **CITE**

- **Title:** Sustainable supply chain management: A green computing approach using deep Q-networks  
- **Authors:** D. Yuan, Y. Wang  
- **ID:** DOI `10.1016/j.suscom.2024.101063`  
- **Supports:** DQN + MDP for inventory / supply chain (not SKU retail, but credible DQN-in-operations cite).  
- **Sentence sketch:** “DQN has also been applied to inventory control in supply-chain MDPs [yuan2024]; our focus is per-SKU retail pricing without carbon or supplier-network constraints.”

### `loukili2025` — **CITE**

- **Title:** Adaptive Pricing Strategies in Digital Marketing: A Machine Learning Approach with Deep Q-Networks  
- **Authors:** M. Loukili et al.  
- **ID:** DOI `10.19139/soic-2310-5070-2200`  
- **Supports:** DQN for dynamic digital-marketing pricing — retail-adjacent.  
- **Sentence sketch:** “DQN-based adaptive pricing has been explored in digital marketing settings [loukili2025].”

### `denboer2024` — **CITE** (Discussion only)

- **Title:** Artificial Collusion: Examining Supracompetitive Pricing by Q-Learning Algorithms  
- **Authors:** A. V. den Boer, J. M. Meylahn, M. P. Schinkel  
- **Supports:** Caution on deployed Q-learning pricing — strengthens “not deployment-ready” limitation.  
- **Sentence sketch:** “Legal and economic work cautions that Q-learning pricing rules may raise collusion concerns under specific market structures [denboer2024]; our simulation does not model multi-firm strategic interaction.”

### `wang2025loyalty` — **CITE** (Discussion only)

- **Title:** Impact of Dynamic Pricing Strategies Based on DRL on Customer Loyalty  
- **ID:** DOI `10.1145/3768801.3768868`  
- **Supports:** Fairness / loyalty risks of DRL pricing — limitation, not a method claim.  
- **Sentence sketch:** “DRL pricing can interact with perceived fairness and loyalty [wang2025loyalty], which our revenue-only simulation does not evaluate.”

### `Fabian_Lange_25.pdf` — **DUPLICATE / FIX**

- Same paper as current `groeneveld2025`. Update bibliography authors; do not count as +1 reference.

### **SKIP** — `junxin_shen_24`, `Zhenhua_25`, `xiuen_qin_25`

- Data-product marketplace, low-tier hotel study, and stock trading respectively — poor fit for retail SKU EG-DQN narrative.

### **MAYBE** — `m_chen-26.pdf`

- Finite-horizon AI pricing for manufacturing assets — add only if Wave 1 still leaves §II.A thin.

---

## 7. Decision rule (unchanged)

Only add if:

1. A **specific sentence** needs the cite.  
2. The claim is **not already covered** by an existing key.  
3. Tier A/B preferred; arXiv cap ≤35%.  
4. No decorative cites in Abstract / Results.

---

## 8. Next action in chat

Reply with one of:

- **“Implement Wave 1”** — edit `index.tex` §II + §Conclusion + bibliography (+ Lange author fix).  
- **“Implement Wave 1 + 2”** — full 10-ref expansion.  
- **“Wave 1 minus loukili”** — conservative 6-add variant.

---

## Manuscript staging (unchanged)

| Unit | Status | Notes |
|------|--------|-------|
| **7a–7c** Simulation | ✅ | Defense cites; ε matrix; hyperparams; 5 baselines |
| **8a–8f** Results | ✅ | All figures + tables aligned to `data.md` / `figures/` |
| **9–10** Discussion / Conclusion | ✅ | Tier-safe framing |
| **11** References | ✅ | 20 keys — **expansion pending Wave 1** |
| **14–15** Sync + pre-submission | ✅ | Author block still placeholder |

### Locked results numbers (do not change when adding cites)

**Revenue (Item 1, 120d):** FP \$175,651 · RB \$138,864 · Pure DQN \$183,549 · GLM/EG-DQN \$226,764 @ \$6 (tie).  
**Significance:** GLM *p*=0.50; FP/RB *p*<0.001.  
**Ablation:** reward shaping = lift; full hybrid = DQN+Reward.  
**Training:** +25.5% / +26.9%; no loss claims.

*Full step criteria → `blueprint.md` · Master bib list → `citations.md`*
