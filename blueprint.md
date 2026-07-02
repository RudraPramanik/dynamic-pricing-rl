# Paper Polish Blueprint — EG-DQN Simulation Study

**Goal:** Pass review by presenting an honest, defensible **experimental simulation study** — not an overclaimed “new best pricing method.”

**Positioning target (from `critic.md`):** The paper should read as **economically grounded pricing research with RL integration**, not a generic “RL simulation paper.”

**Files in scope:**
- `main.tex` — manuscript
- `Dynamic_pricing_hybrid_EG_DQN.ipynb` — experiment source
- `critic.md` — external review notes (integrated below)
- `context.md` — agent onboarding / method summary
- Your exported **tables & figures** (authoritative outputs)
- Reference PDFs in `references/`

**Working principle:** Every sentence that reports a number or claims “significance” must trace to one saved table or figure. No number in the abstract unless it appears in Results.

**Last updated:** 2026-07-02 — integrated `critic.md` verdict + notebook/paper audit.

---

## Reviewer risk matrix (from critic — treat as design constraints)

| Risk | Severity | Mitigation in blueprint |
|------|----------|-------------------------|
| “Simulation-only” | High | Scope every claim; defend simulation in Methods; cite Nambiar, Xia, Chen |
| “Incremental hybrid RL” | High | Lead with **integration design + ablation**, not SOTA claims |
| “Overclaiming” | High | Claims Ledger; soften holdout if adaptive methods tie |
| “Simulation favors EG-DQN” | **Critical** | Explicit limitation: DGP–GLM partial alignment may advantage guided methods |
| “Too many arXiv citations” | Medium | Rebalance bib: journal anchors first, trim decorative arXiv |
| Notebook ≠ paper numbers | **Critical** | Priority 0 audit before abstract or results edits |

**Main danger is reviewer perception, not missing implementation** (`critic.md`).

---

## Realistic venue targets (after polish)

| Venue type | Outlook |
|------------|---------|
| IEEE workshop / regional conference | Strong chance |
| Mid-tier applied conference | Borderline but possible |
| Springer AI / operations application journal, JRPM-type | Possible after polish |
| IEEE Access | Maybe after stronger polish |
| Top-tier RL/ML conference | No — wrong contribution type |

**Do not frame as:** breakthrough RL algorithm, new RL theory, or deployment-ready pricing product.

---

## Priority order (revised)

### 🔴 Priority 0 — Evidence audit (**blocker — do before abstract**)

**Owner:** You (confirm source of truth) + assistant (cross-check `main.tex`)

- [ ] Decide authoritative run: `main.tex` tables/figures **or** saved notebook outputs.
- [ ] Complete Claims Ledger (pre-seeded below with known conflicts).
- [ ] Resolve notebook red flags if notebook is authoritative:
  - `Loss=nan` throughout DQN training
  - GLM-Only = Pure DQN = EG-DQN identical holdout revenue (\$226,764; price \$6.00)
  - EG-DQN vs GLM/DQN: *p* = 0.5004 (not significant)
- [ ] Choose **headline result** honestly (see Framing decision tree).

**Do not** rewrite Abstract, Results, or Conclusion until Priority 0 is resolved.

---

### 🔴 Priority 1 — Language & framing blockers (from `critic.md`)

**Owner:** assistant (after Priority 0 framing is chosen)

#### Already fixed in `main.tex` ✅

| Critic item | Status |
|-------------|--------|
| Remove “GLM ceiling” → use “GLM benchmark / econometric baseline” | ✅ Body uses “GLM benchmark”; verify no “ceiling” remains |
| Remove “dominate both pure approaches” | ✅ Conclusion: “can outperform standalone approaches under the evaluated conditions” |
| “Theoretical Implications” → “Methodological Implications” | ✅ Done (§Discussion) |
| “Correctly specified GLM” limitation heading | ✅ Renamed to “Potential GLM misspecification” with mature rewrite |

#### Still required ❌

| Location | Current (bad) | Replace with |
|----------|---------------|--------------|
| §GLM Derived Signals (~L388) | “validates structural identification against the known DGP” | “validates accurate elasticity recovery against known simulated parameters” |
| Fig. 8 caption (`8.png`) | “confirm structural identification accuracy” | “estimated elasticities closely match simulated ground-truth values” |
| Convergence § (~L759) | “consistent with the theoretical prediction” | “consistent with prior-informed RL literature” (cite Cheung) |
| Fig. 21 caption (`21.png`) | “suitable for deployment” | “stable over the evaluation window” (deployment → Future Work) |
| Limitations § | partial GLM-alignment note only | **Add:** “The simulated demand process was partially aligned with GLM assumptions, which may advantage econometric-guided methods relative to highly irregular real-world retail environments.” |
| Simulation § (Methods) | no simulation defense | **Add one sentence:** simulation is standard when controlled comparison and known structure are needed; cite Nambiar, Xia, Chen |
| Abstract | two competing drafts + `//` artifact + typos | Single paragraph; match ledger |
| Title | “With Integrating” | “Integrating an Econometric Demand Model” (or similar) |

#### Search-and-destroy terms (never use)

- `GLM ceiling`, `econometric ceiling`
- `structural identification` (unless formal ID proof — we do not have one)
- `dominate`, `breakthrough`, `state-of-the-art`, `promising framework`, `comprehensive study`, `tightly couples`
- `theoretical prediction` (we are not making a theory contribution)
- `suitable for deployment` in Results/captions

---

### 🟠 Priority 2 — Results structure & contribution emphasis

**Owner:** assistant + your approval on headline

- [ ] **Abstract** — one paragraph; scoped to simulation + Item 1 holdout; match ledger.
- [ ] **Title** — grammar fix.
- [ ] **Results** — two explicit layers:
  1. **Holdout revenue** (120-day test set)
  2. **Training dynamics** (convergence ep 35 vs 68, variance, ablation)
- [ ] **Emphasize ablation** (`critic.md` §5) — largest defensible differentiator; expand 1–2 sentences in Results intro + Discussion.
- [ ] **Significance language** — only claim rejection where table supports; never “all baselines” unless all rows reject.
- [ ] **Conclusion** — match ledger framing; no holdout superiority among adaptive methods if audit shows ties.
- [ ] **Contributions** — reduce to **3 bullets** (design, benchmark, evaluation/ablation).

---

### 🟡 Priority 3 — References & citations

**Owner:** you (PDFs) + assistant (integration)

**Critic verdict:** References are a weakness, but **do not purge good topical arXiv** (e.g. `apte2024`, `xia2024`). Rebalance instead.

#### Keep and lead with (Tier A anchors — already in bib)

- `kopalle2023` — *J. Retailing*
- `xiaocheng2023` (Li & Zheng) — *Management Science* — fix confusing `\bibitem` key/author field
- `cheung2023` — *Management Science* — nonstationary RL / guided exploration
- `bu2023` — *Management Science* — offline demand learning
- `chen2023` — *J. Oper. Manag.* — field experiments (add to Intro for deployment credibility)
- `groeneveld2025` — *JRPM*
- `liu2024`, `alamdar2024`, `rios2023` — operational retail / inventory

#### Keep but reduce prominence (Tier B arXiv — cite with purpose)

- `apte2024` — direct retail Q-learning baseline (essential)
- `xia2023`, `xia2024` — simulation / RetailSynth framing
- `zheng2024`, `safonov2024` — hybrid neighbours
- `razumovskiy2025`, `hadi2025`, `kumar2026` — cite sparingly in future work / MARL

#### Removed / not in current `main.tex` (critic outdated)

- Charafeddine — already absent; no action

#### Tasks

- [ ] Fix `xiaocheng2023` `\bibitem` metadata (authors = Li & Zheng, not wrong key label).
- [ ] Intro: one sentence on field-evidence RL pricing → cite `chen2023`.
- [ ] Related Work §II.A: lead paragraph with journal anchors before arXiv neighbours.
- [ ] Demote decorative citations (one cite per claim).
- [ ] Target: **≤35% arXiv** in final bib (currently ~10/26 ≈ 38%).

---

### 🟢 Priority 4 — De-AI tone & caption polish

**Owner:** assistant — **only after Priority 0–2**

- [ ] Remove ornate phrasing: “directly instantiates,” “richly parameterised,” stacked superlatives.
- [ ] Use: “in our simulation,” “relative to,” “suggest,” “consistent with,” “under the evaluated conditions.”
- [ ] Shorten figure captions — technical, concise; move interpretation to body text.
- [ ] Full section pass: Intro → Methods → Results → Discussion.

---

### ⚪ Priority 5 — Pre-submission checklist

- [ ] Abstract claims ⊆ Results claims
- [ ] All table/figure numbers match authoritative run
- [ ] Methods parameters match notebook (SEED=42, episodes=300, λ=30, holdout=120, K=20, per-item RL)
- [ ] Limitations cover: synthetic data, single-item RL, discrete actions, DGP–GLM alignment advantage, tied adaptive outcomes (if true)
- [ ] No “structural identification,” “ceiling,” “deployment-ready” in Results
- [ ] Figures exist and match captions (`eda.png`, `8.png`, `10.png`, `15.png`, `17.png`, `20.png`, `21.png`)
- [ ] Optional: reproducibility appendix or anonymized notebook link

---

## Framing decision tree (after audit)

```
Do holdout revenues separate EG-DQN from GLM and Pure DQN?
├── YES (main.tex tables + figures agree)
│   ├── Headline: holdout revenue + significance vs GLM (p=0.003)
│   └── Secondary: convergence + ablation
└── NO (notebook saved outputs: identical adaptive revenues)
    ├── Headline: integration design + ablation + convergence (1.9×, 61% variance)
    ├── Holdout: adaptive methods achieve similar revenue — state in Limitations
    ├── Significance: claim only vs Fixed-Price / Rule-Based
    └── Do NOT claim EG-DQN beats GLM/DQN on holdout
```

### Lead with ✅ (always safe)

1. **Design** — three named, ablated integration mechanisms (state, reward, masking).
2. **Benchmark** — reproducible simulation, known elasticities, five-strategy protocol.
3. **Evaluation rigor** — non-parametric tests, ablation, convergence, elasticity validation.

### Keep in background ⚠️

- Large % lifts vs fixed-price (scope: Item 1, 120-day, simulation).
- Holdout superiority among adaptive methods — **only if ledger confirms separation**.
- “Significant vs all baselines” — **only if every row in significance table rejects**.
- Deployment / A/B readiness — Future Work only.

### Honest narrative templates

**If holdout separates (main.tex tables authoritative):**

> We propose EG-DQN, a hybrid that injects GLM-derived demand signals into DQN via state augmentation, reward shaping, and action masking. In a synthetic three-product simulation (120-day holdout, Item 1), EG-DQN achieves the highest cumulative revenue among five strategies, with significant gains over naive and econometric baselines. Training evidence — faster convergence (~1.9×), lower reward variance (61%), and ablation — supports econometric guidance as a stabilizing prior rather than a stand-alone policy.

**If adaptive methods tie (notebook authoritative):**

> We propose EG-DQN, a hybrid that injects GLM-derived demand signals into DQN via three ablated mechanisms. In a synthetic three-product simulation, adaptive policies substantially outperform fixed and rule-based baselines, while GLM-optimal, pure DQN, and EG-DQN achieve similar holdout revenue; we therefore do not claim holdout superiority among adaptive methods. The main evidence for hybrid integration comes from training dynamics: faster convergence (~1.9×), lower reward variance (61%), and ablation showing state augmentation as the largest contributor.

---

## Claims Ledger (pre-seeded from audit — **you must confirm**)

**Status key:** ✅ supported · ⚠️ partially supported · ❌ not supported · ❓ unconfirmed

| # | Claim in current `main.tex` | `main.tex` table/fig | Notebook saved output | Status | Action |
|---|----------------------------|----------------------|------------------------|--------|--------|
| 1 | EG-DQN +18.3% vs fixed-price | Table revenue: +18.3% | +29.1% (\$226,764 vs \$175,651) | ❓ **Conflict** | Confirm source; revise all % if notebook |
| 2 | GLM +13.9%, DQN +11.9% | Table revenue | GLM/DQN/EG-DQN all +29.1% (tied) | ❓ **Conflict** | If tied: drop differentiated adaptive % |
| 3 | EG-DQN highest revenue (\$151,940) | Table revenue | \$226,764 tied with GLM/DQN | ❓ **Conflict** | If tied: remove “highest among adaptive” |
| 4 | Significant vs all baselines incl. GLM (*p*=0.003) | Table significance | GLM *p*=0.5004, DQN *p*=0.5004 — **NS** | ❓ **Conflict** | If notebook: claim FP/RB only |
| 5 | Convergence ep 35 vs 68 (~1.9×) | Fig. 15, ablation table | Ablation cell references same metric | ❓ | Likely keep — verify from training logs |
| 6 | 61% lower reward variance | Convergence § | Not yet cross-checked | ❓ | Verify from ablation table |
| 7 | State augmentation = largest ablation gain | Table ablation | Ablation cell exists | ❓ | Likely keep — verify \$ amounts |
| 8 | Elasticity bias < 1.44% | Fig. `8.png` | Elasticity validation cell | ❓ | Keep wording fix (no “structural ID”) |
| 9 | MAPE 7.83% mean (table) / 7.2% Item 1 | Forecast table | MAPE ~5.2% in one cell output | ❓ | Reconcile Item 1 vs mean |
| 10 | Adaptive methods tied on holdout | Not stated in paper | GLM = DQN = EG-DQN identical | ⚠️ | **Add to Limitations if notebook true** |
| 11 | DQN training stable | Implied | `Loss=nan` all episodes | ❌ | Investigate if notebook is source |

---

## Critic integration tracker (`critic.md` → blueprint)

| Critic # | Recommendation | Verdict | Blueprint priority |
|----------|----------------|---------|-------------------|
| GLM “ceiling” language | Replace with benchmark/baseline | ✅ Mostly done | P1 verify |
| “Dominate” in conclusion | Soften | ✅ Done | — |
| GLM misspecification limitation | Rename + rewrite | ✅ Done | P1 add “may advantage” clause |
| Structural identification | Remove | ❌ 2 locations remain | P1 |
| Emphasize ablation | Expand | Adopt | P2 |
| Theoretical prediction | Remove | ❌ 1 location | P1 |
| Methodological not theoretical | Rename section | ✅ Done | — |
| Dense / AI prose | Simplify | Adopt | P4 |
| Short figure captions | Trim | Adopt | P4 |
| Reference rebalance | Journal anchors | Partial adopt | P3 |
| Simulation favors method | Acknowledge explicitly | Adopt | P1 |
| Defend simulation in Methods | One sentence + cites | Adopt | P1 |
| Venue / framing | Simulation hybrid OR paper | Adopt | Throughout |
| Notebook ≠ paper | *(not in critic)* | **Critical** | **P0** |

---

## Do we need to re-run the notebook?

### Short answer: **Depends on Priority 0 outcome.**

| Situation | Action |
|-----------|--------|
| `main.tex` tables match the run that produced `17.png`, `20.png`, etc. | **Audit only** — align text to figures |
| Notebook saved outputs are authoritative and match figures | Update `main.tex` tables to notebook; soften claims |
| Paper numbers ≠ notebook **and** figures look like notebook (flat \$6 prices, tied revenues) | **Re-run after fixing** (`Loss=nan`, evaluation bug) |
| Uncertain which run made figures | **Stop** — identify run first; do not edit abstract |

**Known notebook warnings (saved outputs):**
- `Loss=nan` for all training episodes
- Identical holdout revenue and price across GLM / DQN / EG-DQN
- Significance NS vs GLM and Pure DQN

**Rule:** Figures + exported tables define the experiment. Text follows them — not the other way around.

---

## What goes in main text vs appendix

| Main text | Appendix / supplementary (optional) |
|-----------|-------------------------------------|
| Final revenue table (one holdout window) | Sensitivity: seeds, holdout lengths |
| Significance tests (honest subset only) | Runs that showed larger gains |
| One convergence + one ablation figure | Hyperparameter grid (λ, episodes) |
| Core limitations incl. DGP–GLM alignment | Full training logs, `Loss=nan` diagnosis |

**Do not hide contradictory runs.** De-emphasize; document in appendix if used during development.

---

## Division of labour

| Task | You | Assistant |
|------|-----|-----------|
| Confirm authoritative tables/figures | ✅ | |
| Resolve Claims Ledger conflicts | ✅ | audit + recommend framing |
| Add reference PDFs to `references/` | ✅ | |
| Priority 1 language fixes (`critic.md`) | | ✅ |
| Rewrite Abstract / Results / Conclusion | | ✅ (after P0) |
| References rebalance + `xiaocheng2023` fix | | ✅ |
| De-AI prose + caption pass | | ✅ |
| Re-run notebook (if P0 requires) | ✅ | verify after |

---

## Suggested workflow (next sessions)

### Session 1 — Evidence (before abstract)

1. **You:** Confirm whether `main.tex` tables or notebook outputs match your figures (`17.png`, `20.png`).
2. **You:** Paste revenue + significance + ablation tables (or mark ledger rows ✅/❌).
3. **Assistant:** Finalize Claims Ledger + choose framing branch (separate vs tied holdout).
4. **Assistant:** Apply Priority 1 language fixes (quick wins from `critic.md`).

### Session 2 — Abstract & Results

5. **Assistant:** Rewrite Abstract (single paragraph, ledger-aligned).
6. **Assistant:** Restructure Results (holdout layer + training layer).
7. **Assistant:** Update Conclusion + Limitations (DGP–GLM advantage sentence).

### Session 3 — References & polish

8. **You:** Add any missing PDFs to `references/`.
9. **Assistant:** Related Work rebalance, caption trim, de-AI pass.
10. **Both:** Pre-submission checklist (Priority 5).

---

## Target contribution statement (Intro — after ledger confirmed)

> 1. We propose EG-DQN, a hybrid pricing agent that integrates GLM-derived demand signals into DQN via three explicit and ablated mechanisms (state augmentation, reward shaping, action masking).
> 2. We implement a reproducible simulation benchmark with known elasticities and a five-strategy evaluation protocol with non-parametric inference.
> 3. We report [holdout comparisons and/or training convergence and ablation — per ledger] and discuss when econometric priors help RL pricing in simulation.

---

## Re-run decision tree

```
Which run produced your figures (17.png, 20.png, etc.)?
├── main.tex table run → Align notebook export OR update paper if figures differ
└── notebook run → Update main.tex tables + soften claims if adaptive methods tie

Do paper numbers match that run?
├── YES → No re-run. Proceed P1–P4.
└── NO → Can you fix without code changes (wrong export cell)?
    ├── YES → Re-export tables from correct cell
    └── NO → Fix notebook (Loss=nan, eval bug) → re-run SEED=42 once
```

---

*Sources: `critic.md` (external review), `context.md` (method), notebook audit 2026-07-02. No abstract/manuscript edits until Claims Ledger conflicts are resolved.*
