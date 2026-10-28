# Credit Pre-Screening Engine — Loan Intake Optimisation

**Calibo AI Academy · Path 2 · KLU Batch · Phase 2, Mini Use Case 02 (MUC02)**
**Industry:** BFSI — Retail Lending (unsecured personal loans) · **Team:** Batch 9

---

## The question

A mid-size private bank receives about **3,000 personal loan applications a month**. Every one is reviewed by hand, taking roughly **25 minutes** — about **1,250 analyst hours a month**. Close to half are declined, and many of those are obvious in the first minute.

> Can a pre-screening model safely remove the clear-cut declines from the manual queue — and how many applications and analyst hours would that free up, without wrongly turning away creditworthy customers?

## What we recommend

Auto-decline an application only when the model is at least **90% confident**.

| | |
|---|---|
| Applications auto-declined | **15.2%** — about **456 a month** |
| Analyst time freed | **≈ 190 hours a month** (one full-time analyst) |
| Wrongly declined | **4.7%** of auto-declines — inside the bank's 5% limit |
| Model performance | 79.1% accuracy · 0.867 ROC-AUC · precision 79.1% · recall 75.5% |

Plus one carve-out: **never auto-decline an applicant with no credit history.** The model is only 56.7% accurate on that group — barely a coin toss — because delinquencies, active loans and existing EMIs are all zero by definition for them. Routing them to a human lets the threshold drop to 0.88 and frees about **205 hours a month at a lower 4.1% error rate** — more volume and less risk at the same time.

## Team and section ownership

| Member | Role | Sections owned |
|---|---|---|
| Mohammed Mutee Ruknuddin | Data Analyst | Step 5.1 cleaning · CBIM Problem Canvas |
| Mohammed Saalif Udyawar | Insights Analyst | Steps 5.2, 5.3 — approval drivers, affordability features |
| Mulla Javeed | Model Builder | Steps 5.4, 5.5 — logistic regression, evaluation, threshold |
| Nagothu Varshitha | Policy Synthesiser | Steps 5.6, 5.7 — fairness check, Credit Policy Briefing |

## What is in this folder

### Submission deliverables

| File | What it is |
|---|---|
| `Batch9_MUC02_KLU_Notebook.ipynb` | The analysis. All seven steps, code plus a markdown explanation in each. Runs end-to-end from a clean kernel with no errors. |
| `Batch9_MUC02_KLU_Summary.docx` / `.pdf` | One-page insight summary — three findings, the recommendation, one limitation, and who owned which section (446 words, group limit 500). |
| `Batch9_MUC02_KLU_CBIM_Canvas.docx` / `.pdf` | The problem canvas, completed by all four members before any code was written. Also reproduced at the top of the notebook. |
| `Batch9_MUC02_KLU_Individual_Reflections.docx` / `.pdf` | A 150-word note from each member — what they contributed and what they learned from a teammate's section. |
| `Batch9_MUC02_KLU_Presentation.pptx` | 17-slide deck, about 22–24 minutes across the four speakers. |
| `MUC02_Credit_PreScreening_Dataset.csv` | The source data — 12,000 applications, April–September 2026, 19 columns. Keep it beside the notebook. |
| `figures/` | The six charts from the notebook, exported as PNGs. |

### Team working documents (`team-docs/`)

Not part of the submission — these are for the team.

| File | What it is |
|---|---|
| `Batch9_MUC02_KLU_Presentation_Script.docx` / `.pdf` | Slide-by-slide speaking script with timings, the handover lines, 17 expected questions with short answers, and the ten numbers everyone should know. |
| `Batch9_MUC02_KLU_Use_Case_Story.docx` / `.pdf` | The whole project in one read — what we did, why, how, and what it is worth — in plain English with the figures embedded. |

## How to run the notebook

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook Batch9_MUC02_KLU_Notebook.ipynb
```

The dataset must sit in the same folder as the notebook. Then restart the kernel and run all cells.

## What we found

| Finding | The number | Why it matters |
|---|---|---|
| **The bank has an unwritten credit score floor at 650** | 3.0% approved below 650 (n=1,179) against 32.5% just above | A tenfold jump across one point is a rule, not a gradient — the safest thing in the dataset to automate |
| **Approvals collapse past 50% FOIR** | 65.4% → 47.4% → 31.7% → 15.6% | Supports writing a 50% FOIR limit and a 50–55% referral band into credit policy |
| **Gig workers are declined on affordability, not occupation** | 34.9% approved vs 58.4% salaried; median income ₹28,000 vs ₹50,000 | Defensible — and it points to a product fix (smaller ticket, longer tenure) |
| **The model rediscovered both cliffs on its own** | FOIR +1.22, credit score −1.01, delinquencies +0.98 | Strongest evidence it learned real credit policy, not an artefact of this sample |
| **The model cannot read new-to-credit applicants** | 81.2% accuracy with a score vs 56.7% without | Structural, not statistical — three of its four strongest features are zero by definition for them |
| **The model does not widen the gender gap** | 15.6% of women auto-declined vs 15.0% of men, against 3.7 points in the human decisions | Measured rather than assumed — and monitored monthly after launch |

## The honest limitation

The target is the credit team's **past decision**, not whether the customer repaid. The model learns to copy historical analyst judgement, including any inconsistency or bias in it. So "4.7% wrong" means "4.7% disagreed with an analyst" — not "4.7% were bad credit risks."

Before going live the bank should shadow-run the lane for one month and start capturing repayment outcomes.

---

*Submitted for Calibo AI Academy Phase 2, Mini Use Case 02. The dataset is simulated and contains no real customer data.*
