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

**Review philosophy (locked):** This is **our simulation study** — honest, scoped, limitation-forward. We are **not** claiming a new SOTA pricing method or a general RL breakthrough. If a result looks too strong, is artifact-driven, or cannot be defended under scrutiny, **do not headline it** (and do not put it in the abstract). Prefer “we observe / in this run / under these conditions” over “we prove / outperform / novel.”

**Last updated:** 2026-07-06 — human academic tone guide added; section-by-section workflow active.

---

## Review-safe claim policy (read before every edit)

### What this paper IS

| Yes | No |
|-----|-----|
| A **simulation study** of a hybrid EG-DQN **design** | A deployed pricing product |
| An **honest comparison** of five strategies on one holdout | Proof that hybrid RL beats econometrics in general |
| **Method + benchmark + evaluation protocol** contribution | A new RL algorithm or theory result |
| **Limitation-first** reporting | Marketing language (“breakthrough,” “dominates,” “SOTA”) |

### Four-tier claim rule

| Tier | Where it may appear | Rule |
|------|---------------------|------|
| **A — Headline** | Abstract, Conclusion (1 sentence max each) | Only Tier-A claims. Must be boringly defensible. |
| **B — Body** | Results, Tables, Figure captions | Supported numbers; always scoped (“Item 1,” “120-day holdout,” “simulation”). |
| **C — Limitations / Discussion only** | §Limitations, §Discussion | Artifacts, ties, corner solutions, misfit, failed tests. |
| **D — Do not claim** | Nowhere in manuscript | Unsupported, wrong run, or reviewer-bait. |

**“Hide” means Tier D or C** — not fabricating data. We **report** awkward findings in Limitations; we **do not** put them in the abstract or sell them as success.

### Tier assignment for OUR data

#### Tier A — safe to headline (abstract / conclusion)

- We propose **EG-DQN** with three named integration mechanisms and evaluate them in simulation.
- Five-strategy comparison on a **120-day holdout (Item 1)** in a synthetic three-product environment.
- **GLM-optimal and EG-DQN achieve identical holdout revenue** in this run (tie — not a win for hybrid).
- **EG-DQN does not significantly differ from GLM-optimal** on daily holdout revenue (*p* = 0.50).
- Adaptive / econometric-guided policies **outperform naive fixed-price and rule-based** baselines in this simulation (with scope).
- Study is **simulation-only**, per-item RL, discrete price grid.

#### Tier B — report in Results tables/prose (not abstract)

- Exact holdout revenues (\$175,651 – \$226,764) and prices (\$6 – \$16).
- Pure DQN **lower** than GLM/EG-DQN on holdout (\$183,549) — descriptive only until significance re-run.
- Ablation: reward shaping associated with higher test revenue; **EG-DQN = DQN+Reward** on holdout.
- Training: EG-DQN **slightly higher** last-20-episode rewards than Pure DQN (+26.9% vs +25.5% learning gain).
- GLM elasticity recovery within **~4%** of simulated truth (per product).
- Holdout forecast MAPE **~5.2%** for Item 1.

#### Tier C — Limitations / Discussion only (honest, not hidden)

- GLM-optimal price **collapses to grid minimum (\$6)** for all 120 holdout days — corner solution, not rich dynamic pricing.
- DGP is **partially aligned** with GLM log-link structure → may **favour** econometric-guided methods.
- GLM recovers **elasticity** reasonably but **level** misfit (`8.png` curves above true demand).
- Test forecast **mean residual ≈ −4.57** (systematic over-forecast on holdout).
- **Rule-based** heuristic **underperforms** fixed-price (−20.9%) — heuristic design flaw, not a general claim about rule-based pricing.
- EG-DQN **adds no holdout revenue** over GLM in this run — hybrid value is **not** established on OOS revenue.
- Full hybrid **does not beat** reward-only ablation on test revenue.
- `Loss=nan` during training — learning diagnostics unreliable.
- Single item, single seed, single holdout window — no generalization claim.
- Large Cohen’s *d* vs FP/RB (*d* > 2) — report in table but **note** policies are far apart (constant prices); effect size is not “hybrid magic.”

#### Tier D — do NOT claim (delete if still in `main.tex`)

| Do not claim | Why |
|--------------|-----|
| EG-DQN beats GLM / “highest revenue” | Tied; *p* = 0.50 |
| Significant vs **all** baselines | False for GLM |
| +18.3%, \$151,940, ep 35/68, 61% variance | Wrong run |
| State augmentation = main holdout driver | Not in ablation design |
| Stable TD-loss / “faster loss decay” | `Loss=nan`; Fig. 15 middle panel unreliable |
| “Structural identification” | Not formal ID |
| Deployment / “suitable for deployment” | Simulation only |
| “Novel / SOTA / breakthrough” | Wrong paper type |
| +29.1% in **abstract** | Technically true but **reviewer-bait** without corner-solution context — keep **B** or **C** only |
| Cohen’s *d* = 4.6 in abstract | Looks unbelievable headline — table only, with caveat |
| Training rewards in **millions** in abstract | Scale confuses readers; use % improvement only if mentioned |

### Percentage rule

Large lifts (e.g. +29.1% vs fixed) are **allowed in Results tables** with Item 1 + simulation scope. In **Abstract**, prefer:

- “substantially higher than fixed-price and rule-based baselines” **or**
- “+29.1% vs fixed-price **in this simulation** (GLM and EG-DQN at grid-minimum price)”

Never imply the lift transfers to real retail without validation.

### Significance rule

| Comparison | Claim in main text? |
|------------|---------------------|
| EG-DQN vs Fixed-Price | ✅ Yes — *p* < 0.001 |
| EG-DQN vs Rule-Based | ✅ Yes — *p* < 0.001 |
| EG-DQN vs GLM-Only | ✅ Report as **not significant** — this is a **strength** (honesty) |
| EG-DQN vs Pure DQN | ⚠️ **Descriptive only** until re-run — say “higher cumulative revenue” without *p* |

**Significance table in paper: 3 rows** (FP, RB, GLM with NS). Add Pure DQN row only after clean re-run.

### Ablation rule

- Report 3-config ablation honestly.
- **Do not** claim “full EG-DQN > DQN+Reward.”
- **Do** say: “holdout revenue increases when econometric reward shaping is enabled; full EG-DQN matches reward-only in this run.”
- State-only / mask-only: **future work** (not fabricated).

### Figure caption rule

| Figure | Caption must NOT say |
|--------|----------------------|
| `17.png` | “EG-DQN diverges / dominates” — lines **overlap** GLM |
| `15.png` | “stable loss decay” — loss unreliable |
| `8.png` | “structural identification” — say “elasticity recovery” |
| `21.png` | “deployment-ready” — say “rolling estimate stability” |
| `10.png` | Perfect forecast — mention holdout residual bias in body |

### Title direction (conservative)

Prefer simulation-study framing:

> **Integrating an Econometric Demand Model with Deep Q-Network Pricing: A Simulation Study**

Avoid: “Novel,” “Superior,” “Optimal Hybrid Framework.”

---

## Human academic tone (write like a researcher, not a template)

**Goal:** Prose that satisfies **scholarly conventions** in operations research / retail analytics — precise, restrained, evidence-led — while remaining readable. Apply on every section edit (Steps 0–12).

### Academic writing principles (non-negotiable)

These are standard expectations in peer-reviewed OR/MS and IEEE venues. Follow them in every unit.

| Principle | What it means | Example for this paper |
|-----------|---------------|------------------------|
| **Epistemic humility** | Match claim strength to evidence | “suggest,” “in this simulation,” “on the holdout window” — not “prove” or “demonstrate superiority” |
| **Explicit scope** | Bound every finding | “Item~1,” “120-day holdout,” “discrete price grid,” “per-item training” |
| **Logical progression** | One rhetorical move per paragraph | context → method → evaluation → finding → limitation |
| **Terminological consistency** | Same name throughout | EG-DQN (Econometric-Guided Deep Q-Network); GLM-optimal price $p^*_{\text{GLM}}$ |
| **Definition before use** | Acronym on first mention | “Generalized Linear Model (GLM)” then “GLM” |
| **Evidence-linked assertions** | Claims point to table/figure/test | “Table~\ref{tab:revenue} reports…”; “$p=0.50$” |
| **Hedging where appropriate** | Strong tests → cautious interpretation | Significant vs FP ≠ “EG-DQN is the best pricing system” |
| **Nominal academic register** | Formal but plain | “evaluate,” “estimate,” “compare” — not conversational, not ornate |
| **Economy** | No filler or repetition | State a result once; do not re-list all percentages in abstract and conclusion |
| **Objectivity** | Limitations stated plainly | GLM–EG-DQN tie is a **finding**, not buried |
| **IEEE conventions** | Present tense for method; past for experiments | “We describe…” / “We compared…” / “EG-DQN attained…” |

**Modal verbs (academic hedging scale):**

| Strength | Verbs | Use when |
|----------|-------|----------|
| Design / protocol | describe, formulate, implement, evaluate | Methods, contributions |
| Empirical pattern | show, indicate, suggest, appear to | Results aligned to figures |
| Strong (rare here) | demonstrate, establish | Only if table/test unambiguous **and** scoped |

**Do not use** “clearly,” “obviously,” “undoubtedly,” “it goes without saying.”

**Paragraph architecture (Results / Discussion):**

1. **Topic sentence** — the paragraph’s single point.  
2. **Evidence** — table, figure, or test statistic.  
3. **Interpretation** — one or two sentences, scoped.  
4. **Limitation or caveat** (if needed) — especially for corner solutions and ties.

**Abstract architecture (IEEE):**

1. Problem context (1–2 sentences).  
2. Method + what was evaluated (2–3 sentences).  
3. Principal findings — **include GLM tie** (2–3 sentences).  
4. Scope / limitation closing (1 sentence).  
No citations in abstract; no equation numbers; minimal abbreviations beyond GLM, DQN, EG-DQN.

---

| Human academic | AI-typical (avoid) |
|----------------|-------------------|
| Short, direct sentences mixed with longer technical ones | Every sentence same length and shape |
| Names **your** design choices (EG-DQN, Item 1, 120-day holdout) | Generic “the proposed approach” / “this framework” |
| States **limitations** without drama | Hedges everything with “comprehensive,” “promising,” “robust” |
| Uses verbs: *describe, report, compare, estimate, evaluate* | *leverage, delve, underscore, facilitate, utilize* |
| One clear claim per sentence | Stacked clauses + three adjectives |
| “In this simulation…” / “On the holdout window…” | “In today’s rapidly evolving landscape…” |
| Figures/tables do the heavy lifting | Prose repeats all numbers twice |

### Banned words & phrases (search `main.tex` before each unit ✅)

**Delete or rewrite if found:**

`comprehensive` · `promising framework` · `richly parameterised` · `tightly couples` · `directly instantiates` · `traditional dichotomy` · `paradigms` (unless quoting literature) · `bridge these traditions` · `landscape` · `delve` · `leverage` · `utilize` · `Furthermore,` (paragraph opener) · `Moreover,` (every paragraph) · `It is important to note` · `In conclusion,` (in body) · `novel` / `innovative` / `groundbreaking` · `seamlessly` · `holistic` · `pivotal` · `underscores` · `facilitates rapid` · `notably` · `crucially`

**Current `main.tex` offenders (fix when that section is edited):**

| Phrase | Location | Replace with |
|--------|----------|--------------|
| “tightly couples” | Abstract | “combines” or “integrates” |
| “promising framework” | Abstract | delete sentence |
| “comprehensive study” | Abstract | “simulation study” |
| “richly parameterised” | Conclusion | “synthetic three-product” |
| “bridge these traditions” | Intro | “combines econometric estimation with RL” |
| “traditional dichotomy” | Discussion | “distinction between model-based and model-free pricing” |
| “directly instantiates” | Results/ablation | “is consistent with” |

*Exception:* “HC3-robust standard errors” is **statistical terminology** — keep.

### Preferred voice for **this** paper

1. **First person plural, restrained:** “We describe…”, “We report…”, “We do not claim…” — not “This study investigates and simulates how hybrid framework…”
2. **Scope tag on every result:** “on Item 1,” “over 120 holdout days,” “in this run,” “under the discrete price grid.”
3. **Honest negatives upfront:** GLM–EG-DQN tie reads **human** because AI rarely volunteers it.
4. **Active voice for your actions:** “We train a DQN per product” not “Training is conducted.”
5. **Passive only for standard bits:** “Demand is modelled as…” / “Prices are selected from a grid.”
6. **No throat-clearing:** Cut “This paper proposes and evaluates…” → start with the object: “EG-DQN integrates…”

### Sentence patterns that work (examples for your study)

**Abstract / Intro**

- ❌ “This study investigates how hybrid frameworks bring dynamic pricing as an effective strategy in supply chains.”
- ✅ “We evaluate EG-DQN, a hybrid that feeds GLM demand signals into a DQN pricing agent, in a synthetic retail simulation.”

**Results**

- ❌ “EG-DQN diverges from baselines after day 20, demonstrating superior adaptive capability.”
- ✅ “On the holdout window, GLM-optimal and EG-DQN policies coincide at \$6 and produce identical cumulative revenue (\$226,764).”

**Discussion**

- ❌ “This directly instantiates Safonov’s argument that econometric structure is most valuable as an interpretable prior.”
- ✅ “This pattern is consistent with Safonov’s view that econometric structure is most useful as an interpretable prior rather than a stand-alone optimiser.”

**Limitations (human = specific)**

- ❌ “Results may not generalise to real-world settings.”
- ✅ “GLM-optimal prices hit the grid lower bound (\$6) on every holdout day, so the experiment does not explore interior price dynamics.”

### Structure rules (IEEE / OR style)

| Rule | Target |
|------|--------|
| Paragraph length | 4–7 sentences; one idea each |
| Opening sentence | States the paragraph’s point — no “Furthermore,” |
| Transitions | “On the holdout set,” “During training,” “By contrast,” — not “Additionally,” chains |
| Figure captions | **Descriptive** only; interpretation stays in body |
| Contribution bullets | Start with verb: “Describe,” “Implement,” “Report” — not “A novel framework…” |
| Equations | Introduced in plain English before symbols |

### Per-section tone checklist (run after each Step ✅)

- [ ] No banned phrases in this unit
- [ ] At least one **scope** phrase (“simulation,” “Item 1,” “holdout”)
- [ ] No claim stronger than Tier allows (see claim policy)
- [ ] One short sentence (≤15 words) somewhere in the unit
- [ ] No paragraph starts with “Furthermore” / “Moreover” / “Additionally”
- [ ] Citations support claims, not decoration
- [ ] Read aloud once — if it sounds like a brochure, rewrite

### Process: tone pass built into section-by-section workflow

For each Step (0–12):

1. **Draft** content from blueprint numbers + conservative framing.
2. **Strip** banned list (search within edited lines).
3. **Add** scope tags to every numeric claim.
4. **Shorten** by ~10% — cut filler adjectives.
5. **You read aloud** — approve or flag awkward lines (human check beats any detector).

**Optional final pass (Step 14):** Read Abstract + Conclusion side-by-side; they should sound like the same author, not two different templates.

### What we do *not* do

- Stuffing synonyms or random typos to fool detectors
- Over-casual or blog-style writing
- Fake “personal anecdotes”
- Inflated claims to sound impressive (detectors flag generic hype too)

**Honest limitation-first science reads human** — use that as the main strategy.

---

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

## Locked headline framing — Branch C (conservative)

**Primary message for reviewers:** We ran a careful simulation study. The hybrid **does not beat** GLM on holdout. That is the honest result — and it is **publishable** as a design + evaluation paper.

**Holdout (Item 1, 120 days) — Tier B facts:**
- GLM-Only and EG-DQN **tie** at **\$226,764** (both \$6 constant price — **Tier C** corner artifact).
- EG-DQN **≈ GLM** statistically (*p* = 0.50) — **lead with this in abstract**.
- Pure DQN lower (\$183,549, \$9.1) — descriptive comparison only.
- Fixed-price \$175,651; rule-based \$138,864 (heuristic fails — Tier C).

**Training / ablation — secondary evidence (Tier B, not abstract headline):**
- Reward shaping linked to holdout lift; EG-DQN = DQN+Reward on test.
- Modest training-reward edge for EG-DQN vs Pure DQN.
- **Do not claim:** ep 35/68, 61% variance, state augmentation holdout driver, stable loss.

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

*Source: `data.md` cell-18. **Paper table: 3 rows only** (add Pure DQN after re-run).*

| Baseline | *p*-value | Cohen's *d* | Report? | Notes |
|----------|-----------|-------------|---------|-------|
| Fixed-Price | <0.001 | 2.495 | ✅ Tier B | Add Discussion caveat: constant-price policies → large *d* |
| Rule-Based | <0.001 | 4.628 | ✅ Tier B | Same caveat |
| GLM-Only | 0.5004 | 0.000 | ✅ Tier B | **Report NS** — key honesty point |
| Pure DQN | — | — | ⏸ Hold | Descriptive revenue gap only until re-run |

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
| 11 | EG-DQN > Pure DQN on holdout | \$226,764 vs \$183,549 | ✅ Supported | **Tier B** descriptive only; no *p* until re-run |
| 12 | Rule-based beats FP | −20.9% | ✅ Supported | **Tier B** report; **Tier C** note heuristic flaw |
| 13 | Training: EG-DQN > Pure DQN | Last20 rewards higher | ✅ Supported | **Tier B** only; % gain not raw \$M in abstract |
| 14 | Ablation: reward +30.5% | `20.png` | ✅ Supported | **Tier B**; **Tier C** that EG-DQN = DQN+Reward |
| 15 | Stable TD-loss / convergence caption | `Loss=nan` | ❌ Unsupported | **Tier D** — omit loss claims |
| 16 | +29.1% headline lift | True but corner-driven | ⚠️ Reviewer-bait | **Tier B** table; **not** abstract headline |
| 17 | Cohen's *d* > 2 | True vs FP/RB | ⚠️ Reviewer-bait | **Tier B** table + caveat in Discussion |

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

### Lead with ✅ (Tier A only in abstract)

1. **Study type** — simulation evaluation of EG-DQN hybrid **design** (not a product claim).
2. **Protocol** — five strategies, 120-day holdout, non-parametric tests, ablation.
3. **Honest holdout result** — GLM and EG-DQN **tie**; both beat naive baselines **in this run**.
4. **Design value** — documents **when** econometric signals help training vs when they do not add OOS revenue over GLM.

### Do not headline ❌ (Tier D or C only)

- EG-DQN beats GLM / “hybrid superiority”
- “Statistically significant vs all baselines”
- Large % lifts without “simulation” + corner-price context
- Convergence ep 35/68, 61% variance, state augmentation as holdout driver
- Cohen’s *d* > 2 as a selling point
- Deployment readiness
- “Novel algorithm / SOTA”

### Conservative abstract template (use this — not the old one)

> **Integrating an Econometric Demand Model with Deep Q-Network Pricing: A Simulation Study.** Retail dynamic pricing must balance econometric structure and sequential adaptivity. We describe **EG-DQN**, a hybrid agent that injects GLM demand signals into a Deep Q-Network via state augmentation, econometric reward shaping, and early action masking, and we evaluate it in a **controlled simulation** of three substitute products (1,460 days; 120-day holdout on Item 1). Five strategies are compared: fixed-price, rule-based, GLM-optimal, pure DQN, and EG-DQN. On the holdout window, **GLM-optimal and EG-DQN attain the same cumulative revenue** and select the same constant price on the discrete grid; EG-DQN **does not** significantly differ from GLM-optimal on daily revenue (*p* = 0.50). Relative to fixed-price and rule-based baselines, EG-DQN achieves **higher** holdout revenue in this simulation (Mann–Whitney *p* < 0.001 for both). Pure DQN earns less than GLM/EG-DQN under the learned policy. Ablations indicate that **econometric reward shaping** is associated with the holdout gain; the full hybrid does not exceed reward-only on test revenue. Training-episode rewards are modestly higher for EG-DQN than for pure DQN. **Limitations:** synthetic data, per-item estimation, discrete actions, GLM–DGP partial alignment, and a grid-minimum corner solution for GLM-guided prices. We do not claim deployment readiness or general superiority over econometric pricing.

*No large % or Cohen’s *d* in abstract — those go in Results table only.*

### Conservative contribution statement (Intro — 3 bullets)

1. We **describe** EG-DQN and three mechanisms for injecting GLM-derived signals into DQN-based retail pricing.
2. We **implement** a reproducible simulation benchmark (known elasticities, five strategies, holdout evaluation, Mann–Whitney tests, ablation).
3. We **report** that in this simulation GLM-optimal and EG-DQN **tie** on holdout revenue while outperforming naive baselines; reward shaping aligns holdout outcomes with GLM-guided prices, and EG-DQN shows modest training-phase gains over pure DQN — **without** claiming hybrid dominance over econometrics.

### One-sentence conclusion (Tier A)

> This simulation study suggests that econometric-guided reward shaping can align RL pricing with GLM-optimal outcomes on the holdout window, but **does not** demonstrate holdout gains beyond GLM-only pricing; further work with richer action spaces, real data, and fuller ablations is needed before operational claims.

---

## Reviewer risk matrix

| Risk | Severity | Mitigation |
|------|----------|------------|
| Simulation-only | High | Scope claims; cite Nambiar, Xia, Chen in Methods |
| Overclaiming / unbelievable stats | **Critical** | Tier A–D policy; no % or *d* in abstract |
| Corner solution at \$6 | High | Tier C — explain before any % lift |
| `main.tex` ≠ figures | **Resolved** | Rewrite per blueprint |
| arXiv-heavy bib | Medium | Priority 3 rebalance |
| `Loss=nan` | Medium | Acknowledge; no loss claims |

---

## Priority order — current status

**Active workflow:** Section-by-section completion (see **Manuscript approach** below). Do not skip ahead without marking prior unit ✅.

### ✅ Priority 0 — Evidence audit — DONE

### 🔄 Priority 1 — Section-by-section `main.tex` rewrite (IN PROGRESS)

Work through **Steps 0 → 12** in order. Results split into **8a–8f**. References (Step 11) after Conclusion; sync pass (Step 14) last.

*Legacy bulk tasks (replace all tables at once) superseded by per-unit tracker above.*

### 🟡 Priority 3 — References

- [ ] Fix `xiaocheng2023` bibitem; add `chen2023` to Intro; rebalance arXiv.

### 🟢 Priority 4 — Human tone & captions (every section, not only at end)

- [ ] Apply **Human academic tone** checklist after each Step 0–12 unit.
- [ ] Search banned-word list in edited section before marking ✅.
- [ ] Shorten figure captions; move interpretation to body.
- [ ] Fix Fig. 17 caption (GLM/EG-DQN overlap, not “diverges”).
- [ ] Step 14: Abstract + Conclusion sound like same author.

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
| Section-by-section `main.tex` rewrite | Review each unit | ✅ Lead edits |
| References + prose polish | | ✅ Per section |

---

## Manuscript approach — section by section (locked)

**Your approach:** Complete `main.tex` **one section (or subsection) at a time** — abstract → conclusion — with citations, tables, and figures done **inside that unit** before moving on. **Agreed.** This is the right strategy for a defensible rewrite: each unit is self-contained, reviewable, and aligned to `data.md` / `figures/` + Tier A–D policy.

### Rules for every unit

1. **Read** blueprint Tier rules + relevant `data.md` / figure before editing.
2. **Edit** only that section in `main.tex` (minimal diff).
3. **Include** in the same pass: prose, `\cite{}`, tables, `\includegraphics`, labels.
4. **Check** done-criteria below before marking ✅.
5. **You review** the unit; we fix feedback; then advance.
6. **No forward references** to numbers not yet written in an earlier Results subsection.
7. After **§Results** is complete, do a **sync pass** on Abstract + Conclusion (one short pass, not a full rewrite).

### Recommended order (front → back)

| Step | Unit | Why this order |
|------|------|----------------|
| 0 | Title + Keywords | Sets simulation-study tone |
| 1 | **Abstract** | Locks Tier-A story early (re-sync after Results) |
| 2 | **§Introduction** | Contributions must match abstract; no results leakage |
| 3 | **§Related Work** | Citations stable before Methods cite same keys |
| 4 | **§Problem Formulation** | MDP/DGP — no results numbers |
| 5 | **§Econometric Demand Model** | GLM spec; fix “structural identification” here |
| 6 | **§RL Framework** | EG-DQN mechanisms + algorithm |
| 7 | **§Simulation Environment** | Defense of simulation + hyperparams + baselines |
| 8a | **§Results — Demand estimation** | Elasticity + forecast tables/figs |
| 8b | **§Results — Revenue** | **Highest scrutiny** — `17.png` + revenue table |
| 8c | **§Results — Training** | `15.png`; no loss claims |
| 8d | **§Results — Ablation** | `20.png`; 3-config table only |
| 8e | **§Results — Significance** | 3 rows; report GLM NS |
| 8f | **§Results — Rolling elasticity** | `21.png`; no deployment language |
| 9 | **§Discussion — Methodological** | Interpretation only; no new numbers |
| 10 | **§Discussion — Limitations** | Corner solution, tie, DGP alignment |
| 11 | **§Discussion — Path to deployment** | Future work tone only |
| 12 | **§Conclusion** | Tier-A only; mirror abstract |
| 13 | **References** | Fix bibitems; sync with all `\cite{}` |
| 14 | **Sync pass** | Abstract ↔ Results ↔ Conclusion consistency |

**Note:** Results is split into **6 sub-units (8a–8f)** because it is the largest section and carries most claim risk.

---

## Section completion tracker

Mark: `⬜` not started · `🔄` in progress · `✅` done · `👁` your review

### Step 0 — Front matter

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 0a | **Title** | `\title{...}` | — | — | — | ✅ |
| 0b | **Keywords** | `\begin{IEEEkeywords}` | — | — | — | ✅ |

**0a Done when:** Title includes “Simulation Study”; no “novel/superior/optimal.”  
**0b Done when:** Keywords include *simulation study*; no deployment hype.

---

### Step 1 — Abstract

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 1 | **Abstract** | `\begin{abstract}` | — | — | — | ✅ |

**Content (Tier A only):**
- Study type: simulation evaluation of EG-DQN design.
- GLM and EG-DQN **tie** on holdout; *p* = 0.50 vs GLM.
- Beats naive baselines in this run (no % or Cohen’s *d*).
- Reward shaping in ablation; no hybrid > GLM claim.
- Limitations sentence in abstract (simulation, discrete grid, per-item).

**Done when:** Single paragraph; no `//` artifact; no Tier D terms; conservative template from §Defensible framing applied.

**After §Results:** Re-read once (Step 14 sync) — numbers in abstract must ⊆ Results.

---

### Step 2 — Introduction

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 2 | **§Introduction** | `\section{Introduction}` | — | — | kopalle, apte, groeneveld, nambiar, chen | ⬜ |

**Sub-parts (one edit pass):**
- 2.1 Problem motivation (econometric vs RL gap).
- 2.2 EG-DQN one-paragraph preview (three mechanisms).
- 2.3 **Contributions** — exactly **3 bullets** (describe / implement / report).

**Done when:** No result numbers in Intro; contributions match abstract; “propose/describe” not “prove/outperform.”

---

### Step 3 — Related Work

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 3a | **§II.A** Dynamic Pricing and RL | `\subsection{Dynamic Pricing...}` | — | — | groeneveld, xiaocheng2023, liu, razumovskiy | ⬜ |
| 3b | **§II.B** Demand Forecasting | `\subsection{Demand Forecasting}` | — | — | husna, ramos, bu, zhang, xia2023 | ⬜ |
| 3c | **§II.C** Econometric Models | `\subsection{Econometric...}` | — | — | zhang, safonov | ⬜ |
| 3d | **§II.D** RL for Pricing | `\subsection{RL for Pricing}` | — | — | apte, zheng, alamdar, nambiar | ⬜ |

**Done when:** Journal anchors lead each subsection; no decorative cites; `xiaocheng2023` author text correct; gap statement points to **hybrid design + simulation protocol** (not SOTA).

---

### Step 4 — Problem Formulation

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 4a | Market structure | `\subsection{Market Structure}` | — | — | rios2023 | ⬜ |
| 4b | DGP | `\subsection{Demand Data-Generating Process}` | — | elasticity matrix (if here) | kopalle, zhang | ⬜ |
| 4c | MDP | `\subsection{Pricing as a Markov...}` | — | — | — | ⬜ |

**Done when:** Eqs. (objective, DGP, reward) unchanged unless notebook differs; note “approximately aligned” with GLM (not “correctly specified”).

---

### Step 5 — Econometric Demand Model

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 5a | GLM specification | `\subsection{Specification}` | — | — | — | ⬜ |
| 5b | Derived signals | `\subsection{Derived Econometric Signals}` | — | — | — | ⬜ |

**Done when:** “Structural identification” → “elasticity recovery”; enumerate three signals (ε̂, p\*\_GLM, D̂).

---

### Step 6 — Reinforcement Learning Framework

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 6a | Network | `\subsection{Network Architecture}` | — | — | — | ⬜ |
| 6b | Training | `\subsection{Training Procedure}` | — | — | — | ⬜ |
| 6c | EG-DQN mechanisms | `\subsection{EG-DQN Integration...}` | — | — | cheung2023 (optional) | ⬜ |
| 6d | Algorithm | `Algorithm~\ref{alg:egdqn}` | — | — | — | ⬜ |

**Done when:** Three mechanisms named; λ, K, episodes match `data.md`; no performance claims.

---

### Step 7 — Simulation Environment

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 7a | Design & calibration | `\subsection{Design and Calibration}` | — | cross-price ε table | xia2023, xia2024 | ✅ |
| 7b | Hyperparameters | `\subsection{Hyperparameters}` | — | Table hyperparams | — | ✅ |
| 7c | Baselines | `\subsection{Baseline Strategies}` | — | — | — | ✅ |

**Done when:** One sentence defending simulation (Nambiar, Xia, Chen); SEED=42, holdout=120, Item 1 stated; five baselines defined.

---

### Step 8 — Experiments and Results (split into 6 units)

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 8a | Demand estimation accuracy | `\subsection{Demand Estimation...}` | `eda.png`, `8.png`, `10.png` | elasticity, forecast | — | ✅ |
| 8b | **Revenue comparison** | `\subsection{Revenue Comparison}` | `17.png` | revenue (5 strategies) | kumar2026 optional | ✅ |
| 8c | Training convergence | `\subsection{Convergence Analysis}` | `15.png` | — | cheung2023 | ✅ |
| 8d | Ablation | `\subsection{Ablation Study}` | `20.png` | 3-config ablation | safonov2024 | ✅ |
| 8e | Significance | `\subsection{Statistical Significance}` | — | 3 rows (FP, RB, GLM) | — | ⬜ |
| 8f | Rolling elasticity | `\subsection{Rolling Elasticity...}` | `21.png` | — | — | ⬜ |

**8a Done when:** Elasticity table from `data.md` (max bias 3.7%); forecast MAPE 5.20%; fig captions Tier-safe; `figures/` paths.

**8b Done when:** Revenue table matches `17.png`; prose states GLM=EG-DQN tie; Pure DQN lower; **no** “diverges/dominate”; % only with simulation scope.

**8c Done when:** Training reward comparison only; **delete** ep 35/68 and 61% variance; **no** TD-loss claims; caption describes episode revenue panel only.

**8d Done when:** 3-row ablation only; reward shaping interpretation; EG-DQN = DQN+Reward stated; state/mask → future work.

**8e Done when:** GLM row shows NS (*p*=0.50); Cohen’s *d* with Discussion caveat; Pure DQN row omitted until re-run.

**8f Done when:** Rolling elasticity descriptive; caption: “stable over evaluation window” not “deployment.”

---

### Step 9 — Discussion

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 9a | Methodological implications | `\subsection{Methodological Implications}` | — | — | cheung2023, apte2024 | ⬜ |
| 9b | **Limitations** | `\subsection{Limitations}` | — | — | nambiar, xia2024 | ⬜ |
| 9c | Path to deployment | `\subsection{Path to Deployment}` | — | — | chen2023, xia2024 | ⬜ |

**9b must include (Tier C):** synthetic only; per-item RL; discrete grid; \$6 corner; GLM=EG-DQN tie; DGP–GLM alignment; level misfit; rule-based failure; no hybrid OOS gain over GLM.

**Done when:** No new numeric claims; “theoretical prediction” removed; deployment framed as future work.

---

### Step 10 — Conclusion

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 10 | **§Conclusion** | `\section{Conclusion}` | — | — | future: hadi, nomura | ⬜ |

**Done when:** One honest Tier-A summary; no 18.3%, no ep 35/68, no “significant vs all”; future work 2–3 bullets; matches abstract.

---

### Step 11 — References

| Step | Unit | `main.tex` | Figures | Tables | Citations | Status |
|------|------|------------|---------|--------|-----------|--------|
| 11 | **Bibliography** | `\begin{thebibliography}` | — | — | all keys | ⬜ |

**Done when:** Every `\cite{}` has `\bibitem`; `xiaocheng2023` fixed; no orphan bibitems; arXiv ≤35%.

---

### Step 12 — Final sync pass

| Step | Unit | Action | Status |
|------|------|--------|--------|
| 14 | **Cross-section sync** | Abstract ⊆ Results; Conclusion ⊆ Results; Intro contributions ⊆ Abstract; figure paths compile | ⬜ |
| 15 | **Pre-submission** | Priority 5 checklist | ⬜ |

---

## How we work in chat (one unit per session)

**Per unit, you say:** e.g. *“Let’s do Step 1 — Abstract.”*

**I will:**
1. Quote current `main.tex` text for that unit.
2. Propose replacement (or edit file directly if you prefer).
3. List citations/tables/figures added.
4. Run **tone checklist** (banned words, scope tags, read-aloud test).
5. Show done-criteria checklist.
6. Wait for your 👁 before marking ✅ and moving on.

**Suggested first session:** Step 0a–1 (Title + Abstract) — smallest unit, sets tone for everything else.

**Suggested heavy session:** Step 8b only (Revenue) — do not combine with 8e in one pass.

---

*Sources: `data.md`, `figures/`, `critic.md`, `context.md`. **Manuscript rule:** when in doubt, downgrade one tier (headline → body → limitations → omit). Honesty passes review; hype fails it.*
