# CLAUDE.md — HRV Predictor (Garmin Fitness Project)

> Last updated: 2026-04 — current step: **EDA** (`01_eda.ipynb`)

---

## Project

Predict tomorrow's HRV (D+1) from today's biometrics (D).
Core goal: learn the ML pipeline end-to-end on synthetic data, then transfer to real Garmin data via `garminconnect`.

---

## Dataset

| File | Description |
|------|-------------|
| `data/garmin_like_daily_clean.csv` | 730 days, 24 columns, 0 missing values, synthetic, single individual (male, 30y, 95–100 kg) |
| `data/garmin_like_data_dictionary.csv` | Full data dictionary — 55 entries |

`garmin_like_modeling_ready.csv` exists but **will not be used** — feature engineering will be done manually from `daily_clean`.

---

## Key decisions

| Decision | Rationale |
|----------|-----------|
| Target = `hrv_ms` shifted by -1 (D+1) | Real prediction — D+1 is unknown at prediction time |
| `hrv_ms` excluded from features | Leakage — it's the basis of the target |
| `soreness_score` and `training_rpe` excluded | Manual input — not exportable automatically via Garmin |
| `readiness_score` imported via `garminconnect`, not modeled | Already computed by Garmin — no value in replicating it |

---

## Pipeline

```
1. EDA                     ← CURRENT STEP
2. Feature engineering     ← D-1 shifts, rolling loads, encodings
3. Feature selection       ← correlations, importance, leakage check
4. Modeling                ← model choice left to the user
5. Evaluation              ← metric choice left to the user
6. Interpretability        ← SHAP (to discover)
7. Real data bridge        ← garminconnect package
```

---

## Project structure

```
fitness-predictor/
├── data/
│   ├── garmin_like_daily_clean.csv
│   └── garmin_like_data_dictionary.csv
├── notebooks/
│   └── 01_eda.ipynb
├── src/
├── outputs/
│   ├── figures/
│   └── models/
├── CLAUDE.md
└── requirements.txt
```

---

## Collaboration rules

| | Rule |
|-|------|
| ✅ | Actionable blocks of 30–90 min max — do not deliver everything at once |
| ✅ | Complete, functional code — zero pseudo-code |
| ✅ | Explain the *why* behind technical choices, not basic syntax |
| ✅ | Explain the theorical choices, why this model ? why that metric ? why this loss ? etc. |
| ✅ | Socratic tone: ask questions before giving answers |
| ✅ | Always work from `garmin_like_daily_clean.csv` unless explicitly told otherwise |
| ✅ | Flag structural decisions — do not make them on the user's behalf |
| ❌ | Do not suggest models, metrics, or approaches — let the user think first |
| ❌ | No over-engineered architectures given the time constraints |
| ❌ | No explanations of `for`, `if`, `print`, or basic pandas |
| ❌ | Do not jump ahead to future steps while the current one is unfinished |