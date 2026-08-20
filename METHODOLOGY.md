# Methodology

This document describes exactly how VitalScope calculates the risk score, for those
who are interested in the details rather than trusting us blindly.

## Two modes, two approaches

### 1. Quick screening (heuristic, points-based)

Does not require lab values. Each answer is worth a certain number of points,
based on well-established clinical risk factors for each condition. Points are
added up and then scaled to a 0-100% risk index. Straightforward, simple to
implement, fully transparent: every point value is visible in the factor
breakdown.

Asian-Indian-specific cutoffs. BMI and waist-circumference categories use the
2009 Asian Indian consensus guidelines (overweight: ≥23 BMI, obese: ≥25 BMI; waist
≥90cm for men and ≥80cm for women) rather than the WHO cutoffs, which are higher.
Asian Indians have increased risk of metabolic diseases at lower BMI and waist
circumference than the global population, on average, so applying the same
cutoffs to them would systematically underestimate risk.

### 2. Clinical mode (trained logistic regression)

For those who have the lab values. Actually runs a logistic regression: the

weights, standardization parameters, and intercept used are the same as in
scikit-learn's
`LogisticRegression(solver="liblinear", C=1)`
, applied to the same

datasets.
The process is as follows:
standardize (z-score) the input
features using the training set's mean and standard deviation
→ take a weighted sum with the learned coefficients and add the intercept
→ apply the sigmoid function to get a probability between 0 and 1.
The coefficients, standardization parameters, and intercept for all five
clinical models are hard-coded into
`index.html`
as the
`MODELS`
object.
Nothing is simulated or approximated; this is the same code that runs on the
page, except that in production, it runs in the user's browser (for privacy)
rather than on the server.
Missing values: if a field is left blank, it is replaced with the median value
for that feature in the training set (or, in the case of the Framingham study,
the mean, since medians were not reported there; note that using the mean is
mathematically equivalent to saying that the feature contributes 0 to the
standardized value). This is explicitly stated in the "Missing values" note in
the UI: leaving a field blank results in an estimate rather than an exact
calculation.
## Datasets

Model Dataset Records Validation
Diabetes Pima Indians Diabetes Dataset 768 5x stratified CV (seed=42)
Heart disease Cleveland Heart Disease Dataset (UCI, reprocessed) 297 (6 excluded due to missing data) 5x stratified CV (seed=42)
Hypertension Framingham Heart Study Dataset 4240 20% held out (seed=42)
Liver disease (fatty-liver proxy) Indian Liver Patient Dataset 583 20% held out (seed=42)
Chronic kidney disease UCI Chronic Kidney Disease Dataset 400 20% held out (seed=42)

The liver disease model is a bit of an edge case: the Indian Liver Patient Dataset classifies general liver patients (with cirrhosis or hepatitis) versus healthy controls, rather than specifically fatty-liver patients. We treat it as a general indicator of liver health rather than a fatty-liver-specific one, and note this in the UI.

## Reported metrics

Accuracy, precision, recall, F1, and AUC are all reported as the mean and standard
deviation across all cross-validation folds (for cross-validated models) or the
held-out test set (for models with a separate test set), not the training set.
These are meant to serve as a reference for a screening tool rather than a
replacement for established clinical risk scores such as FINDRISC or the
ASCVD Risk Estimator, which are based on large-scale studies.

## Verified example cases

The "Verified example cases" table on the page contains a few example inputs and
their corresponding probability, calculated using the same production formula
as above. This is for transparency: anyone, including a regulatory
scrutiny auditor, can independently verify that the reported probability for
any given input matches the coefficients published in the source code.

If you change any of the coefficients, standardization parameters, or the
`predictLogReg`
function itself, you should update the table as well, or it
will become out-of-date and misleading, see
CONTRIBUTING.md
.

## This is not a diagnostic tool

This tool is not a diagnostic device. It does not take the place of blood tests,
professional medical advice, or a physical exam. The datasets it was trained on
are not necessarily representative of all populations (in particular, the Pima
Indians Diabetes Dataset was collected from a specific ethnic group), so it may
not be as accurate for other groups. This is a limitation of clinical datasets in
general, not this tool specifically.