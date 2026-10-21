# NPPAD Digital Twin — Task E (E1–E5)

A standalone, offline 3D digital twin of the PCTRAN pressurized-water reactor
for Task E (NPPAD accident diagnosis), serving the diagnosis of the **core
Task E notebook** — E1 dataset, E2 classical baseline, E3 deep models, E4
early diagnosis, E5 realistic instrumentation. The notebook aligns the
diagnosis windows to the plant's real response onset.

**Live (GitHub Pages):** https://saurabhkulkarni77.github.io/npp-digital-twin-e1e5/

## What it shows

- The notebook's 12 held-out **test** runs (one per accident class, median
  severity — the same runs its prediction files score), played on the 3D
  plant with channel traces, playback/scrub, and truth reveal.
- **Diagnosis = the notebook's four classifiers**, switchable in the panel
  (CNN is the default): Logistic regression (starter baseline),
  Logistic regression + (log10-compressed features, tuned C), the 1D CNN and
  the GRU. Probabilities are taken verbatim from the notebook's saved outputs
  (`results/probs/*.npz`; CNN/GRU averaged over seeds 0/1/2) and cross-checked
  against its `outputs/pred_E_*_W60_test.csv` files — 192/192
  run×setting×model checks pass. (There is no random forest: Homework E2
  offers logistic regression *or* random forest, and this notebook took the
  logistic-regression branch.)
- A **"Models used in this notebook"** panel at the top lists all four models
  with their macro-F1 at the default setting and drives the same switcher.
- Each model reports **one prediction per evaluated window**
  (W = 30/60/120/300/600 s), so the panel shows **window snapshots**: the
  W snapshot appears at t = t0 + W and stays until the next window completes.
  Nothing is interpolated between windows.

## Settings (toggle in the diagnosis panel)

| Setting | Test macro-F1, CNN (notebook report, mean of 3 seeds) |
|---|---|
| Onset-aligned t0 = 100 s · realistic (default) | W=30 0.961 · W=60 0.967 · W=120 0.968 · W=300 0.960 · W=600 0.971 |
| Onset-aligned t0 = 100 s · all 96 variables | W=30 0.989 · W=60 0.985 · W=120 0.989 · W=300 0.983 · W=600 0.985 |
| Handout t0 = 20 s · realistic | W=30/60 0.189 (ceiling) · W=120 0.976 · W=300 0.971 · W=600 0.960 |
| Handout t0 = 20 s · all 96 variables | W=30/60 0.189 (ceiling) · W=120 0.992 · W=300 0.988 · W=600 0.990 |

The notebook's main prediction file (seed-averaged probabilities, onset-aligned,
realistic, W = 60 s) scores macro-F1 **0.972**. With handout windows every
W ≤ 60 s window is one of only four distinct inputs, so no model can beat the
0.189 information ceiling — the notebook's central finding, reproduced here by
construction.

This twin serves **only what the core notebook contains**: its four Task E
classifiers (E1–E5, no extensions), no other models, no conformal sets, no
explainability add-ons.

## Deployment

GitHub Pages on this repo (`main`, root). It does not touch the other twins
(repos `npp-digital-twin` and `npp-digital-twin-hw2`) or their AWS buckets.
