# Citation Guide — EG-DQN Paper

**Purpose:** Single source of truth for references before editing `main.tex`.  
**Style:** IEEE (`IEEEtran` + manual `\begin{thebibliography}`).  
**Workflow:** Gather → verify → fix registry → update `\bibitem{}` → update `\cite{}` → sync in-text names.

---

## 1. Rules for AI agents

### 1.1 Do not guess metadata

- Prefer **local PDF** in `references/` over memory or web snippets.
- If a field is missing from the PDF, mark `status: needs_verification` — do not invent authors, pages, or DOIs.
- **Never** copy a `\bibitem` from another paper without checking the PDF title and author list match.

### 1.2 Cite-key convention

```
{lastname}{year}
```

Examples: `apte2024`, `razumovskiy2025`, `chenavaz2025`, `liu2024`.

- Two papers, same lead author + year → `li2023a`, `li2023b`.
- Rename keys only when fixing a **wrong paper** (e.g. `charafeddine2025` → `razumovskiy2025`) and update **every** `\cite{}` and in-text name.

### 1.3 IEEE `\bibitem` format (match existing `main.tex`)

**Journal article**

```latex
\bibitem{key}
A.~Author, B.~Author, and C.~Author,
``Title in sentence case,''
\textit{Journal Name}, vol.~V, no.~N, pp.~start--end, YEAR.
\url{https://doi.org/10.xxxx/xxxxx}
```

**arXiv preprint**

```latex
\bibitem{key}
A.~Author and B.~Author,
``Title,''
\textit{arXiv:YYYY.NNNNN}, YEAR.
```

**Book chapter**

```latex
\bibitem{key}
A.~Author \textit{et~al.},
``Chapter title,''
in \textit{Book Title} (Publisher), YEAR.
\url{https://doi.org/10.xxxx/xxxxx}
```

**Formatting rules**

| Element | Rule |
|--------|------|
| Authors | Up to 6: list all. 7+: first author + `\textit{et~al.}` |
| Names | `F.~M.~Surname` (IEEE initials) |
| Title | Double backticks ``...'' |
| Journal | `\textit{...}` italic |
| Pages | En-dash: `pp.~580--593` |
| DOI | `\url{https://doi.org/...}` when available |
| `et al.` | `\textit{et~al.}` in author list only |

### 1.4 In-text citation rules

| Situation | LaTeX |
|-----------|-------|
| Numeric cite | `...\cite{apte2024}.` |
| One author | `Apte~\cite{apte2024}` |
| Two authors | `Zhang and Meng~\cite{zhang2025}` |
| Three+ authors | `Groeneveld~\textit{et~al.}~\cite{groeneveld2025}` |
| Possessive | `Apte~\textit{et~al.}~\cite{apte2024}'s` → prefer *the framework of Apte~\textit{et~al.}~\cite{apte2024}* |

**In-text author name must match the `\bibitem`**, not an old/wrong label.

### 1.5 When to cite

- Cite when making a **factual claim** about prior work (method, finding, dataset, benchmark).
- Do **not** cite for generic statements (“RL is popular”) unless the sentence attributes a specific result.
- Every `\cite{key}` in the body must have a matching `\bibitem{key}`.
- Every `\bibitem` should be cited at least once, or moved to **Reserve** (Section 4).

### 1.6 Agent checklist (per reference)

- [ ] PDF or DOI verified
- [ ] Authors match PDF
- [ ] Title matches PDF (minor hyphenation OK)
- [ ] Venue + year correct
- [ ] `cite_key` follows convention
- [ ] `\bibitem` pasted into `main.tex`
- [ ] `\cite{}` added where claim appears
- [ ] In-text name matches bib authors
- [ ] Removed/replaced any wrong duplicate entry

---

## 2. Paper sections → what to cite

| Section | Cite for |
|---------|----------|
| **Introduction** | Econometric vs RL split; retail Q-learning baseline; DP vs RL robustness; offline/simulation RL justification |
| **Related Work — Dynamic pricing & RL** | Survey (`kopalle2023`); competitive RL (`kastius2021`); DP vs RL (`groeneveld2025`, `razumovskiy2025`); inventory + online learning (`li2023` / `xiaocheng2023`); MARL retail (`kumar2026`); **SLR** (`chenavaz2025`) |
| **Related Work — Demand forecasting** | ML benchmarks (`husna2023`); time-varying elasticity (`zhang2025`); synthetic retail data (`xia2023`); optional forecast surveys (`ramos2023`, `ammar2025`) |
| **Related Work — Econometric models** | Log-linear Poisson (`fokianos2012`); GLM as interpretable prior (`antipov2021`); censored demand (`bu2023` — if discussing offline learning) |
| **Related Work — RL for pricing** | Retail Q-learning (`apte2024`); DQN variants (`liang2023`, `alamdar2024`); actor–critic + demand (`kaur2023`); offline RL (`nambiar2022`); MARL supply chain (`hadi2025`); omni-channel DRL (`liu2024`) |
| **Methods / Simulation** | DGP theory (`kopalle2023`, `fokianos2012`); calibration (`xia2023`) |
| **Results / Discussion** | MARL dimensions (`kumar2026`); nonstationary RL (`cheung2023`); hybrid prior (`apte2024`, `antipov2021`) |
| **Future work** | Field experiments (`chen2023`); simulation benchmark (`xia2024`); MARL (`hadi2025`, `kumar2026`); perishable inventory (`nomura2025`, `yavuz2024`) |

---

## 3. Master reference registry

**Legend**

| Status | Meaning |
|--------|---------|
| `verified` | Metadata checked against PDF or publisher |
| `needs_fix` | In `main.tex` but wrong authors/title/key |
| `to_add` | PDF in `references/`; not yet in bib |
| `in_bib_only` | In bib; not cited in body — cite or move to Reserve |
| `optional` | Citable but low relevance to retail EG-DQN |
| `reserve` | Keep for later; do not cite until needed |

---

### 3.1 Local PDFs (`references/`)

| Status | cite_key | Local PDF | Authors (verified) | Title (short) | Venue | Year | DOI / ID | Relevance |
|--------|----------|-----------|-------------------|---------------|-------|------|----------|-----------|
| `verified` | `apte2024` | `mohit_apte.pdf` | M. Apte, K. Kale, P. Datar, P. R. Deshmukh | Dynamic retail pricing via Q-Learning | arXiv preprint | 2024 | arXiv:2411.18261 | **High** — direct retail Q-learning baseline |
| `needs_fix` | `kumar2026` → keep key or `pillai2026` | `Krishna Kumar.pdf` | **K. K. N. Pillai, S. K. Amma** (not “A. Kumar”) | Multi-agent RL for dynamic pricing: profitability, stability, fairness | arXiv + IJSR | 2025/2026 | arXiv:2603.16888 | **High** — MARL retail; fix author line |
| `needs_fix` | `charafeddine2025` → **`razumovskiy2025`** | `Lev Razumovskiy_2026.pdf` | **L. Razumovskiy, N. Karenin** (not Charafeddine) | DP vs RL in finite-horizon dynamic pricing | arXiv | 2025 | arXiv:2604.14059 | **High** — rename key + fix Related Work sentence |
| `needs_fix` | `elkhatib2025` → **`nomura2025`** | `youseke_Nomura_25.pdf` | **Y. Nomura, Z. Liu, T. Nishi** (not El Khatib) | DRL for dynamic pricing and ordering in perishable inventory | *Appl. Sci.* | 2025 | 10.3390/app15052421 | **Medium** — future work; fix authors |
| `to_add` | `chenavaz2025` | `regis_Y_Chenavaz_25.pdf` | R. Y. Chenavaz, S. Dimitrov | AI and dynamic pricing: systematic literature review | *J. Appl. Econ.* | 2025 | 10.1080/15140326.2025.2466140 | **High** — SLR for Related Work opening |
| `to_add` | `liu2024` | `Liu_24.pdf` | S. Liu, J. Wang, R. Wang, Y. Zhang, Y. Song, L. Xing | Data-driven dynamic pricing and inventory (omni-channel retailer) | *Expert Syst. Appl.* | 2024 | 10.1016/j.eswa.2023.122948 | **High** — retail DRL + inventory |
| `optional` | `bae2024` | `Sangjun_Bae_24.pdf` | S. Bae, B. Kulcsár, S. Gros | Personalized dynamic pricing for EV charging | arXiv | 2024 | arXiv:2401.00661 | **Low** — EV charging, not retail |
| `optional` | `parishad2025` | `Nasser Parishad_25.pdf` | N. Parishad, M. Yildirimoglu, M. Hickman | Congestion pricing via DRL (transport networks) | *Transp. Res. Part C* | 2025 | 10.1016/j.trc.2025.105166 | **Low** — transport tolls |

---

### 3.2 Already in `main.tex` bibliography (no local PDF)

| Status | cite_key | Authors | Title (short) | Venue | Year | DOI / ID | Cited in body? |
|--------|----------|---------|---------------|-------|------|----------|----------------|
| `verified` | `kopalle2023` | P. K. Kopalle et al. | Dynamic pricing: definition and future directions | *J. Retailing* | 2023 | 10.1016/j.jretai.2023.11.003 | Yes |
| `verified` | `kastius2021` | A. Kastius, R. Schlosser | Dynamic pricing under competition using RL | *J. Revenue Pricing Manag.* | 2021 | — | Yes |
| `verified` | `groeneveld2025` | J. Groeneveld et al. | RL vs data-driven DP for finite-horizon pricing | *J. Revenue Pricing Manag.* | 2025 | 10.1057/s41272-025-00519-8 | Yes |
| `verified` | `husna2023` | A. ul Husna et al. | ML/DL demand forecasting in retail | Springer book ch. | 2023 | 10.1007/978-3-031-25847-3_24 | Yes |
| `verified` | `hadi2025` | M. Hadi et al. | MARL for dynamic pricing in supply chains | arXiv | 2025 | arXiv:2507.02698 | Yes |
| `verified` | `zhang2025` | W. Zhang, Y. Meng | Time-varying price elasticity forecasting | *SAGE Open Econ.* | 2025 | 10.1177/14727978251338001 | Yes |
| `verified` | `kaur2023` | A. Kaur et al. | Actor–critic dynamic pricing (energy) | *Energies* | 2023 | 10.3390/en16145469 | Yes |
| `verified` | `nambiar2022` | M. Nambiar et al. | Offline deep RL for dynamic pricing (credit) | arXiv | 2022 | arXiv:2203.03003 | Yes |
| `in_bib_only` | `ramos2023` | P. Ramos et al. | Retail demand forecasting (multivariate TS) | arXiv | 2023 | arXiv:2308.11939 | **No** |
| `in_bib_only` | `ammar2025` | M. Ammar et al. | Systematic review: retail e-commerce forecasting | *Int. J. Sci. Interdiscip. Res.* | 2025 | — | **No** |
| `verified` | `xia2023` | Y. Xia et al. | RetailSynth synthetic data | arXiv | 2023 | arXiv:2312.14095 | Yes |
| `verified` | `fokianos2012` | K. Fokianos, R. Fried | Log-linear Poisson autoregression | *J. Time Ser. Anal.* | 2012 | — | Yes |
| `verified` | `antipov2021` | E. A. Antipov, E. B. Pokryshevskaya | Interpretable ML for demand modeling | *J. Revenue Pricing Manag.* | 2021 | — | Yes |
| `in_bib_only` | `bu2023` | J. Bu, D. Simchi-Levi, L. Wang | Offline pricing with censored data | *Manag. Sci.* | 2023 | 10.1287/mnsc.2022.4498 | **No** |
| `verified` | `liang2023` | X. Liang et al. | Dueling DQN presale pricing | *Sustainability* | 2023 | 10.3390/su151310480 | Yes |
| `verified` | `alamdar2024` | P. F. Alamdar, A. Seifi | DQN for ordering + dynamic pricing | *Int. J. Prod. Econ.* | 2024 | — | Yes |
| `verified` | `chen2023` | J. Chen, Y. Xu, P. Yu | RL hotel revenue management (field experiments) | *J. Oper. Manag.* | 2023 | — | Yes (future work) |
| `verified` | `xia2024` | Y. Xia et al. | Simulation benchmarking RL for retail promotions | arXiv | 2024 | arXiv:2405.10469 | Yes |
| `verified` | `padakandla2021` | S. Padakandla | Survey: RL in nonstationary environments | *ACM Comput. Surv.* | 2021 | — | Yes |
| `verified` | `yavuz2024` | T. Yavuz, O. Kaya | DRL pricing + perishable inventory | *Appl. Soft Comput.* | 2024 | — | Yes (future work) |
| `in_bib_only` | `rios2023` | J. H. Rios, J. R. Vera | Multi-product pricing + inventory (retail chain) | *Comput. Ind. Eng.* | 2023 | — | **No** |
| `needs_fix` | `xiaocheng2023` | **Xiaocheng Li, Zeyu Zheng** (not “M. El Xiaocheng Li”) | Dynamic pricing with external info + inventory | *Manag. Sci.* | 2023 | 10.1287/mnsc.2023.4963 | Yes — **fix author + journal name** |
| `needs_fix` | `cheung2023` | W. C. Cheung, D. Simchi-Levi | Nonstationary RL: blessing of optimism | *Manag. Sci.* | 2023 | 10.1287/mnsc.2023.4704 | Yes — fix journal spelling (`Manag. Sci.`) |

**Note:** `groeneveld2025` and `razumovskiy2025` are **different papers**. Do not merge them.

---

## 4. Ready-to-paste `\bibitem` blocks

### 4.1 Fixes (replace existing entries)

```latex
\bibitem{razumovskiy2025}
L.~Razumovskiy and N.~Karenin,
``A comparative study of dynamic programming and reinforcement learning
in finite-horizon dynamic pricing,''
\textit{arXiv:2604.14059}, 2025.

\bibitem{kumar2026}
K.~K.~N.~Pillai and S.~K.~Amma,
``Multi-agent reinforcement learning for dynamic pricing: Balancing
profitability, stability and fairness,''
\textit{arXiv:2603.16888}, 2026.

\bibitem{nomura2025}
Y.~Nomura, Z.~Liu, and T.~Nishi,
``Deep reinforcement learning for dynamic pricing and ordering policies
in perishable inventory management,''
\textit{Appl. Sci.}, vol.~15, no.~5, p.~2421, 2025.
\url{https://doi.org/10.3390/app15052421}

\bibitem{xiaocheng2023}
X.~Li and Z.~Zheng,
``Dynamic pricing with external information and inventory constraint,''
\textit{Manag. Sci.}, 2023.
\url{https://doi.org/10.1287/mnsc.2023.4963}

\bibitem{cheung2023}
W.~C.~Cheung and D.~Simchi-Levi,
``Nonstationary reinforcement learning: The blessing of (more) optimism,''
\textit{Manag. Sci.}, 2023.
\url{https://doi.org/10.1287/mnsc.2023.4704}

\bibitem{apte2024}
M.~Apte, K.~Kale, P.~Datar, and P.~R.~Deshmukh,
``Dynamic retail pricing via Q-Learning --- A reinforcement learning
framework for enhanced revenue management,''
\textit{arXiv:2411.18261}, 2024.
```

**Migration:** Delete `\bibitem{charafeddine2025}` and `\bibitem{elkhatib2025}`. Replace `\cite{charafeddine2025}` → `\cite{razumovskiy2025}` and `\cite{elkhatib2025}` → `\cite{nomura2025}` everywhere.

### 4.2 New entries (from local PDFs)

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
```

### 4.3 Optional / reserve (add only if cited)

```latex
\bibitem{bae2024}
S.~Bae, B.~Kulcs\'ar, and S.~Gros,
``Personalized dynamic pricing policy for electric vehicles: Reinforcement
learning approach,''
\textit{arXiv:2401.00661}, 2024.

\bibitem{parishad2025}
N.~Parishad, M.~Yildirimoglu, and M.~Hickman,
``Congestion pricing in multi-modal networks: An application of deep
reinforcement learning,''
\textit{Transp. Res. Part C}, vol.~177, p.~105166, 2025.
\url{https://doi.org/10.1016/j.trc.2025.105166}
```

---

## 5. Known errors in current `main.tex` (fix in next pass)

| Issue | Location | Fix |
|-------|----------|-----|
| Wrong paper attributed to Charafeddine | `\bibitem{charafeddine2025}`, Related Work L226–228 | Use `razumovskiy2025`; in-text: *Razumovskiy and Karenin* |
| Broken Related Work sentence | L226–227 | Split into two sentences: Li & Zheng (`xiaocheng2023`); Razumovskiy & Karenin (`razumovskiy2025`) |
| Wrong Kumar authors | `\bibitem{kumar2026}` | Pillai & Amma (Section 4.1) |
| Wrong Nomura authors | `\bibitem{elkhatib2025}` | Rename to `nomura2025` |
| Malformed Li & Zheng entry | `\bibitem{xiaocheng2023}` | Fix authors; journal = *Manag. Sci.* |
| `Management Sciences` typo | `cheung2023`, `xiaocheng2023` | *Manag. Sci.* |
| Orphan bibitems | `ramos2023`, `ammar2025`, `bu2023`, `rios2023` | Cite in Demand Forecasting / Future Work, or remove |

---

## 6. Suggested citation plan (priority order)

### Phase A — Correctness (do first)

1. Replace `charafeddine2025` → `razumovskiy2025` (bib + cites + prose).
2. Fix `kumar2026`, `xiaocheng2023`, `cheung2023`, `apte2024` author lines.
3. Replace `elkhatib2025` → `nomura2025` in Future Work.
4. Repair Related Work paragraph (L222–232).

### Phase B — Add high-value local PDFs

5. Add `chenavaz2025` — open Related Work § Dynamic Pricing (“systematic review of AI in dynamic pricing”).
6. Add `liu2024` — Related Work § RL for Pricing (“omni-channel retail DRL with inventory”).

### Phase C — Clean up orphans

7. Either cite `ramos2023` + `ammar2025` in Demand Forecasting, or move to Reserve.
8. Cite `bu2023` if discussing censored/offline demand; else Reserve.
9. Cite `rios2023` with multi-product inventory discussion, or Reserve.
10. Skip `bae2024` and `parishad2025` unless scope widens beyond retail.

---

## 7. Claim → citation map (current draft)

| Claim in draft | Required key(s) | Status |
|----------------|-----------------|--------|
| Econometric structural pricing / surveys | `kopalle2023` | OK |
| RL as sequential pricing framework | `kastius2021`, `groeneveld2025` | OK |
| Retail Q-learning baseline | `apte2024` | OK — expand authors |
| Offline / simulation RL | `nambiar2022` | OK |
| M5 / RetailSynth calibration | `xia2023` | OK |
| DP vs RL comparison (finite horizon) | `groeneveld2025`, `razumovskiy2025` | **Fix second key** |
| Inventory + external info pricing | `xiaocheng2023` | **Fix bib** |
| MARL profitability / stability / fairness | `kumar2026` | **Fix authors** |
| ML demand forecasting | `husna2023` | OK |
| Time-varying elasticity | `zhang2025` | OK |
| Log-linear Poisson / GLM | `fokianos2012` | OK |
| GLM as prior not standalone optimizer | `antipov2021` | OK |
| DQN / Dueling DQN pricing | `liang2023`, `alamdar2024` | OK |
| Actor–critic + demand integration | `kaur2023` | OK |
| MARL supply chain | `hadi2025` | OK |
| Nonstationary RL / guided exploration | `cheung2023`, `padakandla2021` | **Fix cheung journal** |
| Field experiments / simulation pipeline | `chen2023`, `xia2024` | OK |
| Perishable inventory future work | `yavuz2024`, `nomura2025` | **Fix nomura** |

---

## 8. File map

```
dynamic pricing/
├── main.tex              ← \cite{} and \bibitem{} (IEEE thebibliography)
├── citations.md          ← this guide (registry + rules)
├── references/           ← source PDFs (verify here first)
│   ├── mohit_apte.pdf
│   ├── Krishna Kumar.pdf
│   ├── Lev Razumovskiy_2026.pdf
│   ├── youseke_Nomura_25.pdf
│   ├── regis_Y_Chenavaz_25.pdf
│   ├── Liu_24.pdf
│   ├── Sangjun_Bae_24.pdf      (optional)
│   └── Nasser Parishad_25.pdf  (optional)
└── blueprint.md            ← writing polish; citations tracked here too
```

---

## 9. Next step after this file

When ready, agent should:

1. Apply **Phase A** fixes to `main.tex` only (no new claims).
2. Apply **Phase B** adds + one sentence each in Related Work.
3. Run a grep audit:
   - `grep -o '\\cite{[^}]*}' main.tex` — every key has `\bibitem`
   - `grep '\\bibitem{' main.tex` — every item is cited or marked Reserve
4. Update this file: set `status` to `done` for each completed row.

---

*Last updated: 2025-06-24 — metadata verified from `references/*.pdf` and current `main.tex` bibliography.*
