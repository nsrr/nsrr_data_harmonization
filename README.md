An introduction to the NSRR harmonized variable documentation
================
Ying Zhang
15 November, 2022

This repository contains documentation for the phenotype, sleep
monitoring (e.g. polysomnography, polygraphy, actigraphy) and survey
questionnaire data harmonized by the [National Sleep Research Resource
(NSRR)](https://www.sleepdata.org/), including harmonization standards,
rationale, and script for recreating the harmonized data.

## Repository contents

| file                   | description                                                                                        |
|------------------------|----------------------------------------------------------------------------------------------------|
| `non-sleep-phenotype/` | A directory containing documentations for harmonized non-sleep covariates.                         |
| `sleep-monitoring/`    | A directory containing documentations and script for creating harmonized sleep monitoring data.    |
| `sleep-questionnaire/` | A directory containing documentations and script for creating harmonized sleep questionnaire data. |
| `README.md`            | This document.                                                                                     |
| `README.Rmd`           | Source for this document.                                                                          |

**Table of Contents**

- [Overview](#overview)

- [Non-Sleep Phenotype Data](#non-sleep-phenotype-data)

- [Polysomnography and Polygraphy
  Data](#polysomnography-and-polygraphy-data)

- [Sleep Questionnaire Data](#sleep-questionnaire-data)

- [Resources](#resources)

------------------------------------------------------------------------

## Overview

The objectives for data harmonization is (1) to identify core sleep
terms that are semantically similar, (2) to assess the heterogeneity in
the study-level and variable-level metadata, (3) to improve
comparability of the harmonized terms, and (4) to make harmonized terms
inferentially equivalent when possible. We identified three types of
data currently shared on the NSRR website that are most likely to be
valuable to our users once harmonized - commonly used non-sleep
covariates, sleep monitoring data (e.g.,polysomnography/polygraphy) and
sleep questionnaire data. We’ve developed domain-specific approach to
address the challenges in harmonizing each of these types of data.
Harmonized data were created using SAS and saved as a separate data file
in each dataset on NSRR website. All SAS scripts are publicly available
on [github.com/nsrr](https://github.com/nsrr).

## Non-Sleep Phenotype Data

[TOPMed DCC’s harmonized
phenotype](https://github.com/UW-GAC/topmed-dcc-harmonized-phenotypes)
and [BioDataCatalyst Common Data
Model](https://github.com/uc-cdis/gtex-dictionary/tree/master/gdcdictionary/schemas/)
were identified as the harmonization standards for the non-sleep
phenotype data. We have harmonized 9 non-sleep phenotype variables:
age(`nsrr_age`), sex(`nsrr_sex`), race(`nsrr_race`),
ethnicity(`nsrr_ethnicity`), body mass index(`nsrr_bmi`), current
smoker(`nsrr_current_smoker`), ever smoker(`nsrr_ever_smoker`), resting
systolic(`nsrr_bp_systolic`) and diastolic blood
pressure(`nsrr_bp_diastolic`).

### Data format

The original data format was preserved for numeric variables;
Categorical variables (e.g., sex, race) were recoded as string. All
permissible values for the harmonized categorical variables are
available as JSON files in `non-sleep-phenotype/`.

### Missing value

For numerical variables, missing values were represented as empty cell
value in the date files. For categorical variables, explicit missing
codes were used to comply with the TopMED/BioDataCatalyst harmonization
standards. Unspecific missing was coded as “not reported” for most
categorical variables.

## Polysomnography and Polygraphy Data

Retrospective polysomnography/Polygraph (PSG) data harmonization focuses
on summary PSG data submitted by the data owners, which was either
manually scored by scorers or automatically scored by the device.
Prospective PSG data harmonization using EDF files can be done by the
NSRR [NAP pipeline](https://zzz.bwh.harvard.edu/luna/nap/). We’ve
identified 5 PSG terms to harmonize: AHI3%, AHI4%, total sleep duration,
overall arousal index and sleep/wake signal quality flag.

### Apnea-hypopnea Indices

After carefully evaluating the main sources of heterogeneity in PSG data
generation and processing across datasets, we’ve identified four varying
components in the metadata - sleep test type (i.e., type I or II
polysomnography, type III or IV polygraphy/home sleep apnea test), flow
reduction, oxygen desaturation, and arousal. Here is a diagram for how
commonly referred to as AHI3% and AHI4% can vary in the these categories
and create many more permutations.

![](README_files/figure-gfm/unnamed-chunk-1-1.png)<!-- --> Among the 13
permutations of AHI3% and AHI4% as illustrated in the diagram above,
we’ve mapped a subset of them to the American Academy of Sleep Medicine
(AASM) Clinical Guidelines and consolidated into 10 harmonized
variables - 7 AHIs and 3 REIs.

| Year | Version     | Definition for Hypopnea Events                                                                     | Event Duration | Reduction Duration Criteria  | Consolidated Coding |
|------|-------------|----------------------------------------------------------------------------------------------------|----------------|------------------------------|---------------------|
| 1999 |             | \>50% nasal cannula reduction or discernible nasal cannula reduction with (\>=3% desat or arousal) | \>=10 sec      |                              | chicago1999         |
| 2007 | Recommended | \>=30% nasal cannula \[or alternative sensor\] reduction with \>=4% desat                          | \>=10 sec      | \>=90% of the event duration | same as hp4u_aasm15 |
| 2007 | Alternative | \>=50% nasal cannula \[or alternative sensor\] reduction with (\>=3% desat or arousal)             | \>=10 sec      | \>=90% of the event duration | hp3r_aasm07         |
| 2012 | Recommended | \>=30% nasal cannula \[or alternative sensor\] reduction with (\>=3% desat or arousal)             | \>=10 sec      | \>=10 sec                    | same as hp3r_aasm15 |
| 2012 | Alternative | \>=30% nasal cannula \[or alternative sensor\] reduction with \>=4% desat                          | \>=10 sec      | \>=10 sec                    | same as hp4u_aasm15 |
| 2015 | Recommended | \>=30% nasal cannula \[or alternative sensor\] reduction with (\>=3% desat or arousal)             | \>=10 sec      | \>=10 sec                    | hp3r_aasm15         |
| 2015 | Acceptable  | \>=30% nasal cannula \[or alternative sensor\] reduction with (\>=4% desat)                        | \>=10 sec      | \>=10 sec                    | hp4u_aasm15         |

![](README_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

![](README_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Harmonized AHI/REI terms are shown in grey circles in the diagram above,
and can be searched on the NSRR website. Links to the source variables
are available in the metadata.

### Total Sleep Duration

Total sleep duration (i.e., total sleep time) is defined as the interval
between sleep onset and offset while the subject was asleep, which is an
important measure used in calculating various indices. However,
depending on the context and usage, “time” might refer to a specific
point in time or to an interval between two time points,while “duration”
and “period” could both be taken to correspond to intervals, they were
often used interchangeably in protocol descriptions and data
dictionaries without any indication of whether they referred to
intervals between designated time points or to specific intervals when
subjects were determined to be awake or asleep.Therefore, we’ve
developed a set of standardized terms to address the challenges of
“time”, “period”, “duration” being used inconsistently at the NSRR
metadata documentation.

![](README_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

### Overall Arousal Index

### Sleep/Wake Signal Quality Flag

We’ve selected sleep/wake signal quality flag for it’s commonly
misunderstood and misused by the NSRR users. When the EEG is
insufficient in quality to allow distinction of sleep stages, all sleep
stages were scored using a default of “Stage 2” and no arousal was
scored. Studies and individuals with “sleep/wake only” instead of “full
scoring” for `nsrr_flag_spsw` should not be used to analyze sleep
architecture or arousals.

## Sleep Questionnaire Data
