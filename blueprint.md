# Paper Polish Blueprint — EG-DQN Simulation Study

**Goal:** Pass review by presenting an honest, defensible **experimental simulation study** — not an overclaimed “new best pricing method.”

**Files in scope:**
- `main.tex` — manuscript
- `Dynamic_pricing_hybrid_EG_DQN.ipynb` — experiment source
- Your exported **tables & figures** (authoritative outputs)
- Reference PDFs (when added to `references/`)

**Working principle:** Every sentence that reports a number or claims “significance” must trace to one saved table or figure. No number in the abstract unless it appears in Results.

---

## Do we need to re-run the notebook?

### Short answer: **No full re-run required right now** — if your saved tables/figures are the source of truth.

Re-running is **optional** at this stage. What matters is **consistency**, not re-execution.

| Situation | Action | 
|-----------|--------|
| You have final tables/figures and know which run produced them | **Audit only** — align `main.tex` to those outputs |
| Paper numbers match your saved tables exactly | **Skip re-run** — proceed to writing/citations |
| Paper numbers ≠ saved tables, or you’re unsure which run the figures came from | **Re-run once** (or export tables from the run that made the figures) |
| Reviewer asks for reproducibility / you find a bug (e.g. `Loss=nan`, identical revenues across methods) | **Re-run after fix** — not before polish |

### Recommended now: **Audit pass (30–60 min, no GPU time)**

1. Open your saved revenue table, significance table, and ablation table.
2. Compare every number in `main.tex` (Abstract, Table I–IV, Conclusion) against those files.
3. Fill the **Claims Ledger** below — mark each claim ✅ / ❌ / ⚠️.
4. Only re-run if the ledger shows mismatches you cannot resolve from existing exports.

**Rule:** The figures you already have define the experiment. The text must follow them — not the other way around.

---

## What to focus on NOW (priority order)

### 🔴 Priority 1 — Evidence alignment (before any prose polish)

**Owner:** You (tables) + me (`main.tex` audit)

- [ ] Locate authoritative exports: revenue comparison, significance tests, ablation, convergence.
- [ ] Complete Claims Ledger (Section below).
- [ ] Decide the **headline result** the paper will honestly lead with (see Framing).
- [ ] Flag any figure captions that overstate what the panel shows.

**Do not** rewrite Abstract/Conclusion until this is done.

---

### 🟠 Priority 2 — Fix review blockers in structure

**Owner:** me (with your approval on framing)

- [ ] **Abstract** — one paragraph; remove duplicate/commented block (`//` lines); fix typos; match ledger numbers.
- [ ] **Title** — fix grammar (e.g. “Integrating” not “With Integrating”).
- [ ] **Related Work** — fix broken citations (`Charafeddine` / `xiaocheng2023` / `charafeddine2025` mix-up); tighten positioning.
- [ ] **Results** — split into two layers:
  - **Holdout revenue** (what happened on the 120-day test set)
  - **Training dynamics** (convergence, variance, ablation)
- [ ] **Significance language** — only claim significance for baselines where tests actually reject; do not say “all baselines” unless table shows it.
- [ ] **Limitations** — explicitly state if adaptive methods (GLM / DQN / EG-DQN) tied on holdout.
- [ ] **Conclusion** — soften to “we evaluate / observe / within this simulation.”

---

### 🟡 Priority 3 — References & citations

**Owner:** you (PDFs) + me (integration)

- [ ] Add PDFs to `references/` (no `.md` conversion needed).
- [ ] Fix malformed `\bibitem` entries in `main.tex`.
- [ ] Map each in-text claim → one correct source; remove decorative citations.
- [ ] Add missing citations where Related Work makes claims without support.

---

### 🟢 Priority 4 — De-AI tone & academic polish

**Owner:** me — **only after Priority 1–2**

- [ ] Remove AI tells: “promising framework,” “comprehensive study,” “tightly couples,” stacked superlatives.
- [ ] Use cautious academic voice: “in our simulation,” “relative to,” “suggest,” “consistent with.”
- [ ] Reduce contribution bullets from 4 → **3 precise, defensible** items.
- [ ] Full section pass: Intro → Methods → Results → Discussion.

---

### ⚪ Priority 5 — Pre-submission checklist

- [ ] Abstract claims ⊆ Results claims
- [ ] All table/figure numbers match saved exports
- [ ] Methods parameters match notebook (SEED, episodes, λ, holdout, K=20, per-item training)
- [ ] Limitations cover: synthetic data, single-item RL, discrete actions, tied adaptive outcomes (if true)
- [ ] Figures referenced exist and match captions (`8.png`, `10.png`, `15.png`, `17.png`, `20.png`, `21.png`)
- [ ] Optional: anonymous notebook link or appendix for reproducibility

---

## Defensible framing (use this throughout)

### Lead with ✅

1. **Design** — EG-DQN integrates GLM signals via state augmentation, reward shaping, and action masking (ablated separately).
2. **Benchmark** — reproducible simulation with known ground-truth elasticities (RetailSynth / M5-calibrated style).
3. **Evaluation protocol** — five-strategy comparison + non-parametric tests (Mann–Whitney, bootstrap CI, Cohen’s *d*).
4. **Learning-process evidence** — convergence speed and training variance (if supported by your convergence/ablation figures).

### Keep in background ⚠️ (report, don’t headline)

- Large % revenue lifts vs fixed-price unless clearly scoped (single item, single holdout, simulation only).
- “EG-DQN outperforms GLM and DQN on holdout” — **only if your revenue table shows separation**.
- “Statistically significant against all baselines” — **only if significance table supports it**.
- Deployment / A/B-test readiness — keep in Future Work.

### Honest narrative template (adapt after ledger)

> We propose EG-DQN, a hybrid that injects GLM-derived demand signals into DQN. In a synthetic three-product retail simulation, adaptive policies outperform naive fixed and rule-based baselines. [If tied:] On the holdout window, GLM-optimal, pure DQN, and EG-DQN achieved similar revenue, so we do not claim holdout superiority among adaptive methods. The main evidence for hybrid integration comes from [convergence / ablation / training stability — whichever your tables support].

---

## Claims Ledger (fill this first)

Copy your numbers from saved tables into the “Evidence” column. Mark status before editing text.

| # | Claim in current `main.tex` | Evidence (your table/figure) | Status | Action |
|---|----------------------------|------------------------------|--------|--------|
| 1 | EG-DQN +18.3% vs fixed-price | Revenue table: _____ | ☐ | Keep / revise / drop |
| 2 | GLM +13.9%, DQN +11.9% | Same table: _____ | ☐ | Keep / revise / drop |
| 3 | EG-DQN highest total revenue ($151,940) | Same table: _____ | ☐ | Keep / revise / drop |
| 4 | Significant vs **all** baselines incl. GLM ($p=0.003$) | Significance table: _____ | ☐ | Keep / revise / drop |
| 5 | EG-DQN 1.9× faster convergence (ep 35 vs 68) | Convergence fig / ablation: _____ | ☐ | Keep / revise / drop |
| 6 | 61% lower reward variance (EG-DQN vs DQN) | Ablation / training log: _____ | ☐ | Keep / revise / drop |
| 7 | State augmentation = largest ablation contributor | Ablation table: _____ | ☐ | Keep / revise / drop |
| 8 | GLM elasticity recovery (bias < 1.44%) | Elasticity fig (`8.png`): _____ | ☐ | Keep / revise / drop |
| 9 | MAPE 7.2%, R² 0.944 forecast | Forecast fig (`10.png`): _____ | ☐ | Keep / revise / drop |
| 10 | Adaptive methods tied on holdout (GLM = DQN = EG-DQN) | If true in your table | ☐ | Add to Limitations + soften Results |

**Status key:** ✅ supported · ⚠️ partially supported · ❌ not supported · ❓ needs your table to confirm

---

## Known issues already flagged (verify against your exports)

These came from comparing draft `main.tex` to notebook outputs — **your saved tables override this list**:

1. Abstract contains **two competing versions** (duplicate paragraph + `//` comment).
2. Related Work has **broken author/citation text** (Charafeddine / Li / Zheng mix-up).
3. Possible **number mismatch** between paper tables and one notebook run — resolve via ledger, not assumptions.
4. Some `\bibitem` entries have formatting errors (e.g. `xiaocheng2023` author field).
5. Figure captions describe EG-DQN “diverging” from baselines — confirm against `17.png` before keeping.

---

## What goes in main text vs appendix

| Main text | Appendix / supplementary (optional) |
|-----------|-------------------------------------|
| Final revenue table (one holdout window) | Sensitivity: different seeds, holdout lengths |
| Significance tests (honest subset) | Extra runs that showed larger gains |
| One convergence + one ablation figure | Hyperparameter grid (λ, episodes) |
| Core limitations | Full training logs |

**Do not hide contradictory runs.** De-emphasize them; don’t delete them if you used them to develop the method.

---

## Division of labour

| Task | You | Assistant |
|------|-----|-----------|
| Provide saved tables / confirm headline claim | ✅ | |
| Add reference PDFs to `references/` | ✅ | |
| Fill Claims Ledger evidence column | ✅ | audit |
| Rewrite Abstract, Results, Conclusion to match ledger | | ✅ |
| Fix citations & Related Work | | ✅ |
| De-AI prose pass | | ✅ |
| Re-run notebook (only if ledger fails) | ✅ optional | verify after |

---

## Suggested workflow for next session

1. **You:** Paste or attach your revenue + significance + ablation tables (screenshot, CSV, or copied notebook output).
2. **Me:** Complete the Claims Ledger and list exact `main.tex` edits needed.
3. **Me:** Rewrite Abstract + Conclusion + Results framing (no overclaim).
4. **You:** Add PDFs to `references/`.
5. **Me:** Related Work + bibliography cleanup.
6. **Both:** Pre-submission checklist.

---

## Re-run decision tree (future reference)

```
Do paper numbers match your saved tables?
├── YES → No re-run. Proceed Priority 2–4.
└── NO → Do saved tables come from a known notebook run?
    ├── YES → Update main.tex to match tables. No re-run.
    └── NO → Re-run once with SEED=42, export tables, regenerate figures.
```

---

## Target contribution statement (for Intro — after ledger confirmed)

> 1. We propose EG-DQN, a hybrid pricing agent that integrates GLM-derived demand signals into DQN via three explicit and ablated mechanisms.
> 2. We implement a reproducible simulation benchmark with known elasticities and a five-strategy evaluation protocol with non-parametric inference.
> 3. We report [holdout revenue comparisons / training convergence / ablation findings — fill from ledger] and discuss when econometric priors help RL pricing in simulation.

---

*Last updated: planning phase — no manuscript edits until Claims Ledger is filled.*
