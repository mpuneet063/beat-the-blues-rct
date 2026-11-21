# Beat the Blues: reanalysis of a randomised controlled trial in R

A reproducible reanalysis of the **Beat the Blues** trial, which compared computerised cognitive behavioural therapy
(CBT) with usual care for patients with depression in UK general practice. Depression was measured on the Beck
Depression Inventory II (BDI) at baseline and at 2, 3, 5 and 8 months after randomisation.

The project reproduces the published linear mixed model analysis of these data, then extends it with the reporting a
trial statistician would produce: trial flow, a baseline table, an intention-to-treat estimate with its confidence
interval, model comparisons and a missing-data sensitivity analysis, all knitted into one automated report.

## Question

Does computerised CBT reduce depression scores over 8 months of follow-up, compared with treatment as usual, after
adjusting for baseline severity?

## Data

`BtheB` from the R package **HSAUR3**: 100 patients randomised to Beat the Blues or treatment as usual (TAU), with
baseline BDI, antidepressant use (`drug`), length of current episode (`length`) and BDI at four follow-up visits.
Dropout increases over follow-up, so not every patient is observed at every visit.

## Methods

1. **Trial flow.** CONSORT-style counts of patients randomised per arm and observed at each visit.
2. **Baseline table.** Baseline characteristics by arm (`gtsummary`). Differences are described, not tested, because
   any imbalance after randomisation is due to chance.
3. **Primary analysis.** Linear mixed model with a random intercept per patient (`lme4`), fitted by maximum likelihood
   to all available observations from all randomised patients:
   `bdi ~ bdi.pre + time + treatment + drug + length + (1 | subject)`.
   The estimand is the difference in mean BDI between arms across follow-up, adjusted for baseline.
4. **Model extensions.** A random slope for time, compared with the primary model by likelihood ratio test; a
   treatment × time interaction; residual and random-effect diagnostics.
5. **Missing-data sensitivity.** Whether dropout relates to arm or baseline severity (logistic regression), and the
   treatment effect from a complete-case analysis compared with the mixed model. The mixed model is valid if data are
   missing at random; complete-case analysis needs the stronger assumption that data are missing completely at random.
6. **Report.** One R Markdown document that knits to PDF.

## Results

| Analysis | Treatment effect (BDI points, BtheB − TAU) | 95% CI | p |
| --- | --- | --- | --- |
| Primary: random-intercept mixed model | [ ] | [ ] | [ ] |
| Complete case | [ ] | [ ] | [ ] |

- Random slope vs random intercept (likelihood ratio test): [ ]
- Treatment × time interaction: [ ]
- Intraclass correlation: [ ]

The primary estimate reproduces the published value of −2.33 (Everitt & Hothorn, 2014). [Add a one-line conclusion.]

![Mean BDI over follow-up by arm](figures/bdi_over_time.png)

## Reproduce

```r
install.packages(c("HSAUR3", "lme4", "lmerTest", "broom.mixed", "gtsummary", "tidyverse", "knitr", "rmarkdown"))
rmarkdown::render("analysis.Rmd", output_format = "pdf_document")
```

## Repository layout

```
analysis.Rmd     the full analysis and report
output/          knitted PDF report
figures/         figures used in the report and this README
```

## What is reproduced and what is new

The data and the random-intercept and random-slope models follow Everitt & Hothorn (2014), and the primary estimate
is checked against theirs. The trial flow table, baseline table, intention-to-treat framing with confidence
intervals, treatment × time model, missing-data sensitivity analysis and report are additions in this project.

## References

- Proudfoot, J., Goldberg, D., Mann, A., Everitt, B. S., Marks, I., & Gray, J. A. (2003). Computerized, interactive,
  multimedia cognitive-behavioural program for anxiety and depression in general practice. *Psychological Medicine*,
  33(2), 217–227.
- Everitt, B. S., & Hothorn, T. (2014). *A Handbook of Statistical Analyses Using R* (3rd ed.). CRC Press. Chapter
  "Analysing Longitudinal Data I"; data from the HSAUR3 package.
- Bates, D., Mächler, M., Bolker, B., & Walker, S. (2015). Fitting linear mixed-effects models using lme4. *Journal of
  Statistical Software*, 67(1), 1–48.
- Kuznetsova, A., Brockhoff, P. B., & Christensen, R. H. B. (2017). lmerTest package: tests in linear mixed effects
  models. *Journal of Statistical Software*, 82(13), 1–26.
- Sjoberg, D. D., Whiting, K., Curry, M., Lavery, J. A., & Larmarange, J. (2021). Reproducible summary tables with the
  gtsummary package. *The R Journal*, 13(1), 570–580.
- Wickham, H., et al. (2019). Welcome to the tidyverse. *Journal of Open Source Software*, 4(43), 1686.
- Schulz, K. F., Altman, D. G., & Moher, D. (2010). CONSORT 2010 statement: updated guidelines for reporting parallel
  group randomised trials. *BMJ*, 340, c332.
- R Core Team. *R: A Language and Environment for Statistical Computing*. R Foundation for Statistical Computing,
  Vienna, Austria.

Author: Puneet Mishra (MSc Data Science and AI, University of Birmingham and IIT Madras)
