# round-4 — Reconstruct

**Team:** BB-014  
**Queries used:** Controlled perturbation queries from the Round 4 investigation

## What we concluded

We reconstructed the observable behavior of the Blackbox system using controlled input perturbations and the input/output examples collected across the investigation.

The system produces a continuous score together with a final decision. The score is not determined by a single input feature. Instead, multiple numerical and categorical inputs contribute to the output, and some interactions between features appear to matter.

Our Round 4 reconstruction uses the observed feature set:

- `age`
- `baseline_score`
- `comorbidity_ratio`
- `dependants`
- `prior_visits`
- `recent_admissions`
- `requested_beds`
- `vitals_index`
- `ward`
- `years_registered`

The reconstruction was implemented in `BB_014.ipynb` and tested against the collected observations.

## How we got there

We started from the behavior observed in the earlier rounds and then used controlled perturbations in Round 4.

The important experiments changed one input at a time while keeping the other inputs fixed. This allowed us to determine whether the output changed when a particular feature changed.

We then tested combinations of changes to identify effects that were not explained by a single feature independently.

Examples of useful perturbations included changes to:

- `baseline_score`
- `age`
- `comorbidity_ratio`
- `dependants`
- `prior_visits`
- `recent_admissions`
- `requested_beds`
- `vitals_index`
- `ward`
- `years_registered`

The collected outputs showed that small changes in some numerical inputs can produce small score changes, while larger or more unusual changes can produce substantially different scores.

The reconstruction model was then built from these observations and evaluated against the previously collected examples.

## What we ruled out

We ruled out the hypothesis that the final decision is controlled by only one visible input.

We also ruled out the idea that changing every input has the same effect on the score. The perturbation experiments showed different sensitivities across features.

We found that the `ward` value can affect the score, so it should not be treated as irrelevant.

Similarly, changes to numerical features such as `age`, `baseline_score`, and `comorbidity_ratio` can change the score while the remaining inputs stay fixed.

The observed examples also show that a high score generally corresponds to `APPROVE`, while the observed low-score example corresponds to `DECLINE`. However, we do not claim that a single universal threshold completely explains every possible case without further unseen tests.

## What we are still unsure about

The reconstruction is based on black-box observations and therefore cannot expose the original internal implementation.

We are still uncertain about:

1. The exact mathematical form of the original scoring function.
2. Whether the original system applies hidden transformations or normalization to some inputs.
3. Whether there are additional feature interactions that were not covered by our queries.
4. The exact decision threshold and whether it is fixed across all possible inputs.
5. Whether there are hidden preprocessing, clipping, rounding, or feature-bucketing steps.
6. How the system behaves on combinations of values outside the ranges explored during the investigation.

Therefore, our reconstruction should be interpreted as a behavioral approximation supported by the observed queries rather than a claim that we recovered the original source implementation exactly.
