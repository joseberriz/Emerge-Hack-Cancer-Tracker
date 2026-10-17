# eMerge Hackathon Challenge 1 Guide

This guide covers the thinking behind the challenge, what makes it difficult, and what the judges will be looking for. For details on the files and columns, see the dataset `README.md`.

## Background

Cancer survivors live with physical, psychological, financial, and practical needs that change over time. Most screening finds problems that are already present. If we could anticipate which survivors are heading toward an emergency visit or hospital stay, care teams could step in earlier, with symptom management, financial navigation, or a phone call.

Patient-reported outcomes (PROs) capture what clinical records often miss: fatigue, pain, distress, trouble paying for care, a family member who had to stop working. Clinical records capture what surveys miss: stage, treatment, comorbidities, every encounter. Even within one health system, these two kinds of data are collected through different platforms and on different schedules, and key fields such as cancer stage are frequently missing. Researchers working across institutions or with public data face a harder version: each dataset captures part of the picture, and they rarely describe the same patients.

For this hackathon we combined a real national survey of cancer survivors (MEPS) with synthetic clinical records (Synthea) tuned to resemble a South Florida survivorship cohort. They share no patient key.

## Challenge 1: From Fragmented Data to Future Needs

You have 2017 survey answers from real survivors, their 2018 health care use for a development set, and a separate set of synthetic clinical records for different people. We would like you to predict which survivors will need acute care in 2018, identify the factors associated with that risk, and show whether the clinical records add anything beyond what the survey alone can tell us.

Three decisions sit at the center of the challenge, and there is no single right answer to any of them:

- **How to define risk.** The scored outcome is any ER visit or hospital stay in 2018. You may also propose other definitions, such as costs or worsening patient-reported health, if you can justify them clinically.

- **How to frame the prediction.** For example, 2017 measures predicting 2018 outcomes, or change between the two years.

- **How to integrate the sources.** Matching on shared variables, aligning subgroups rather than individuals, learning relationships in one source and transferring them to the other, or deciding that some parts should not be aligned at all.

AI tools can write a join in seconds. The difficulty is choosing an integration you can defend, and knowing where it should not be trusted.

## Evaluation Criteria

There is no single leaderboard metric. The judges will consider the aspects below, roughly equally. The questions in the right-hand column are there to help you think, not a checklist you have to tick off.

| Aspect               | Questions worth asking yourself                                                                                                                |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| Scientific soundness | Is my outcome definition one a clinician would accept? Am I only using information available before the outcome? Are my assumptions stated?    |
| Integration strategy | Why did I align the sources the way I did? How sensitive are my results to that choice? Where did I decide not to align, and why?              |
| Validation           | Did I validate on survivors my model never saw? Do I beat the MEPS-only baseline, and by more than chance? Are my risk estimates calibrated?   |
| Clinical insight     | Do the factors associated with risk make sense given what is known about survivorship? Am I careful not to claim that associations are causes? |
| Fairness             | Is performance consistent across age, sex, race and ethnicity, insurance, and cancer type? Am I honest about subgroups too small to judge?     |
| Reproducibility      | Is my workflow clearly explained, from data to results? Could someone else rerun it and get the same results?                                  |

Predictions for the held-out survivors are scored for discrimination, calibration, and subgroup consistency. The held-out set has about 57 survivors with the outcome, so modest score differences between teams are within chance, and the judges weigh the other aspects accordingly.

## A few things that often go wrong

The most common mistake with this data is treating the two sources as if they describe the same people. A join on row order, or on a handful of shared variables without checking how well they match, produces a dataset that looks complete but is mostly noise. Report how many records you aligned, how confident each alignment is, and how your results change if you align differently.

The populations also differ. MEPS is a national sample in which few survivors are Hispanic; the Synthea population resembles South Florida, where about half are. An alignment that ignores this will quietly mismatch people.

It is easy to treat MEPS codes as numbers. Negative values are missing-data codes, not measurements, and averaging them in will distort everything downstream. Likewise, a missing cancer stage in Synthea is missing, not "no cancer stage."

Be careful with time. The 2018 file includes spending and health measures from the same year as the outcome; using them as predictors makes results look far better than they are.

Finally, always compare against the MEPS-only baseline, and report uncertainty around your scores. If your integration does not improve on the baseline, that is a legitimate finding worth explaining, not a failure.