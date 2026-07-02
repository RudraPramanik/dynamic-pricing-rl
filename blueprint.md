# Paper Polish Blueprint — EG-DQN Simulation Study

**Goal:** Pass review by presenting an honest, defensible **experimental simulation study** — not an overclaimed “new best pricing method.”

**Positioning target:** The paper should read as **economically grounded pricing research with RL integration**, not a generic “RL simulation paper” or a breakthrough RL algorithm.

**Authoritative evidence (locked 2026-07-02):**
- `data.md` — notebook stdout exports
- `figures/` — all 7 figures from the same research run
- **`figures/17.png` overrides `data.md` on Pure DQN revenue** (table row is wrong)

**Files in scope:**
- `main.tex` — manuscript (**must be rewritten to match evidence**)
- `Dynamic_pricing_hybrid_EG_DQN.ipynb` — experiment source
- `data.md`, `figures/` — **source of truth for all numbers**
- `critic.md`, `context.md` — framing and agent rules
- `references/` — PDFs for citation verification

**Working principle:** Every sentence that reports a number or claims “significance” must trace to `data.md` or a figure in `figures/`. No number in the abstract unless it appears in the updated Results tables.

**Last updated:** 2026-07-02 — Priority 0 **complete**; ledger locked; ready for manuscript pass.

---

## Priority 0 — Evidence audit ✅ COMPLETE

| Step | Status |
|------|--------|
| Authoritative run identified | ✅ `data.md` + `figures/` |
| All figures collected | ✅ 7/7 in `figures/` |
| Claims Ledger finalized | ✅ Below |
| Headline framing chosen | ✅ **Branch C** (mixed holdout — see Framing) |
| `main.tex` vs evidence | ❌ **Major mismatch** — full table rewrite required |

### Figure inventory (authoritative)

| File | Role | Use in `main.tex` |
|------|------|-------------------|
| `figures/eda.png` | EDA 3×3 panel | `\includegraphics{figures/eda.png}` |
| `figures/8.png` | Elasticity validation | `\includegraphics{figures/8.png}` |
| `figures/10.png` | GLM forecast Item 1 | `\includegraphics{figures/10.png}` |
| `figures/15.png` | Training convergence | `\includegraphics{figures/15.png}` |
| `figures/17.png` | Revenue 2×2 panel | `\includegraphics{figures/17.png}` |
| `figures/20.png` | Ablation (3 configs) | `\includegraphics{figures/20.png}` |
| `figures/21.png` | Rolling elasticity | `\includegraphics{figures/21.png}` |

**Action:** Update all `\includegraphics` paths in `main.tex` to `figures/` prefix (or copy files to project root — pick one convention).

### Known data quality notes (document, do not hide)

| Issue | Evidence | Manuscript handling |
|-------|----------|---------------------|
| `data.md` Pure DQN row wrong | Table says \$226,764 @ \$6; `17.png` shows \$183,549 @ \$9.1 | **Use `17.png`**; fix `data.md` for records |
| Significance vs Pure DQN | `data.md` *p*=0.50 (artifact of wrong row) | **Re-run** Mann–Whitney or compute from daily series; until then: report FP/RB only as confirmed |
| `Loss=nan` in training | `data.md` cell 15 | Do not claim stable TD-loss; caption `15.png` carefully |
| GLM level misfit | `8.png` — curves above true demand | One Limitations sentence (elasticity OK, level biased) |
| Test forecast bias | `10.png` — mean residual ≈ −4.57 | Report MAPE 5.20%; note slight over-forecast |
| GLM-optimal price collapse | `data.md` cell 11 — \$6.00 constant | Limitations: grid minimum / corner solution |
| `main.tex` ablation (5 variants) | Not in this run; `20.png` has 3 configs | **Replace** ablation table entirely |

---

## Locked headline framing — Branch C

**Holdout (Item 1, 120 days):**
- GLM-Only and EG-DQN **tie** at **\$226,764** (+29.1% vs fixed-price) — same \$6 constant price.
- EG-DQN **does not** outperform GLM on holdout revenue (Mann–Whitney *p*=0.5004, CI [0, 0]).
- Pure DQN **underperforms** GLM/EG-DQN (\$183,549, +4.5% vs fixed) — separate price (\$9.1).
- Rule-based **underperforms** fixed-price (−20.9%) — high price (\$16) hurts revenue.

**Training / ablation:**
- EG-DQN achieves higher **training** episode rewards than Pure DQN (Last20: \$2.00M vs \$1.96M).
- Ablation (`20.png`): **reward shaping** drives holdout lift (+30.5% vs Pure DQN baseline \$173,830); EG-DQN = DQN+Reward on test.
- **Do not claim:** ep 35 vs 68, 61% variance reduction, state augmentation as largest holdout contributor (not in this ablation design).

**Significance (confirmed from `data.md` cell-18):**
- ✅ EG-DQN vs Fixed-Price: *p*<0.001, *d*=2.495, CI [+421.5, +437.4]
- ✅ EG-DQN vs Rule-Based: *p*<0.001, *d*=4.628, CI [+725.6, +751.7]
- ❌ EG-DQN vs GLM-Only: *p*=0.5004 — **not significant**
- ❓ EG-DQN vs Pure DQN: *p*=0.5004 in export — **stale**; re-run before claiming

---

## Authoritative numbers — copy into `main.tex`

### Table: Revenue — 120-day holdout, Item 1

*Source: `figures/17.png` Panel D (authoritative). Daily avg = Total / 120.*

| Strategy | Total (\$) | Daily (\$) | vs Fixed-Price | Avg Price (\$) |
|----------|------------|------------|----------------|----------------|
| Fixed-Price | 175,651 | 1,464 | — | 10.00 |
| Rule-Based | 138,864 | 1,157 | −20.9% | 16.00 |
| Pure DQN | 183,549 | 1,530 | +4.5% | 9.10 |
| GLM-Only | 226,764 | 1,890 | +29.1% | 6.00 |
| **EG-DQN** | **226,764** | **1,890** | **+29.1%** | **6.00** |

*Replace entire `main.tex` revenue table. Remove CV-price column unless recomputed from daily price series.*

### Table: Statistical significance — EG-DQN vs baselines

*Source: `data.md` cell-18. Report only confirmed rows until Pure DQN is re-tested.*

| Baseline | *p*-value | Cohen's *d* | Effect | 95% CI (daily \$ diff) |
|----------|-----------|-------------|--------|-------------------------|
| Fixed-Price | <0.001 | 2.495 | Large | [+421.5, +437.4] |
| Rule-Based | <0.001 | 4.628 | Large | [+725.6, +751.7] |
| GLM-Only | 0.5004 | 0.000 | — | [0.0, 0.0] |
| Pure DQN | — | — | — | **Re-run required** |

### Table: Ablation — 3 configurations (this run)

*Source: `figures/20.png` Panel B. 200 training episodes per config.*

| Variant | Test revenue (\$) | vs Pure DQN |
|---------|---------------------|-------------|
| Pure DQN | 173,830 | — |
| DQN + Reward | 226,764 | +30.5% |
| Full EG-DQN | 226,764 | +30.5% |

*Remove `main.tex` 5-row ablation (State / Mask variants). Interpretation: econometric **reward shaping** accounts for holdout gain; full hybrid does not exceed reward-only on test.*

### Table: GLM elasticity estimation

*Source: `data.md` cell 7 + `figures/8.png`.*

| Product | True ε | Est. ε̂ | Rel. bias |
|---------|--------|--------|-----------|
| Item 1 | −1.50 | −1.519 | 1.2% |
| Item 2 | −1.80 | −1.770 | 1.7% |
| Item 3 | −1.20 | −1.245 | **3.7%** |

*Do not claim “bias < 1.44% for all products.” Use “within 3.7%” or per-product values.*

### Table: GLM forecast — Item 1 holdout

*Source: `data.md` cell 9 + `figures/10.png`.*

| Metric | Train | Test (120-day) |
|--------|-------|----------------|
| MAE | 4.80 units/day | 5.81 |
| RMSE | 6.17 | 7.07 |
| MAPE | 5.22% | **5.20%** |
| R² | 0.983 | **0.965** |

*Replace `main.tex` forecast table/caption (7.2% / 0.944). Note mean test residual ≈ −4.57 (slight over-forecast).*

### Training statistics (prose only — no ep 35/68)

*Source: `data.md` cells 15–15b + `figures/15.png`.*

| Metric | Pure DQN | EG-DQN |
|--------|----------|--------|
| First-20 ep avg reward | \$1,564,257 | \$1,579,526 |
| Last-20 ep avg reward | \$1,963,282 | \$2,003,676 |
| Improvement (first→last) | +25.5% | +26.9% |
| Best episode reward | \$1,976,095 | \$2,017,148 |

---

## Claims Ledger — FINAL

| # | Old `main.tex` claim | Authoritative evidence | Status | Manuscript action |
|---|---------------------|------------------------|--------|-------------------|
| 1 | EG-DQN +18.3% vs FP | +29.1% (\$226,764 vs \$175,651) | ❌ Wrong | **Replace** with +29.1% |
| 2 | GLM +13.9%, DQN +11.9% | GLM +29.1%; DQN +4.5% | ❌ Wrong | **Replace** per revenue table |
| 3 | EG-DQN highest at \$151,940 | Ties GLM at \$226,764 | ❌ Wrong | **Remove** “beats GLM”; state tie |
| 4 | Significant vs all baselines | FP/RB yes; GLM no | ❌ Overclaim | **Significance table**: 2 rows only (+ note Pure DQN TBD) |
| 5 | Convergence ep 35 vs 68 | Not in exports | ❌ Unsupported | **Delete** from Abstract/Results/Conclusion |
| 6 | 61% variance reduction | Not in exports | ❌ Unsupported | **Delete** |
| 7 | State augmentation largest ablation gain | Ablation has no state-only variant | ❌ Unsupported | **Reframe**: reward shaping drives holdout lift |
| 8 | Elasticity bias < 1.44% all items | Max 3.7% (Item 3) | ⚠️ Partial | **Per-product** biases; fix caption |
| 9 | MAPE 7.83% / R² 0.944 | MAPE 5.20%, R² 0.965 | ❌ Wrong | **Replace** forecast numbers |
| 10 | GLM = EG-DQN on holdout | `17.png` identical bars | ✅ Supported | **Add** to Results + Limitations |
| 11 | EG-DQN > Pure DQN on holdout | \$226,764 vs \$183,549 | ✅ Supported | **Report**; re-run significance |
| 12 | Rule-based beats FP | −20.9% | ✅ Supported | **Report** honestly (heuristic fails) |
| 13 | Training: EG-DQN > Pure DQN | Last20 rewards higher | ✅ Supported | **Lead** training-layer evidence |
| 14 | Ablation: reward drives +30.5% | `20.png` | ✅ Supported | **Emphasize** in Results/Discussion |
| 15 | Stable TD-loss / convergence caption | `Loss=nan` | ❌ Unsupported | **Soften** Fig. 15 caption |

---

## What to DELETE from `main.tex` (non-negotiable)

These numbers are **not from this run** and must not remain:

- Revenue table: \$128,450 – \$151,940 block
- Significance table: GLM *p*=0.003, Pure DQN *p*<0.001, Cohen's *d* = 0.31 / 0.47 as currently written
- Ablation table: 5 variants (\$143,760 – \$151,940; conv. ep 35/68; σ 0.31/0.79)
- Abstract/Conclusion: +18.3%, “highest revenue,” “significant vs all baselines,” 1.9× convergence, 61% variance
- Forecast table: MAPE 7.2% / 7.83%, R² 0.944 in caption
- Elasticity table: −1.488 etc. (replace with authoritative estimates)
- Prose: “EG-DQN surpasses GLM by 3.1 pp,” “diverges after day 20” (verify — GLM and EG-DQN overlap in Panel A)

---

## Defensible framing — use everywhere

### Lead with ✅

1. **EG-DQN design** — three integration mechanisms (state, reward, masking); honest ablation of what each contributes in **this** run.
2. **Simulation benchmark** — 3 products, 1,460 days, M5-calibrated DGP, known ε, 120-day holdout, five strategies.
3. **Evaluation protocol** — Mann–Whitney + bootstrap + Cohen's *d* (report honestly).
4. **Findings (scoped):**
   - Econometric-guided policies (GLM, EG-DQN) match grid-minimum price and maximize holdout revenue vs naive baselines.
   - EG-DQN matches GLM on holdout but exceeds Pure DQN in revenue and training rewards.
   - Reward shaping explains holdout gain in ablation.

### Do not headline ❌

- EG-DQN beats GLM on holdout
- “Statistically significant vs all baselines”
- +18.3% (use +29.1% vs fixed, with simulation scope)
- Convergence ep 35/68 or 61% variance reduction
- State augmentation as primary holdout driver
- Deployment readiness

### Locked abstract template (draft from ledger)

> We propose EG-DQN (Econometric-Guided Deep Q-Network), a hybrid retail pricing agent that injects GLM demand signals into a Deep Q-Network via state augmentation, econometric reward shaping, and early action masking. In a simulation study of three substitute products over four years of synthetic daily demand (M5-calibrated DGP, 120-day holdout on Item 1), we compare five strategies: fixed-price, rule-based, GLM-optimal, pure DQN, and EG-DQN. GLM-optimal and EG-DQN achieve identical holdout revenue (\$226,764; +29.1% vs fixed-price at \$175,651), while pure DQN earns less (\$183,549). Mann–Whitney tests confirm significant daily-revenue gains over fixed-price and rule-based baselines (*p*<0.001) but not over GLM-optimal pricing (*p*=0.50). An ablation isolating reward shaping shows a +30.5% holdout gain over pure DQN. Training-phase episode rewards are higher for EG-DQN than for pure DQN. Results are confined to this synthetic, per-item, discrete-price simulation with partially GLM-aligned demand.

*Adjust Pure DQN significance sentence after re-run if needed.*

### Locked contribution statement (Intro — 3 bullets)

1. We propose EG-DQN and three explicit mechanisms for integrating GLM-derived signals into DQN pricing.
2. We implement a reproducible simulation benchmark with known elasticities and a five-strategy evaluation protocol including non-parametric inference and ablation.
3. We show that in this simulation, econometric-guided policies tie on holdout revenue while outperforming naive baselines; reward shaping drives most holdout gain, and EG-DQN improves training rewards relative to pure DQN.

---

## Reviewer risk matrix

| Risk | Severity | Mitigation |
|------|----------|------------|
| Simulation-only | High | Scope claims; cite Nambiar, Xia, Chen in Methods |
| Overclaiming holdout superiority | **Critical** | Ledger Branch C; GLM tie explicit |
| Simulation favors EG-DQN | High | DGP–GLM alignment + \$6 corner solution in Limitations |
| `main.tex` ≠ figures | **Resolved** | Rewrite tables per this blueprint |
| arXiv-heavy bib | Medium | Priority 3 rebalance |
| `Loss=nan` | Medium | Acknowledge; no loss claims |

---

## Priority order — current status

### ✅ Priority 0 — Evidence audit — DONE

### 🔴 Priority 1 — `main.tex` tables & numbers (NEXT)

**Owner:** assistant

- [ ] Replace revenue, significance, ablation, elasticity, forecast tables (use Authoritative numbers above).
- [ ] Point `\includegraphics` to `figures/`.
- [ ] Apply language fixes from `critic.md` (structural identification, theoretical prediction, deployment caption).
- [ ] Add Limitations: GLM–DGP alignment, \$6 corner, level misfit, GLM=EG-DQN tie, `data.md` note on eval.
- [ ] Add simulation-defense sentence in Methods.

### 🔴 Priority 2 — Abstract, Results, Conclusion

**Owner:** assistant

- [ ] Single abstract (locked template above).
- [ ] Results: **Layer 1** holdout (`17.png`); **Layer 2** training (`15.png`) + ablation (`20.png`).
- [ ] Split significance: confirmed (FP, RB) vs not confirmed (GLM).
- [ ] Conclusion: match Branch C; no deleted claims.
- [ ] Contributions → 3 bullets.
- [ ] Title grammar fix.

### 🟡 Priority 3 — References

- [ ] Fix `xiaocheng2023` bibitem; add `chen2023` to Intro; rebalance arXiv.

### 🟢 Priority 4 — Prose & captions

- [ ] De-AI pass; shorten captions; fix Fig. 17 caption (GLM/EG-DQN overlap, not “diverges”).

### ⚪ Priority 5 — Pre-submission

- [ ] Abstract ⊆ Results; all numbers trace to `data.md`/`figures/`; optional notebook re-run for Pure DQN *p*-value.

---

## Optional notebook tasks (not blocking manuscript)

| Task | Why | Blocking? |
|------|-----|-----------|
| Re-run Mann–Whitney EG-DQN vs Pure DQN | Fix stale *p*=0.50 | No — revenue gap is visible in `17.png` |
| Fix `data.md` Pure DQN row | Record-keeping | No |
| Diagnose `Loss=nan` | Reproducibility | No for first draft |
| Extend ablation (state-only, mask-only) | Stronger design story | No — future work or appendix |

**No full re-run required** to proceed with manuscript if we write to `figures/` + `data.md`.

---

## Critic integration tracker

| Item | Status |
|------|--------|
| Remove GLM “ceiling” | ✅ Verify in rewrite |
| Soften “dominate” | ✅ Already in Conclusion |
| Methodological Implications | ✅ Done |
| GLM misspecification limitation | ✅ Add “may advantage guided methods” |
| Remove structural identification | ❌ P1 |
| Emphasize ablation | ✅ Reframe to reward shaping |
| Remove ep 35/68 claims | ❌ Delete in P1–P2 |
| Reference rebalance | 🟡 P3 |
| Simulation defense sentence | ❌ P1 |

---

## Division of labour

| Task | You | Assistant |
|------|-----|-----------|
| Evidence package (`data.md`, figures) | ✅ Done | |
| Re-run significance vs Pure DQN | Optional | Can script if asked |
| Rewrite `main.tex` tables + Results | | ✅ Next |
| Abstract + Conclusion | | ✅ After tables |
| References + prose polish | | ✅ P3–P4 |

---

## Suggested workflow — remaining sessions

### Session 1 (next): Tables + language fixes
1. Replace all Results tables in `main.tex`.
2. Update figure paths to `figures/`.
3. P1 language fixes (structural ID, captions, Limitations).

### Session 2: Abstract + narrative
4. Write abstract from locked template.
5. Rewrite Results prose (two layers).
6. Update Conclusion + Intro contributions.

### Session 3: Polish
7. References, de-AI pass, pre-submission checklist.

---

*Sources: `data.md`, `figures/` (audit 2026-07-02), `critic.md`, `context.md`. Manuscript edits may now proceed per Priority 1.*
