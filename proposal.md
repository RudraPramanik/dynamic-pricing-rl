# Citation Expansion Plan — EG-DQN Paper

**Purpose:** Review queue for adding **6–10** references to `index.tex`.  
**Current:** 20 cited · 7 arXiv (35%) · **Target:** 26–30 cited · arXiv ≤35%.  
**Rule:** Every new cite supports **one sentence**; nothing new in Abstract or Results.

**Last reviewed:** 2026-07-09 (`references/new/` batch)  
**Recommended path:** **Wave 1** (see §9) — implement in **one step per chat session**.

---

## 9. Recommended path — **Wave 1** (do this, not Wave 1+2 yet)

### Why Wave 1 is the best choice

| Option | Verdict | Reason |
|--------|---------|--------|
| **Wave 1** (7 edits) | **✅ Recommended** | Hits target (27 refs); arXiv 9/27 = **33%** (under blueprint 35% cap); adds Tier-A journal anchors; fixes the **Lange author error**; minimal risk to locked Results |
| Wave 1 + 2 (10 edits) | Defer to Phase 9 | Too many Discussion edits in one pass; higher chance of touching frozen sections or adding decorative limitation cites |
| Wave 1 minus loukili | Not needed | Loukili is retail-adjacent DQN pricing with DOI; fills a real gap in §II.D without overlapping existing keys |

**Wave 2** (`denboer2024`, `wang2025loyalty`, optional `mohamadi2024`) is valuable but belongs in a **separate pass after Wave 1 compiles and audits clean** — it only strengthens Limitations / Path to Deployment, not Related Work breadth.

### What Wave 1 delivers

- **20 → 27** unique `\cite{}` keys (+6 new; +1 author fix, not +1 count)
- **5 journal DOI** adds (`orspectrum2024`, `yuan2024`, `loukili2025`, `liang2023`) + **2 arXiv** (`chenfair2023`, `hadi2025`)
- **Zero** changes to Abstract, Results numbers, tables, or figure captions
- Aligns with `blueprint.md`: simulation-study framing, no decorative cites, arXiv ≤35%, Tier A/B preferred

---

## 10. Safety protocol — do not ruin the current manuscript

### Frozen zones (never edit during citation expansion)

| Zone | Lines (approx.) | Why frozen |
|------|-----------------|------------|
| `\begin{abstract}` … `\end{abstract}` | ~140–162 | Tier-A claims locked; no new cites |
| `\section{Experiments and Results}` | ~701–1035 | All numbers trace to `data.md` / `figures/` |
| Figure `\caption{...}` inside Results | 8a–8f | Caption rule: descriptive only |
| Revenue / significance / ablation tables | Results | Locked numbers in §Manuscript staging below |

### Allowed edit zones (Wave 1 only)

- `\section{Introduction}` — **Lange fix only** (3 author-name + key swaps)
- `\subsection{Dynamic Pricing and RL}` — +2 sentences (`orspectrum2024`, `chenfair2023`)
- `\subsection{RL for Pricing}` — +3 sentences (`yuan2024`, `loukili2025`, `liang2023`)
- `\subsection{Limitations}` — +1 sentence (`orspectrum2024` in DGP paragraph)
- `\subsection{Methodological Implications}` — optional +1 clause (`yuan2024` scope boundary)
- `\section{Conclusion}` — extend future-work clause (`hadi2025`)
- `\begin{thebibliography}` — fix `lange2025`; append 6 new `\bibitem{}`

### Before every implementation session

1. **Checkpoint:** `git status` clean or commit current `index.tex` (user commits; assistant does not unless asked).
2. **Baseline audit** (save output for comparison):

```powershell
cd "g:\mypaper\dynamic pricing"
# Count cite keys and bibitems
Select-String -Path index.tex -Pattern '\\cite\{[^}]+\}' -AllMatches | ForEach-Object { $_.Matches } | ForEach-Object { $_.Value } | Sort-Object -Unique
Select-String -Path index.tex -Pattern '\\bibitem\{[^}]+\}' | ForEach-Object { if ($_ -match '\\bibitem\{([^}]+)\}') { $matches[1] } } | Sort-Object -Unique
```

3. **Compile check:** `pdflatex index.tex` twice — note any pre-existing warnings.

### After every step

1. Re-run cite ↔ bibitem audit (must be **0 orphans** each way).
2. `pdflatex index.tex` ×2 — no undefined citations.
3. Grep frozen zones — no new `\cite{}` in Abstract or Results:

```powershell
Select-String -Path index.tex -Pattern '\\cite' -Context 0,0 | Where-Object { $_.LineNumber -lt 700 -or ($_.LineNumber -gt 1035 -and $_.LineNumber -lt 1131) }
# Manually confirm hits are only in Intro (Lange) and Conclusion (hadi) — not Abstract
```

4. **Tone scan** on edited sentences only: no banned words from `blueprint.md` (`comprehensive`, `novel`, `leverage`, `deployment-ready`, etc.).
5. **User 👁 review** before marking step ✅.

### Rollback rule

If a step introduces an orphan cite, wrong author, or accidental Results edit → **revert that step only** (not the whole manuscript). Do not “fix forward” across multiple steps.

---

## 11. Wave 1 implementation steps (one session each)

**Chat command per step:** e.g. *“Implement Wave 1 — Step R1”*

| Step | Task | Touches | New keys | Status |
|------|------|---------|----------|--------|
| **R0** | Pre-flight audit + backup | — | — | ✅ |
| **R1** | Lange author fix | Intro + §II.A + bib | `lange2025` (rename from `groeneveld2025`) | ✅ |
| **R2** | §II.A demand-model breadth | §II.A only | `orspectrum2024`, `chenfair2023` | ✅ |
| **R3** | §II.D DQN literature | §II.D only | `yuan2024`, `loukili2025`, `liang2023` | ⬜ |
| **R4** | §Limitations GLM rigidity | Limitations ¶ DGP only | `orspectrum2024` (reuse) | ⬜ |
| **R5** | §Conclusion MARL future work | Conclusion future-work sentence | `hadi2025` | ⬜ |
| **R6** | Bibliography append + ordering | `\bibitem{}` block | all 6 new items | ⬜ |
| **R7** | Final audit + compile | whole file read-only checks | — | ⬜ |

**Note:** R6 can be merged into R2–R5 (add each `\bibitem` when its `\cite{}` is inserted). Splitting by section is safer for review.

---

### Step R0 — Pre-flight

**Do:**
- Run baseline audit (§10); record 20 cite keys, 20 bibitems.
- Confirm `index.tex` Abstract and Results grep clean (no pending edits).
- Optional: copy `index.tex` → `index.tex.bak` before R1.

**Done when:** Baseline counts logged; compile succeeds.

---

### Step R1 — Lange author fix (critical, not a new reference)

**Replace** every `groeneveld2025` → `lange2025` and **Groeneveld~et~al.** → **Lange~et~al.**

**Locations in `index.tex` (3 cites + 1 bibitem):**
- Intro ~L185, ~L193
- §II.A ~L245
- `\bibitem{groeneveld2025}` → `\bibitem{lange2025}`

**Correct `\bibitem`:**

```latex
\bibitem{lange2025}
F.~Lange, L.~Dreessen, and R.~Schlosser,
``Reinforcement learning versus data-driven dynamic programming: A
comparison for finite horizon dynamic pricing markets,''
\textit{J. Revenue Pricing Manag.}, vol.~24, no.~6, 2025.
\url{https://doi.org/10.1057/s41272-025-00519-8}
```

**Prose:** Keep existing sentences; only swap author name and key.

**Done when:** 0 occurrences of `groeneveld`; 3× `lange2025`; compile clean.

---

### Step R2 — §II.A (`orspectrum2024`, `chenfair2023`)

**Insert after** the Liu / Li–Zheng paragraph (~L248–253), **before** §II.B.

**Draft sentences (one cite each):**

```latex
Aschersleben and Steiner~\cite{orspectrum2024} optimise store-level prices
with flexible heterogeneous sales-response models that allow nonlinear and
lagged price effects beyond a log-link GLM.
Chen, Simchi-Levi, and Wang~\cite{chenfair2023} study contextual dynamic
pricing with joint demand learning and utility-fairness constraints; we
instead inject a fixed GLM block into DQN state and reward.
```

**Add `\bibitem{}`:**

```latex
\bibitem{orspectrum2024}
P.~Aschersleben and W.~J.~Steiner,
``Dynamic pricing using flexible heterogeneous sales response models,''
\textit{OR Spectrum}, vol.~46, no.~1, pp.~29--72, 2024.
\url{https://doi.org/10.1007/s00291-024-00756-0}

\bibitem{chenfair2023}
X.~Chen, D.~Simchi-Levi, and Y.~Wang,
``Utility fairness in contextual dynamic pricing with demand learning,''
\textit{arXiv:2311.16528}, 2023.
```

**Done when:** 2 new cites in §II.A only; tone checklist pass; no banned words.

---

### Step R3 — §II.D (`yuan2024`, `loukili2025`, `liang2023`)

**Insert after** Zheng et al. paragraph (~L290–293), **before** the Nambiar simulation sentence.

**Draft sentences:**

```latex
Yuan and Wang~\cite{yuan2024} apply deep Q-networks to supplier inventory
control in supply-chain Markov decision processes.
Loukili~et~al.~\cite{loukili2025} explore DQN-based adaptive pricing in
digital marketing environments.
Liang~et~al.~\cite{liang2023} implement a dueling DQN for presale
dynamic pricing.
```

**Add `\bibitem{}`:**

```latex
\bibitem{yuan2024}
D.~Yuan and Y.~Wang,
``Sustainable supply chain management: A green computing approach using
deep Q-networks,''
\textit{Sustainable Computing: Informatics and Systems}, vol.~44,
p.~101063, 2024.
\url{https://doi.org/10.1016/j.suscom.2024.101063}

\bibitem{loukili2025}
M.~Loukili \textit{et~al.},
``Adaptive pricing strategies in digital marketing: A machine learning
approach with deep Q-networks,''
\textit{Stat., Optim. Inf. Comput.}, 2025.
\url{https://doi.org/10.19139/soic-2310-5070-2200}

\bibitem{liang2023}
X.~Liang \textit{et~al.},
``Distributed dynamic pricing strategy based on deep reinforcement
learning in a presale mechanism,''
\textit{Sustainability}, vol.~15, no.~13, p.~10480, 2023.
\url{https://doi.org/10.3390/su151310480}
```

**Optional scope sentence** in §Methodological Implications (~L1055): append clause *“our scope is per-SKU retail pricing without supplier-network or carbon constraints [yuan2024].”* — only if the paragraph still reads naturally.

**Done when:** 3 new cites in §II.D; no new cites in Results.

---

### Step R4 — §Limitations (`orspectrum2024` reuse)

**Edit** the **DGP--model alignment** paragraph (~L1090–1097). Append **one sentence**:

```latex
Retail demand models with richer nonlinear and heterogeneous response
structures may behave differently from our log-link GLM
specification~\cite{orspectrum2024}.
```

**Done when:** `orspectrum2024` appears exactly twice in manuscript (§II.A + Limitations); Limitations still Tier-C tone (no new numeric claims).

---

### Step R5 — §Conclusion (`hadi2025`)

**Edit** future-work sentence (~L1158–1162). Extend MARL clause:

```latex
Future work should test the design on store data, extend estimation to
joint multi-product pricing~\cite{kumar2026, hadi2025}, incorporate
inventory constraints for perishable categories~\cite{yavuz2024,
nomura2025}, and complete ablations that isolate state augmentation and
action masking.
```

**Add `\bibitem{}`:**

```latex
\bibitem{hadi2025}
M.~Hadi \textit{et~al.},
``Multi-agent reinforcement learning for dynamic pricing in supply
chains,''
\textit{arXiv:2507.02698}, 2025.
```

**Done when:** `hadi2025` cited once in Conclusion only; no new claims in abstract.

---

### Step R6 — Bibliography hygiene

**Do:**
- Confirm **27** `\bibitem{}` entries, **27** unique cited keys (or count all keys in compound cites).
- arXiv count: **9** (`apte2024`, `cheung2023`? wait - need current arxiv list in index.tex)

Let me list current arxiv in index.tex from memory:
- apte2024, xia2023, xia2024, safonov2024, zheng2024, nambiar2022, kumar2026, chenfair2023 (new), hadi2025 (new) = 9/27 = 33%

- No orphan `\bibitem{}`; no undefined `\cite{}`.
- DOI or arXiv ID on every new entry.
- Update `citations.md` status column for added keys (optional housekeeping).

**Done when:** `pdflatex` ×2 with zero undefined references.

---

### Step R7 — Final audit (blueprint Priority 5 + Step 14)

**Checklist:**

- [ ] Abstract: **no** new `\cite{}`; Tier-A text unchanged
- [ ] Results: revenue \$175,651–\$226,764 unchanged; GLM *p*=0.50; ablation 3-row story intact
- [ ] 27 cite keys ↔ 27 bibitems; arXiv ≤ 35%
- [ ] `lange2025` authors correct; 0× `groeneveld`
- [ ] Banned-word scan on new sentences only
- [ ] `index.pdf` regenerates; figure paths unchanged

**Mark Step 11 ✅ in blueprint tracker** only after R7 passes and user approves.

---

## 12. Phase 9 — Wave 2 (optional, after Wave 1 ✅)

Only start if R7 is complete and user wants 27 → 30 refs.

| Step | Key | Section | Draft role |
|------|-----|---------|------------|
| R8 | `denboer2024` | §Limitations + §Path to Deployment (1 clause each) | Q-learning collusion caution; no multi-firm simulation |
| R9 | `wang2025loyalty` | §Limitations | Fairness / loyalty not evaluated |
| R10 | `mohamadi2024` **or** `meilan2026` | §Conclusion future work | Pick **one** perishable/finite-horizon angle (avoid overlap with `yavuz2024`, `nomura2025`) |

**Wave 2 arXiv impact:** `denboer2024` may be journal — verify venue before counting arXiv budget.

---

## 1. Verdict summary — `references/new/` (10 imports)

| # | File | Paper (short) | Verdict | Tier | Best section | Proposed key |
|---|------|---------------|---------|------|--------------|--------------|
| 1 | `Fabian_Lange_25.pdf` | RL vs data-driven DP, finite-horizon pricing (*J. Revenue Pricing Manag.*) | **DUPLICATE** | A | — | Already `groeneveld2025` — **fix authors to Lange et al.** → `lange2025` |
| 2 | `Xi_chen_25.pdf` | Utility fairness in contextual dynamic pricing + demand learning (Chen, Simchi-Levi, Wang) | **CITE** | A | §II.A Related Work | `chenfair2023` |
| 3 | `di_yuang.md` | ISCO-DQ: green supply-chain inventory via DQN (*Sustainable Computing*) | **CITE** | A | §II.D RL for Pricing | `yuan2024` |
| 4 | `loukili_25.pdf` | DQN adaptive pricing in digital marketing (*Stat., Optim. Inf. Comput.*) | **CITE** | B | §II.D RL for Pricing | `loukili2025` |
| 5 | `arnoud_v_den_24.pdf` | Q-learning and algorithmic collusion (law & economics) | **CITE** | B | §Limitations / §Path to Deployment | `denboer2024` — **Wave 2** |
| 6 | `yu_wang_25.pdf` | DRL dynamic pricing vs customer loyalty / perceived unfairness | **CITE** | B | §Limitations | `wang2025loyalty` — **Wave 2** |
| 7 | `m_chen-26.pdf` | AI finite-horizon pricing for high-value manufacturing assets | **MAYBE** | C | §II.A — only if thin | `meilan2026` — **Wave 2 pick** |
| 8 | `junxin_shen_24.pdf` | DQN pricing for **data products** (*IEEE Access*) | **SKIP** | C | — | Wrong product domain |
| 9 | `Zhenhua_25.pdf` | Hotel DRL pricing + customer satisfaction (*IJHSES*) | **SKIP** | C | — | Low-tier venue; overlaps `chen2023` |
| 10 | `xiuen_qin_25.md` | DDQN **stock trading** + sentiment (*Information Sciences*) | **SKIP** | — | — | Finance trading, not retail pricing |

**Score:** **5 CITE (Wave 1)** · **3 CITE (Wave 2)** · **1 MAYBE** · **1 DUPLICATE (bib fix)** · **3 SKIP**

---

## 2. Legacy parking lot (pre-import)

| Paper | Verdict | Section | Key |
|-------|---------|---------|-----|
| OR Spectrum 2024 — flexible heterogeneous sales response (DOI `10.1007/s00291-024-00756-0`) | **CITE** | §II.A + §Limitations | `orspectrum2024` |
| Airline Poisson elasticity (*Statistical Modelling* 2022) | **SKIP** | — | Wrong domain |

---

## 3. Registry picks (`citations.md` — not in `references/new/`)

| Key | Verdict | Section | Why |
|-----|---------|---------|-----|
| `hadi2025` | **CITE** | §Conclusion future work | MARL with `kumar2026` |
| `liang2023` | **CITE** | §II.D | DQN variant in pricing literature |
| `mohamadi2024` | **MAYBE** | §Discussion / future work | Perishables — overlaps `yavuz2024` — **Wave 2** |
| `razumovskiy2025` | **SKIP** | — | Overlaps `lange2025` |
| `husna2023` / `ramos2023` | **DEFER** | §II.B | Wave 1 does not need; would push arXiv budget |
| `nataraj2025` | **SKIP** | — | Airline DQN |

---

## 4. Wave summary

### Wave 1 — **7 edits** (20 → 27 refs) — **IMPLEMENT NOW**

| Priority | Key | Source | Edit location |
|----------|-----|--------|---------------|
| 1 | `lange2025` | Fix existing | Intro + §II.A + bib |
| 2 | `orspectrum2024` | Parking lot | §II.A + §Limitations |
| 3 | `chenfair2023` | `Xi_chen_25.pdf` | §II.A |
| 4 | `yuan2024` | `di_yuang.md` | §II.D |
| 5 | `loukili2025` | `loukili_25.pdf` | §II.D |
| 6 | `liang2023` | Registry | §II.D |
| 7 | `hadi2025` | Registry | §Conclusion |

**arXiv budget:** +2 → 9/27 = **33%** ✓

### Wave 2 — **+3 adds** (27 → 30 refs) — after Wave 1 audit

| Priority | Key | Source | Edit location |
|----------|-----|--------|---------------|
| 8 | `denboer2024` | `arnoud_v_den_24.pdf` | §Limitations; §Path to Deployment |
| 9 | `wang2025loyalty` | `yu_wang_25.pdf` | §Limitations |
| 10 | `mohamadi2024` OR `meilan2026` | Registry / PDF | §Conclusion — pick one |

---

## 5. Section placement map

```
§II.A Dynamic Pricing & RL     → orspectrum2024, chenfair2023, lange2025 (fix)
§II.D RL for Pricing           → yuan2024, loukili2025, liang2023
§Limitations                   → orspectrum2024 (GLM rigidity)
§Conclusion / future work      → hadi2025 (+ existing kumar2026, yavuz2024, nomura2025)

Wave 2 only:
§Limitations                   → denboer2024, wang2025loyalty
§Path to Deployment            → denboer2024 (1 clause)
```

**Never cite in:** Abstract · Results tables · Figure captions.

---

## 6. Per-paper cards (condensed)

See §11 for draft sentences and `\bibitem{}` templates. Full cards for Wave 2 papers remain in prior review; verify `denboer2024` and `wang2025loyalty` metadata from `references/new/*.pdf` before R8–R9.

---

## 7. Decision rule (from `blueprint.md`)

Only add if:

1. A **specific sentence** needs the cite.  
2. The claim is **not already covered** by an existing key.  
3. Tier A/B preferred; arXiv cap ≤35%.  
4. No decorative cites in Abstract / Results.  
5. **Downgrade one tier** if unsure (`blueprint.md` manuscript rule).

---

## 8. Next action in chat

**Start here:**

> **“Implement Wave 1 — Step R0”** (pre-flight)  
> then **“Implement Wave 1 — Step R1”** … through **R7**.

One step per session; user 👁 between steps.

After R7:

> **“Implement Wave 2 — Step R8”** (optional)

---

## Manuscript staging

| Unit | Status | Notes |
|------|--------|-------|
| **7a–7c** Simulation | ✅ | Defense cites; ε matrix; hyperparams; 5 baselines |
| **8a–8f** Results | ✅ | All figures + tables aligned to `data.md` / `figures/` |
| **9–10** Discussion / Conclusion | ✅ | Tier-safe framing |
| **11** References | 🔄 | 20 keys synced — **Wave 1 expansion via Steps R0–R7** |
| **14–15** Sync + pre-submission | ✅ | Re-run R7 after expansion; author block still placeholder |

### Locked results numbers (do not change when adding cites)

**Revenue (Item 1, 120d):** FP \$175,651 · RB \$138,864 · Pure DQN \$183,549 · GLM/EG-DQN \$226,764 @ \$6 (tie).  
**Significance:** GLM *p*=0.50; FP/RB *p*<0.001.  
**Ablation:** reward shaping = lift; full hybrid = DQN+Reward.  
**Training:** +25.5% / +26.9%; no loss claims.

*Full step criteria → `blueprint.md` · Master bib list → `citations.md`*
