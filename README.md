# Diagnosing Underpowered Studies and P-Hacking

## Objective

I used simulated data to show how low statistical power and unadjusted multiple testing lead to wrong conclusions, and how power analysis, multiple-testing correction and robust statistics fix them.

## Methodology

- **Underpowered pilot:** I audited an analyst's "no effect" conclusion from a 15-per-group pilot. I computed the observed effect size and the pilot's achieved power against the true effect, found the sample size needed for 80% power, and reran the study at that size from the same population.
- **P-hacked subgroup:** I audited a researcher's claim of a significant subgroup out of 20 tested. I applied a Benjamini-Hochberg (BH) correction to the 20 p-values and calculated the chance of at least one false positive across 20 independent tests of true nulls.
- **Simulation:** I ran 1,000 replications of the 20 tests with every null true, and compared the share with at least one p < 0.05 to the theoretical value.
- **Own BH function:** I wrote my own BH function, checked it against `statsmodels`, and applied it to a version of the data with two real effects planted.
- **Robust alternatives:** I compared four tests on heavy-tailed, outlier-contaminated earnings data: Welch's t-test, Yuen's trimmed t-test, and permutation tests on the difference in means and on the 20% trimmed mean.
- **Challenge (optional):** I simulated 5,000 studies' z-statistics, pushed the borderline ones across the 1.96 line to mimic p-hacking, and compared the clean and hacked distributions (Brodeur's "camel").

## Key Findings

| Analysis | Result |
|---|---|
| Pilot power (15 per group) | 10.9% against the true effect |
| Sample size for 80% power | 222 per group |
| Rerun at 222 per group | p = 0.0012 |
| Subgroups significant after BH | 0 of 20 |
| False positives in 20 true-null tests (simulated) | 65.8% of 1,000 replications |
| False positives in 20 true-null tests (theory) | 64.2% |
| My BH function with two planted effects | flagged 2 subgroups |
| Share significant, clean vs. p-hacked | 24.2% vs. 31.3% |

- The pilot's "no evidence" result was uninformative, not evidence of no effect. With 10.9% power, it would miss a real effect most of the time.
- The "significant" Subgroup 12 was what chance produces when you test 20 times and report the best one.
- My BH function matched `statsmodels` on the original data.
- On the earnings data, the four tests split by the statistic they tested, not by parametric versus permutation. The two tests comparing plain means agreed with each other, and the two comparing trimmed means agreed with each other and disagreed with the first pair.
- P-hacking the borderline studies raised the share of significant results from 24.2% to 31.3%.
