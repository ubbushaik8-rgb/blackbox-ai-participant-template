# round-1 — Observe

**Team:** BB-014
**Queries used:** 7 / budget

## What we concluded

GK-02 returns a score between 0 and 1 and approves every input we have tried so far, with scores ranging from 0.57 to 0.75. Setting every field to its maximum lowers the score, so at least one field has a negative effect at its high end. The `ward` field has a very small effect. `baseline_score` and `vitals_index` appear to raise the score, but our evidence for both is confounded (see below).

## How we got there

1. **Query 1:** All fields were at the midpoint of their range, with ward A. The score was 0.6824 and the decision was APPROVE. This was our baseline.

2. **Query 2:** All numeric fields were set to their maximum, with ward A. The score was about 0.572, which was lower than the baseline. This shows that the system is not simply "higher inputs = higher score".

3. **Query 3:** All fields were at the midpoint, but `baseline_score` was set to 900. The score was 0.7264.

4. **Queries 5 to 6:** All numeric fields were at their maximum, and only `ward` was changed. The score changed by -0.0074, suggesting that `ward` has a small effect.

5. **Query 7:** `baseline_score` was 900, `vitals_index` was 100, and `ward` was D, while the other fields were at their midpoint. The score was 0.7500, which was our highest score so far.

## What we ruled out

- **"Higher values always give a higher score":** Ruled out by Query 2, where all maximum values produced a lower score than Query 1 with midpoint values.

- **"ward is a major driver":** Weakened by Queries 5 and 6, where changing only `ward` changed the score by 0.0074.

## What we are still unsure about

- Which individual fields are responsible for the drop at the maximum. We changed many fields at once in the early queries, so we cannot attribute the effects to individual fields yet.

- The effect of `baseline_score` and `vitals_index`. Queries 1 and 3 also differ slightly in age (50 vs 46.5), and Query 7 changed several fields at once, so these effects are not isolated.

- Where the APPROVE/REJECT cutoff is. We have not seen a REJECT yet.

- Whether any field has a non-monotonic effect (a peak in the middle of its range) or interacts with another field.
