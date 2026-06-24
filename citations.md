# Citation List — EG-DQN Paper (2022–2026)

**Scope:** Real, verifiable sources only. **Publication years 2022–2026.** Prefer **2024–2026 peer-reviewed** work; use arXiv when it is the best topical match (e.g. retail Q-learning baseline).

**Agent workflow:** Pick from Tier A → add `\bibitem` → `\cite{}` → match in-text author names. Do **not** invent DOIs or authors. Verify against `references/*.pdf` when available.

**Style:** IEEE `IEEEtran` manual `\begin{thebibliography}` (see templates in §6).

---

## 1. Priority tiers

| Tier | Meaning | Use when |
|------|---------|----------|
| **A** | Peer-reviewed journal / top OR venue | Primary Related Work, Methods motivation |
| **B** | arXiv or solid specialty journal, topic-direct | Baselines, simulation, MARL, retail RL |
| **C** | In bib but uncited, or tangential | Only if a specific claim needs support |
| **—** | Legacy pre-2022 still in `main.tex` | Replace when editing those sentences |

---

## 2. Master list (recommended citations)

Sorted by year (newest first), then tier.  
**Status:** `in_paper` = already in `main.tex` · `add_next` = add in Phase B · `optional` = cite only if needed

### 2026

| Status | Key | Tier | Authors | Title (short) | Venue | ID | Cite for |
|--------|-----|------|---------|---------------|-------|-----|----------|
| `in_paper` | `kumar2026` | B | K. K. N. Pillai, S. K. Amma | MARL for dynamic pricing: profitability, stability, fairness | arXiv | [2603.16888](https://arxiv.org/abs/2603.16888) | MARL retail; stability/fairness; future work |
| PDF | `references/Krishna Kumar.pdf` | | | | | | |

### 2025

| Status | Key | Tier | Authors | Title (short) | Venue | ID | Cite for |
|--------|-----|------|---------|---------------|-------|-----|----------|
| `in_paper` | `groeneveld2025` | A | J. Groeneveld et al. | RL vs data-driven DP in finite-horizon pricing | *J. Revenue Pricing Manag.* | [10.1057/s41272-025-00519-8](https://doi.org/10.1057/s41272-025-00519-8) | DP vs RL; data-efficiency trade-off |
| `in_paper` | `razumovskiy2025` | B | L. Razumovskiy, N. Karenin | DP vs RL in finite-horizon dynamic pricing | arXiv | [2604.14059](https://arxiv.org/abs/2604.14059) | Systematic DP/RL comparison; simulation framing |
| PDF | `references/Lev Razumovskiy_2026.pdf` | | | | | | |
| `in_paper` | `chenavaz2025` | A | R. Y. Chenavaz, S. Dimitrov | AI and dynamic pricing: systematic literature review | *J. Appl. Econ.* | [10.1080/15140326.2025.2466140](https://doi.org/10.1080/15140326.2025.2466140) | **Open Related Work** — AI + pricing landscape |
| PDF | `references/regis_Y_Chenavaz_25.pdf` | | | | | | |
| `in_paper` | `hadi2025` | B | M. Hadi et al. | MARL for dynamic pricing in supply chains | arXiv | [2507.02698](https://arxiv.org/abs/2507.02698) | MARL supply chain; future work |
| `in_paper` | `zhang2025` | A | W. Zhang, Y. Meng | Time-varying price elasticity forecasting | *SAGE Open Econ.* | [10.1177/14727978251338001](https://doi.org/10.1177/14727978251338001) | Non-stationary elasticity; GLM limitation |
| `in_paper` | `nomura2025` | A | Y. Nomura, Z. Liu, T. Nishi | DRL for pricing + ordering in perishable inventory | *Appl. Sci.* | [10.3390/app15052421](https://doi.org/10.3390/app15052421) | Perishable inventory + PPO; future work |
| PDF | `references/youseke_Nomura_25.pdf` | | | | | | |
| `removed` | ~~`ammar2025`~~ | — | — | Low-credibility venue; deleted from `main.tex` | — | — | — |
| `optional` | `nataraj2025` | A | S. Nataraj et al. | Transfer learning to scale DQNs for airline pricing | *J. Revenue Pricing Manag.* | [10.1057/s41272-024-00493-7](https://doi.org/10.1057/s41272-024-00493-7) | DQN scaling (optional) |

### 2024

| Status | Key | Tier | Authors | Title (short) | Venue | ID | Cite for |
|--------|-----|------|---------|---------------|-------|-----|----------|
| `in_paper` | `liu2024` | A | S. Liu et al. | Omni-channel retail DRL: pricing + inventory | *Expert Syst. Appl.* | [10.1016/j.eswa.2023.122948](https://doi.org/10.1016/j.eswa.2023.122948) | **Retail DRL + inventory** — RL for Pricing § |
| PDF | `references/Liu_24.pdf` | | | | | | |
| `in_paper` | `apte2024` | B | M. Apte et al. | Retail dynamic pricing via Q-Learning | arXiv | [2411.18261](https://arxiv.org/abs/2411.18261) | **Direct baseline** for EG-DQN |
| PDF | `references/mohit_apte.pdf` | | | | | | |
| `in_paper` | `alamdar2024` | A | P. F. Alamdar, A. Seifi | Deep Q-learning: ordering + dynamic pricing | *Int. J. Prod. Econ.* | [10.1016/j.ijpe.2024.109154](https://doi.org/10.1016/j.ijpe.2024.109154) | Joint DQN pricing-inventory |
| `in_paper` | `xia2024` | B | Y. Xia et al. | Simulation benchmarking RL for retail promotions | arXiv | [2405.10469](https://arxiv.org/abs/2405.10469) | Simulation evaluation protocol |
| `in_paper` | `yavuz2024` | A | T. Yavuz, O. Kaya | DRL pricing + inventory of perishables | *Appl. Soft Comput.* | [10.1016/j.asoc.2024.111864](https://doi.org/10.1016/j.asoc.2024.111864) | Perishable DQL/SAC; future work |
| `in_paper` | `zheng2024` | B | Y. Zheng et al. | Dual-agent DRL for pricing + replenishment | arXiv | [2410.21109](https://arxiv.org/abs/2410.21109) | **Hybrid ML demand + DRL** — closest to EG-DQN spirit |
| `in_paper` | `safonov2024` | B | K. Safonov | Neural demand estimation + dynamic pricing in retail | arXiv | [2412.00920](https://arxiv.org/abs/2412.00920) | ML vs econometric demand; motivates hybrid |
| `add_next` | `mohamadi2024` | A | N. Mohamadi et al. | DRL + VMI in perishable supply chain | *Eng. Appl. Artif. Intell.* | [10.1016/j.engappai.2023.107403](https://doi.org/10.1016/j.engappai.2023.107403) | Perishable supply chain RL (optional) |

### 2023

| Status | Key | Tier | Authors | Title (short) | Venue | ID | Cite for |
|--------|-----|------|---------|---------------|-------|-----|----------|
| `in_paper` | `kopalle2023` | A | P. K. Kopalle et al. | Dynamic pricing: definition and future directions | *J. Retailing* | [10.1016/j.jretai.2023.11.003](https://doi.org/10.1016/j.jretai.2023.11.003) | Survey / problem framing |
| `in_paper` | `xiaocheng2023` | A | X. Li, Z. Zheng | Dynamic pricing with external info + inventory | *Manag. Sci.* | [10.1287/mnsc.2023.4963](https://doi.org/10.1287/mnsc.2023.4963) | Online learning + inventory |
| `in_paper` | `cheung2023` | A | W. C. Cheung, D. Simchi-Levi | Nonstationary RL: blessing of optimism | *Manag. Sci.* | [10.1287/mnsc.2023.4704](https://doi.org/10.1287/mnsc.2023.4704) | Nonstationary RL; guided exploration |
| `in_paper` | `bu2023` | A | J. Bu, D. Simchi-Levi, L. Wang | Offline pricing + demand learning (censored) | *Manag. Sci.* | [10.1287/mnsc.2022.4498](https://doi.org/10.1287/mnsc.2022.4498) | Offline demand learning |
| `in_paper` | `chen2023` | A | J. Chen, Y. Xu, P. Yu | RL hotel revenue management (field experiments) | *J. Oper. Manag.* | [10.1002/joom.1276](https://doi.org/10.1002/joom.1276) | Deployment / field evidence |
| `in_paper` | `xia2023` | B | Y. Xia et al. | RetailSynth synthetic retail data | arXiv | [2312.14095](https://arxiv.org/abs/2312.14095) | Simulation calibration |
| `in_paper` | `liang2023` | C | X. Liang et al. | Dueling DQN presale pricing | *Sustainability* | [10.3390/su151310480](https://doi.org/10.3390/su151310480) | DQN variant |
| `in_paper` | `kaur2023` | C | A. Kaur et al. | Actor–critic pricing + demand (energy) | *Energies* | [10.3390/en16145469](https://doi.org/10.3390/en16145469) | Demand model inside RL reward |
| `in_paper` | `rios2023` | A | J. H. Rios, J. R. Vera | Multi-product pricing + inventory (retail chain) | *Comput. Ind. Eng.* | [10.1016/j.cie.2023.109065](https://doi.org/10.1016/j.cie.2023.109065) | Multi-product retail |
| `in_paper` | `ramos2023` | B | P. Ramos et al. | Retail demand forecasting (multivariate TS) | arXiv | [2308.11939](https://arxiv.org/abs/2308.11939) | Forecasting benchmark |
| `in_paper` | `husna2023` | C | A. ul Husna et al. | ML/DL demand forecasting in retail | Springer book ch. | [10.1007/978-3-031-25847-3_24](https://doi.org/10.1007/978-3-031-25847-3_24) | ML forecasting |

### 2022

| Status | Key | Tier | Authors | Title (short) | Venue | ID | Cite for |
|--------|-----|------|---------|---------------|-------|-----|----------|
| `in_paper` | `nambiar2022` | B | M. Nambiar et al. | Offline deep RL for dynamic pricing (credit) | arXiv | [2203.03003](https://arxiv.org/abs/2203.03003) | Offline / simulation-based RL |

---

## 3. Legacy citations (pre-2022) — removed from `main.tex`

These were removed during citation cleanup (2025-06-24). Do not re-add.

| Old key | Year | Replaced by |
|---------|------|-------------|
| `kastius2021` | 2021 | `groeneveld2025`, `razumovskiy2025`, `chenavaz2025` |
| `fokianos2012` | 2012 | `zhang2025` |
| `antipov2021` | 2021 | `safonov2024` |
| `padakandla2021` | 2021 | `cheung2023` |

---

## 4. Do not add (verified but weak fit or low venue)

| Reason | Example |
|--------|---------|
| Not retail | `bae2024` EV charging (`references/Sangjun_Bae_24.pdf`) |
| Transport, not retail | `parishad2025` congestion pricing (`references/Nasser Parishad_25.pdf`) |
| Low-tier / review only | WJARR 2025 e-commerce RL review; Informatica 2026 generic ARL paper |
| Fringe method | PLOS ONE 2025 quantum + RL omnichannel (unless scope widens) |

---

## 5. Where to cite (section map)

### Introduction
- Econometric vs RL split: `kopalle2023`, `chenavaz2025`
- RL sequential pricing: `groeneveld2025`, `kastius2021` → prefer `razumovskiy2025`
- Retail Q-learning baseline: `apte2024`
- DP robustness / hybrid motivation: `groeneveld2025`, `zheng2024`
- Offline simulation: `nambiar2022`, `xia2024`

### Related Work — Dynamic Pricing & RL
- **Lead sentence:** `chenavaz2025` (SLR)
- Competitive / finite-horizon: `groeneveld2025`, `razumovskiy2025`
- Inventory + learning: `xiaocheng2023`, `liu2024`
- MARL retail: `kumar2026`, `hadi2025`

### Related Work — Demand Forecasting
- ML benchmarks: `husna2023`, `ramos2023`, `ammar2025`
- Time-varying elasticity: `zhang2025`
- Synthetic ground truth: `xia2023`
- ML vs econometric: `safonov2024`

### Related Work — Econometric / Hybrid
- Hybrid motivation: `safonov2024`, `zheng2024`
- Censored offline demand: `bu2023`

### Related Work — RL for Pricing
- Retail Q-learning: `apte2024`
- DQN extensions: `liang2023`, `alamdar2024`, `liu2024`
- Actor–critic + demand: `kaur2023`
- Dual-agent hybrid: `zheng2024`

### Methods / Simulation
- DGP / demand theory: `kopalle2023`, `xia2023`
- Nonstationary environment: `cheung2023`

### Results / Discussion
- Stability: `kumar2026`
- Structure reduces exploration: `cheung2023`, `apte2024`

### Future Work
- Field tests: `chen2023`
- MARL: `hadi2025`, `kumar2026`
- Perishable inventory: `yavuz2024`, `nomura2025`

---

## 6. `\bibitem` templates — add_next only

Paste into `main.tex` `\begin{thebibliography}` when citing.

```latex
\bibitem{chenavaz2025}
R.~Y.~Chenavaz and S.~Dimitrov,
``Artificial intelligence and dynamic pricing: A systematic literature
review,''
\textit{J. Appl. Econ.}, vol.~28, no.~1, p.~2466140, 2025.
\url{https://doi.org/10.1080/15140326.2025.2466140}

\bibitem{liu2024}
S.~Liu \textit{et~al.},
``Data-driven dynamic pricing and inventory management of an omni-channel
retailer in an uncertain demand environment,''
\textit{Expert Syst. Appl.}, vol.~244, p.~122948, 2024.
\url{https://doi.org/10.1016/j.eswa.2023.122948}

\bibitem{zheng2024}
Y.~Zheng \textit{et~al.},
``Dual-agent deep reinforcement learning for dynamic pricing and
replenishment,''
\textit{arXiv:2410.21109}, 2024.

\bibitem{safonov2024}
K.~Safonov,
``Neural network approach to demand estimation and dynamic pricing in
retail,''
\textit{arXiv:2412.00920}, 2024.

\bibitem{nataraj2025}
S.~Nataraj \textit{et~al.},
``Transfer learning to scale deep Q networks in the context of airline
pricing,''
\textit{J. Revenue Pricing Manag.}, vol.~24, no.~2, pp.~190--200, 2025.
\url{https://doi.org/10.1057/s41272-024-00493-7}

\bibitem{mohamadi2024}
N.~Mohamadi \textit{et~al.},
``An application of deep reinforcement learning and vendor-managed
inventory in perishable supply chain management,''
\textit{Eng. Appl. Artif. Intell.}, vol.~127, p.~107403, 2024.
\url{https://doi.org/10.1016/j.engappai.2023.107403}
```

---

## 7. Suggested agent order

1. **Add Tier A local PDFs:** `chenavaz2025`, `liu2024` (+ one sentence each in Related Work).
2. **Add hybrid neighbours:** `zheng2024`, `safonov2024` (Econometric + RL for Pricing).
3. **Wire orphans:** `ramos2023`, `ammar2025` → Demand Forecasting; `bu2023`, `rios2023` → inventory/offline.
4. **Phase out pre-2022** keys when touching those sentences (§3).

---

## 8. Audit checklist

```bash
# Every cite has a bibitem
grep -oP '\\cite\{[^}]+\}' main.tex | sort -u
grep '\\bibitem{' main.tex

# No stale keys
grep -E 'charafeddine|elkhatib|Kumar~et' main.tex   # should be empty
```

---

## 9. File map

| Path | Role |
|------|------|
| `main.tex` | Live `\cite{}` + `\bibitem{}` |
| `citations.md` | This list (agent source of truth) |
| `references/*.pdf` | Verify metadata before adding |

---

*Updated: 2025-06-24 · Citation cleanup applied in `main.tex` (27 cited references, 0 orphans).*
