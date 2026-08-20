# Changelog

All notable changes to this project are documented here.

## [1.0.1] — 2026-08-20

### Fixed
- **Verified example cases table was stale.** The probabilities shown for the
  worked examples (diabetes, heart disease, kidney disease) didn't match what the
  published `predictLogReg` formula and `MODELS` coefficients actually produce when
  run. Recomputed all affected rows directly from the production formula:
  - Diabetes (age 50, glucose 170, BMI 34, BP 88, 4 pregnancies): 77.9 → **76.9**
  - Diabetes (age 25, glucose 90, BMI 22, BP 70, 0 pregnancies): 2.3 → **2.8**
  - Heart disease (age 61, male, cp3, chol 275, exercise angina): 83.3 → **70.9**
  - Heart disease (age 42, female, cp0, chol 190, no angina): 0.3 → **4.4**
  - Kidney disease (age 40, no htn/dm, hemoglobin 14.5, creatinine 0.9): 8.4 → **15.6**
  - Kidney disease (age 63, htn+dm, albumin 3, hemoglobin 9.8, creatinine 2.8):
    unchanged (~100 was already accurate).

  Risk bands (Low/Moderate/High) were unaffected by this fix — only the displayed
  numeric probabilities changed. The live calculator itself was never affected;
  this was a documentation/reference-table accuracy issue only.

## [1.0.0] — Initial release

- Quick screening (points-based heuristic) and clinical mode (trained logistic
  regression) for six lifestyle-driven conditions: diabetes, heart disease,
  hypertension, obesity, fatty liver, chronic kidney disease.
- Bilingual English/Hindi support.
- Asian-Indian-specific BMI and waist-circumference cutoffs.
- Fully client-side, no backend, no tracking.
