# VitalScope - Lifestyle Disease Risk Screening

A bilingual (English/Hindi) single-page web application for screening six lifestyle-related diseases: type 2 diabetes, heart, hypertension, obesity, fatty liver, and chronic kidney disease. An instant risk evaluation is generated for each of them.

Live app: https://SG975310.github.io/vitalscope/

![VitalScope landing page](assets/screenshot-hero.png)
## Features
For each of the six diseases, there are two modes of operation:
- A quick screening without any lab tests: a transparent set of heuristics based on common risk factors (age, body mass index, physical activity, presence of other conditions) and using more stringent thresholds for BMI/waist circumference for Asian Indians than the ones recommended by the World Health Organization.
- A clinical evaluation for those with test results: a logistic regression classifier trained on five different sets of clinical data for the given condition, with the model coefficients, precision/recall, and training set used available for review. The model is evaluated in-browser on the input data (no data is sent to the server): data is standardized using the mean and variance of the training set, scaled, multiplied by the model’s coefficients, and passed through a sigmoid function (same approach as scikit-learn’s `LogisticRegression`).
Some example UI elements:

For every disease, brief recommendations for prevention and management are also available:
![Prevention and management cards](assets/screenshot-prevention.png)
## Technologies used
A single static HTML page, no server-side code or build steps required: CSS, HTML, and vanilla JS. Google Fonts is used for nicer display fonts. Models are trained in Python and their coefficients are embedded directly in the JS code for evaluation in-browser.

See `docs/METHODOLOGY.md` for more information on the implementation details, as well as known issues and limitations. Contributions are welcome: see `CONTRIBUTING.md` for details. Changes are documented in `CHANGELOG.md`.

## Disclaimer

This web application is not a substitute for professional medical advice or treatment. It is intended solely for screening purposes. If you believe that you may have a health problem, please consult a healthcare professional.
This product is not FDA-cleared or -approved. You should not change or stop any medication or treatment that you are receiving or considering from a healthcare professional without discussing it with the healthcare professional.

## License

MIT. See the [LICENSE](LICENSE) file.
