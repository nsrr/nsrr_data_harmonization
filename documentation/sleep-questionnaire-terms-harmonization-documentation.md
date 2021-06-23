Harmonization of Sleep Questionnaire Terms
================

## Introduction

This markdown file documents what sleep questionnaire terms were
selected for harmonization, how related variables were mapped in each
data set, and what decisions were made to harmonize the variables.

## Sleep Duration

Self-reported sleep duration is a commonly used term in various sleep
research, which is also highly heterogeneous in terms of how the
questions were asked in the survey, interpreted and answered by the
participants. We decomposed the concept of **self-reported sleep
duration** into **habitual sleep** and **calculated sleep** based
whether the participant was asked to estimate the average length of
sleep - therefore habitual sleep, or to report the in-bed and out-bed
times - therefore calculated sleep based on two time points. To preserve
the meaningful differences in terms, we further introduced modifiers to
distinguish 24 hour vs nocturnal/main sleep, weekdays vs weekends,
in-bed period vs sleep period.

### Self-reported 24 hour habitual sleep averaged over 7 days

| Harmonized Term | AvgDurSp\_ib\_w7\_svy                                                                                                                                                                                                                             |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description     | This term is based on self-reported length of actual sleep including naps averaged over 7 days. The coding **DurSp** implies actual sleep duration which subtracts wake-in-bed (including waso) from the interval between sleep onset and offset. |
| Keywords        | hours of sleep do you get, including naps, 24-hour period, actual sleep, etc.                                                                                                                                                                     |
| Calculation     | Both raw and calculated variables can be included. The calculation only includes averaging over weekdays and weekends                                                                                                                             |
| Coverage        | wsc(all visits, cfs(visit 5)                                                                                                                                                                                                                      |
| Decisions       | 1\. Convert variables in unit of hour to minute by multiplying 60.<br> 2.Convert categorical variables to numeric variables by taking the mean value of each category and the minimum value of the top category.                                  |

``` r
if (data$dataset=="cfs") {
  data<-data%>%
    dplyr::mutate(AvgDurSp_ib_w7_svy=case_when(
      tothrslp==0 ~ 2.5*60,
      tothrslp==1 ~ (6+6.9)*60/2,
      tothrslp==2 ~ (7+7.9)*60/2,
      tothrslp==3 ~ (8+8.9)*60/2,
      tothrslp==4 ~ (9+9.9)*60/2,
      tothrslp==5 ~ (10+10.9)*60/2,
      tothrslp==6 ~ 11*60,
      TRUE ~ NA_real_
    ))
}
if (data$dataset=="wsc") {
  data<-data%>%
    dplyr::mutate(AvgDurSp_ib_w7_svy=totsleepnap*60)
} 
```

### Self-reported nocturnal/main habitual sleep

#### Average over 7 days

| Harmonized Term | AvgDurSp\_im\_w7\_svy                                                                                                                                                                                                                                                     |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description     | This term is based on self-reported length of actual sleep excluding naps (or naps not specified) averaged over 7 days. The coding **DurSp** implies actual sleep duration which subtracts wake-in-bed (including waso) from the interval between sleep onset and offset. |
| Keywords        | hours of sleep do you get, excluding naps, at night, in main sleep,or naps not mentioned in the question.                                                                                                                                                                 |
| Calculation     | Both raw and calculated variables can be included. The calculation only includes averaging over weekdays and weekends                                                                                                                                                     |
| Coverage        | WSC (all visits), MrOS (all visits), CCSHS(TREC), CFS                                                                                                                                                                                                                     |
| Decisions       | 1\. Convert variables in unit of hour to minute by multiplying 60.<br> 2.Derive new harmonized terms if components are available in the dataset.                                                                                                                          |

``` r
if (data$dataset=="cfs") {
  data<-data%>%
    dplyr::mutate(AvgDurSp_im_w7_svy=((dayhrs*60+daymin)*5+(endhrs*60+endmin)*2)/7)
}
if (data$dataset=="shhs1") {
  data<-data%>%
    dplyr::mutate(AvgDurSp_ib_w7_svy=(hrswe02*2+hrswd02*5)*60/7)
} 
if (data$dataset=="shhs2") {
  data<-data%>%
    dplyr::mutate(AvgDurSp_ib_w7_svy=(totminslpwe*2+totminslpwd*5)*60/7)
}
if (data$dataset=="ccshs") {
  data<-data%>%
    dplyr::mutate(AvgDurSp_ib_w7_svy=ysleeptime_s*60)
}
if (data$dataset=="mros") {
  data<-data%>%
    dplyr::mutate(AvgDurSp_ib_w7_svy=pqpactsl*60)
}
if (data$dataset=="mros") {
  data<-data%>%
    dplyr::mutate(AvgDurSp_ib_w7_svy=totsleep*60)
}
```

#### Average over weekdays

#### Average over weekends
