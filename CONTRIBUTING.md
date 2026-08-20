# Contributing

Thanks for taking a look at VitalScope. It's a single self-contained `index.html` —
no build step, no package manager, no dependencies to install.

## Running it locally

Just open `index.html` in a browser. That's the whole setup:

```bash
git clone https://github.com/YOUR-USERNAME/vitalscope.git
cd vitalscope
open index.html   # macOS
# or: xdg-open index.html   (Linux)
# or: start index.html      (Windows)
```

No server required — everything runs client-side.

## Project structure

```
index.html            the entire app (HTML + CSS + JS in one file)
docs/METHODOLOGY.md    how the risk scores are computed, in detail
README.md
LICENSE
```

## Making changes

- **UI/content changes** — edit `index.html` directly. Styles are in the `<style>`
  block, logic in the `<script>` block at the bottom.
- **Translations** — English and Hindi strings live in the `STR` / `RECS_STR`
  objects in the script. Keep both language objects in sync when adding new keys.
- **Model coefficients** — the `MODELS` object holds `intercept`, `scaler_mean`,
  `scaler_scale`, `coef`, and (where applicable) `impute_median` for each of the five
  clinical models. If you retrain a model and update these values, you **must** also
  recompute the "Verified example cases" table (see below) — the two are meant to
  match exactly, and a mismatch is exactly the kind of thing that erodes trust in a
  tool like this.

## Recomputing the verified example cases

The verified-cases table exists so a skeptical reader can hand-check that the shown
formula actually produces the shown numbers. If you change any coefficient, re-derive
each example by running it through `predictLogReg` (or an equivalent re-implementation
in Python/any language) using the exact same missing-value and standardization logic
described in [`docs/METHODOLOGY.md`](docs/METHODOLOGY.md), then update the table.

Don't hand-estimate these numbers — that's the mistake this table exists to prevent.

## Reporting a problem

Open an issue using the bug report template in `.github/ISSUE_TEMPLATE/`. Include:
- What you expected vs. what happened
- The exact inputs you used (age, values entered, mode selected)
- Browser/OS if it's a rendering issue

## Medical content changes

If you're updating prevention/management guidance text, cite a source (WHO, ICMR,
a peer-reviewed guideline, etc.) in the PR description. This is a health tool used by
real people — unsourced claims don't get merged.
