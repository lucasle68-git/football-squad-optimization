# Moneyball for the Premier League: A Data-Driven Squad Optimisation Engine

> **Applying Multiple Linear Regression + Linear Programming to solve a €100M football recruitment problem, built for Wasserman, the global sports agency.**

[![Status](https://img.shields.io/badge/status-completed-success)]()
[![Methods](https://img.shields.io/badge/methods-Regression%20%7C%20LP%20%7C%20Sensitivity%20Analysis-blue)]()
[![Tools](https://img.shields.io/badge/tools-Excel%20Solver%20%7C%20Python-orange)]()
[![Domain](https://img.shields.io/badge/domain-Sports%20Analytics-red)]()

---

## The Problem in One Line

> *"Which 23 players should Chelsea sign for €100M to maximise long-term squad potential?"*

A real-world resource-allocation problem at the intersection of **sports analytics, financial optimisation, and player valuation**, the kind of decision Premier League clubs make every transfer window, where a single recruitment mistake now costs an average of **€10M** (up from €3.2M a decade ago).

---

## Results at a Glance

| Metric | Value |
|---|---|
| **Optimal Squad Potential Score** | **1,981** |
| **Total Spend** | **€99.745M** (of €100M budget) |
| **Statistical Value Upside** | **+€23.5M** (predicted vs. actual) |
| **Squad Composition** | 3 GK · 6 DF · 8 MF · 6 FW |
| **Regression Model Accuracy** | **R² > 98%** across all four position-specific models |
| **Robustness** | Solution stable across €70M–€120M budget range (–0.4% sensitivity) |

**Headline insight:** The model identified that the market systematically *underprices high-potential young players* — including names like Vinícius Jr. (+€8.35M value gap), Rodrygo (+€4.91M), and Hudson-Odoi (+€3.12M) — letting clubs *"buy future superstars at today's current-skill prices."*

---

## Why This Project Matters

Most management science coursework optimises factory schedules or warehouse routing. We chose a harder, more interesting problem:

**Niche domain**: Football industry data is messy, multi-dimensional, and emotionally loaded with intuition bias.
**Real organisation**: Wasserman is an actual global player agency operating at the data + deal-execution intersection.
**End-to-end pipeline**: From 18,278 raw player records → cleaned position-specific datasets → predictive regression → optimisation under constraints → robustness testing → strategic recommendations.
**Commercial framing**: Every output is tied back to a business decision a recruitment director would actually make.

---

## Methodology (Three Linked Models)

```
   ┌─────────────────────────┐     ┌─────────────────────────┐     ┌─────────────────────────┐
   │  STEP 1: VALUATION      │     │  STEP 2: VALUE GAP      │     │  STEP 3: OPTIMISATION   │
   │  Multiple Linear        │  →  │  Identify Undervalued   │  →  │  Linear Programming     │
   │  Regression             │     │  Players (Residuals)    │     │  (Excel Solver)         │
   │                         │     │                         │     │                         │
   │  Y = Market Value       │     │  Predicted > Actual     │     │  Maximise Σ Potential   │
   │  X = 6 Attributes       │     │  → "Bargain"            │     │  s.t. Budget + Position │
   └─────────────────────────┘     └─────────────────────────┘     └─────────────────────────┘
```

### Step 1 — Predictive Valuation (Regression)
- **18,278 players** from FIFA 20 Complete Dataset (Kaggle), 104 attributes → 6 selected predictors
- Predictors: `age`, `overall`, `potential`, `wage_eur`, `international_reputation`, `release_clause_eur`
- **Position-specific models** (FW, MF, DF, GK) because age depreciation, reputation premium, and skill weighting differ by role
- **All four models statistically significant** (p < 0.001) with R² between 0.987 and 0.992

### Step 2 — Undervaluation Detection
- Compute residuals (Predicted − Actual market value)
- Positive residual = market underprices the player vs. their measurable attributes
- Shortlist filtered to **age ≤ 18** to target the asymmetry the market gets most wrong: future ceiling

### Step 3 — Squad Optimisation (Linear Programming)
- **Decision variable:** `xᵢ ∈ {0, 1}` — select or skip player *i*
- **Objective:** `Maximise Σ (Potentialᵢ × xᵢ)`
- **Constraints:** Budget ≤ €100M, Squad size = 23, Min GK ≥ 3, Min DF ≥ 6, Min MF ≥ 6, Min FW ≥ 4
- Solved using **Simplex LP method** in Excel Solver

### Step 4 — Sensitivity & What-If Analysis
- Shadow prices, binding/non-binding constraint analysis
- Stress tests: budget shocks (–10%, –20%, +20%), tactical shifts (attacking vs defensive), age band trade-offs (≤18, 20–21, 25–27)
- Reduced-cost analysis to identify "near-miss" alternative signings

---

## The Three Most Interesting Findings

### 1. Reputation costs 14× more than actual skill
Every 1-point increase in `international_reputation` adds **€614,000** to market value. Every 1-point increase in `overall rating` adds only **€43,000**. The market overpays massively for fame. The model exploits this by filtering reputation-heavy stars and targeting high-overall, low-rep young players.

### 2. The budget isn't the binding constraint
Shadow price on the €100M budget is **0** — there's €255K of slack. The actual binding constraints are **squad size** (shadow price +86: each extra slot adds 86 potential points) and **minimum goalkeeper count** (shadow price −4: forcing in a 3rd keeper costs 4 potential points). Strategic implication: lobby for squad-size flexibility, not more money.

### 3. Youth dominance is structural, not coincidental
Re-running the LP on age band 25–27 drops squad potential from 1,981 → 1,752 (−229 points, −11.6%). The asymmetry isn't a quirk of the dataset — it's a genuine market inefficiency that clubs willing to commit to long-horizon development can systematically exploit.

---

## Repository Structure

```
football-squad-optimization/
├── README.md
├── data/
│   └── finaldataset.xlsx
├── analysis/
│   ├── LP_Sensitivity_Report.xlsx
│   └── LP_Sensitivity_WhatIf.xlsx
└── presentation/
    └── Management_Science_Project.pdf
```

---

## Tech Stack

| Layer | Tools |
|---|---|
| **Data preparation** | Python (pandas) for position classification & missing-value imputation |
| **Statistical modelling** | Excel Data Analysis ToolPak — Multiple Linear Regression |
| **Optimisation** | Excel Solver (Simplex LP) |
| **Sensitivity testing** | Excel Solver sensitivity report + manual what-if scenarios |
| **Communication** | PowerPoint deck + 2,000-word written report |

I deliberately chose **Excel + Solver over Python/PuLP** because the deliverable was a board-level decision tool: a recruitment director needs to interrogate constraints live, not read a Jupyter notebook. Reproducibility for technical reviewers is preserved via the documented formula structure.

---

## My Role — Team Lead

I led a team of four on this project. My responsibilities and contributions:

- **Project framing.** Pitched and defended the football-agency angle to the team (against safer choices like supply-chain or finance) — argued that the niche was an advantage, not a risk.
- **Methodology design.** Owned the decision to use **position-specific regressions** rather than one pooled model — a single model would have masked the very effects (reputation premium varies 2× across positions) that drive the strategic recommendations.
- **Workstream coordination.** Split the team across four workstreams (data prep, regression, LP, sensitivity & strategy) with clear interfaces, weekly checkpoints, and integration milestones.
- **Quality control.** Personally validated the regression diagnostics (multicollinearity, residuals, position-level RMSE) and the LP constraint formulation before integration.
- **Narrative integration.** Wrote and structured the final report and presentation flow.

**Outcome:** Awarded an **Excellent grade (A)** by the University of Glasgow.

---

## Honest Limitations (What I'd Do Differently)

A strong analyst should be the first critic of their own work:

1. **Single-snapshot data.** FIFA 20 is one moment in time. Real recruitment uses rolling Opta/StatsBomb feeds. Mitigation in roadmap.
2. **Multicollinearity risk.** `overall`, `potential`, `reputation`, and `release_clause` overlap conceptually. Coefficients are interpretable but should not be treated as causally clean. Ridge regression or PCA would be a natural extension.
3. **LP assumes static prices.** No bidding war modelling, no add-on/sell-on clauses, no agent fees. The roadmap (docs/04) proposes a 5–10% budget buffer and stochastic price modelling.
4. **Single-objective optimisation.** Real squad-building is multi-objective (potential + tactical fit + chemistry + commercial value). We tested three weighted-objective variants (Potential-first / Value-Gap-first / Balanced) — full comparison in `docs/03`.

---

## What This Project Demonstrates

For anyone reading this as a hiring signal, the project demonstrates:

- **Translating an ambiguous business question** ("who should we sign?") into a formally solvable model.
- **Cross-method fluency** — combining inferential statistics, deterministic optimisation, and post-hoc sensitivity analysis in a single coherent pipeline.
- **Honest uncertainty quantification** — shadow prices, reduced costs, and stress tests integrated as first-class outputs, not afterthoughts.
- **Domain immersion** — the recommendations reference Financial Fair Play, transfer-window dynamics, registration rules, and agency economics, not just statistical metrics.
- **Communication.** A 50+ slide deck and 2,000-word report distilled to a one-line answer a board can act on.
- **Leadership.** Owned framing, structure, and delivery for a four-person team under a hard deadline.

---

## Get in Touch

Open to **data analyst, business analyst, sports analytics, and management consulting** roles in the UK.

- [LinkedIn](https://www.linkedin.com/in/lucasle68/)
- [GitHub](https://github.com/lucasle68-git)

If you're working on data-driven decision-making problems, especially in sport, transport, or consulting, I'd love to talk.

---

## Course & Acknowledgements

- **Course:** MGT5426 — Introduction to Management Science
- **Institution:** Adam Smith Business School, University of Glasgow
- **Year:** 2025/26
- **Team:** Group 1, names available on request

Dataset: [FIFA 20 Complete Player Dataset](https://www.kaggle.com/datasets/stefanoleone992/fifa-20-complete-player-dataset) (Kaggle, public).

---

<p align="center"><i>The model gives the direction. The agency closes the deal.</i></p>
