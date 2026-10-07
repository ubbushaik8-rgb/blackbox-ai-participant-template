# round-2 — Investigate

**Team:** BB-014  
**Queries used:** 150 / budget

## What we concluded

GK-02 returns scores between 0 and 1 with decisions of APPROVE or DECLINE. In Round 2, we observed a much wider range of scores, from 0.4287 to 0.9829.

The highest observed score was 0.9829 in Queries 148 and 150, both with an APPROVE decision. We also found that changing individual features can affect the score. In particular, increasing age from 25 to 51.63 while keeping the other inputs fixed increased the score from 0.5097 to 0.6433.

We also observed a DECLINE decision at Query 7 with a score of 0.4287, showing that the system can produce both APPROVE and DECLINE decisions.

## How we got there

1. **Queries 1–6:** We tested controlled changes to individual fields. Query 1 scored 0.6873, while Queries 2–4 showed that changing the ward and years registered produced small score changes.

2. **Queries 5–6:** All fields were kept the same except age. Age increased from 25 to 51.63, and the score increased from 0.5097 to 0.6433. This suggests that age has a positive effect under these conditions.

3. **Query 7:** The input produced a score of 0.4287 with a DECLINE decision. This confirmed that the system does not always approve inputs.

4. **Queries 8–45:** We tested changes to baseline_score, years_registered, ward, recent_admissions, requested_beds, dependants and other fields. These produced different score changes, showing that feature effects are not uniform.

5. **Queries 46–49:** We tested combinations of age, baseline_score, dependants, vitals_index, ward and other fields. The scores ranged from 0.7201 to 0.8072.

6. **Queries 50–150:** We performed more fine-grained changes to individual features and observed scores mostly around 0.97–0.98. Query 150 produced our highest observed score of **0.9829**, tied with Query 148.

## What we ruled out

- **"The system always approves":** Ruled out by Query 7, which produced a DECLINE decision with a score of 0.4287.

- **"Higher values always produce higher scores":** Not supported. Different features produced different effects, and increasing values did not consistently increase the score.

- **"ward is the only important categorical feature":** Not supported. Although ward changes affected the score, other numeric features also produced noticeable changes.

- **"The score has a narrow fixed range":** Ruled out. We observed scores from 0.4287 to 0.9829.

## What we are still unsure about

- The exact APPROVE/DECLINE cutoff. We observed a DECLINE at 0.4287 and APPROVE at 0.5097, but we have not identified the exact boundary.

- The exact contribution of every individual feature. Some experiments changed multiple features together, so their effects cannot always be separated.

- Whether the effect of a feature changes depending on the values of other features.

- Whether the highest score of 0.9829 represents a general maximum or only the highest value observed within our 150-query budget.
