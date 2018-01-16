Modeling Hospital Readmissions
================
Mahim Naveed

Modeling readmission among patients at a medical facility
=========================================================

Summary
-------

The goal of the study is to perform an exploratory analysis on the data provided by a home health provider medical facility. The dataset consists of binary, categorical, and ordinal variables and after performing summary statistics it appears that there are some duplicate recors but there are no missing values. After performing some visualizations on the data, a statistical analysis is conducted. For the purpose of this report, a binary variable indicating readmissions is created and is referred to as the response variable in the logistic regression model.

Data Description
----------------

The dataset consists of information on patients who acquired home health care services during the years 2014 − 2016. The raw data set consisted of 37452 observations in the home-care excel sheet and 15397 in the hospitalization records sheet. After removal of duplicate rows and merging the datasets by patient IDs, the observation count came down to 40390. Among these, 40.2% were male and 59.8% female. Total number of patients was 37421, out of which which 45.5% were admitted to hospital and 7.4% of these patients got readmitted to hospital during the course of treatment. The table below summarizes some information from the data:

|                     | First               | Last                | Years            | Patient.Count.per.Year |
|---------------------|:--------------------|:--------------------|:-----------------|:-----------------------|
| Home-care Admission | 2013-12-31 19:00:00 | 2014-12-30 19:00:00 | 2014             | 37421                  |
| Hospitalization     | 2014-01-01 19:00:00 | 2015-08-11 20:00:00 | 2014, 2015       | 11739, 801             |
| Discharge           | 2014-01-01 19:00:00 | 2016-01-25 19:00:00 | 2014, 2015, 2016 | 33311, 4109, 1         |

All home health-care admissions occured in 2013. 93.6% of the patients got admitted to hospital in the 2014 and the remaining 6.4% in 2015. 89% of the patients were discharged from the program in 2014, 10.9% in 2015 and only 1 patient was discharged in 2016.

Exploratory Analysis
--------------------

![](HospReadmissions_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-2-1.png)![](HospReadmissions_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-2-2.png)

![](HospReadmissions_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-3-1.png)![](HospReadmissions_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-3-2.png)

The plots above provide some insight into the data. It appears that even though there were more female patients than male, the age distribution was very similar for both the genders, with an average age of about 72 years. The lowest reported age was 28 years and the highest 123. Since it is not known whether the data is real or simulated, high aged records were not removed from the dataset. Like age, dyspnea severity indices reported for both genders were also similar; the second plot provides percentages for comparison. Among different diagnosis categories, hypertension was common among most patients, followed by diabetes and then arthritis. The average duration of treatment was about 41 days, where the longest treatment lasted 437 days.

Statistical Analysis
--------------------

After performing some exploratory visualizations on the data, I decided to model readmissions among the patients. The plots below show that as the duration of treatment increased, readmissions to the hospital decreased. It is also evident that readmissions among female patients decreased faster than among male patients.

![](HospReadmissions_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-4-1.png)

In order to evaluate the factors predictive of readmission to the hospital, logistic regression is applied, given binary outcome (readmission). Before proceeding to data modeling, let's take a look at correlation between predictor variables. The correlogram below displays positive correlations in blue and negative correlations in red color. Color intensity and the size of the circle are proportional to the correlation coefficients.

None of the diagnosis categories appear to be correlated except for diabetes and hypertension. There are no highly correlated predictors so it is safe to include all the variables. If the number of predictors was very large, variance inflation factor could be used to select suitable variables for analysis.

Since correlation between diabetes and hypertension is very little, for the purpose of this report, an assumption of non-multicollinearity is made.

![](HospReadmissions_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-5-1.png)

Assuming response outcomes (hospital readmission) are binary, the independent variables are not linear combinations of each other, and cases are independent, the following logistic model is set up:

*P**r**o**b**a**b**i**l**i**t**y* *d**i**s**t**r**i**b**u**t**i**o**n* :      *y*<sub>*i*</sub> ∼ *B**e**r**n*(*p*<sub>*i*</sub>)
*M**o**d**e**l* :     *l**o**g**i**t*(*p*<sub>*i*</sub>) = *β*<sub>0</sub> + *β*<sub>1</sub>*X*<sub>*g**e**n**d**e**r*</sub> + *β*<sub>2</sub>*X*<sub>*a**g**e*</sub> + *β*<sub>3</sub>*X*<sub>*h**y**p*</sub> + *β*<sub>4</sub>*X*<sub>*d**i**a**b*</sub> + *β*<sub>5</sub>*X*<sub>*a**r**t**h*</sub> + *β*<sub>6</sub>*X*<sub>*c**h**f*</sub> + *β*<sub>7</sub>*X*<sub>*p**d*</sub>+
*β*<sub>8</sub>*X*<sub>*a**m**i*</sub> + *β*<sub>9</sub>*X*<sub>*s**t**r*</sub> + *β*<sub>10</sub>*X*<sub>*d**y**s**p*</sub> + *β*<sub>11</sub>*X*<sub>*m**e**d*</sub> + *β*<sub>12</sub>*X*<sub>*d**a**y**s*</sub>,

$$Link \\ function: \\ \\ \\ \\ \\ g(E\[Y|X\]) = g(p) = logit(p) = log\\frac{p}{1-p}.$$

The performance of this model was compared with a 2-way interaction model (keeping gender as interaction term), but the AIC value was lower for the model without interactions, hence the above mentioned model was used. The odds ratios resulting from the model are acquired by exponentiating the beta estimates and are as follows:

| Resp    | Int  | Male | Age   | Hyp  | Diab  | Arth  | CHF  | PD    | AMI   | Stroke | Dysp Sev | Med Ct | Days Tr |
|---------|------|------|-------|------|-------|-------|------|-------|-------|--------|----------|--------|---------|
| Readmit | 0.03 | 1.56 | 1.003 | 1.41 | 1.453 | 0.816 | 1.90 | 1.178 | 0.942 | 1.289  | 1.063    | 1.028  | 1.00    |
| ------- | ---- | ---- | ----- | ---- | ----- | ----- | ---- | ----- | ----- | -----  | -----    | -----  | -----   |
| p-Value | \*   | \*   |       | \*   | \*    | \*    | \*   | \*    |       | \*     | \*       | \*     |         |

The asterisks denote that the value is less than 0.05. From the results above we can say that according to p-values set at a significance level of 0.05, age, acute myocardial infarction and the number of days under treatment were insignificant predictors of readmission.

Male patients were 56% more likely to get readmitted than female. Patients who had hypertension were 41% more likely to get readmitted than than those who did not have hypertension. Patients who were diabetic were 45.3% more likely to get readmitted than than those who were not diabetic. Patients who had arthritis were 18.4% less likely to get readmitted than those who did not have arthritis. Patients who had a pulmonary disease diagnosis were 17.9% more likely to get readmitted than those who did not have it. Having a congestive heart failure history increased the odds of getting readmitted by 90%. Patients who had reported a stroke incident were 28.9% more likely to get readmitted than those who had not. For each level increase in dyspnea index, the odds of readmission increased by 6.3%. For a unit increase in number of present precriptions, the odds of getting readmitted increased by 2.8%.

Conclusion
----------

Different approaches can be applied to a data set of this sort. I decided to analyze readmissions by creating a binary indicator variable for every patient who got hospitalized more than once. This helped evaluate factors that could lead to getting readmitted. In camparison to all other predictors, having a history of congestive heart failure increased the odds of getting readmitted the most.

Appendix
========

Descriptive statistics
----------------------

### Binary predictors

**Hypertension**

    ##   Gender Status  Freq Percent
    ## 1   Male      0 11465 28.39 %
    ## 2 Female      0 16620 41.15 %
    ## 3   Male      1  5088  12.6 %
    ## 4 Female      1  7217 17.87 %

**Diabetes**

    ##   Gender Status  Freq Percent
    ## 1   Male      0 13189 32.65 %
    ## 2 Female      0 18862  46.7 %
    ## 3   Male      1  3364  8.33 %
    ## 4 Female      1  4975 12.32 %

**Arthritis**

    ##   Gender Status  Freq Percent
    ## 1   Male      0 14005 34.67 %
    ## 2 Female      0 20203 50.02 %
    ## 3   Male      1  2548  6.31 %
    ## 4 Female      1  3634     9 %

**Congestive Heart Failure**

    ##   Gender Status  Freq Percent
    ## 1   Male      0 14316 35.44 %
    ## 2 Female      0 20612 51.03 %
    ## 3   Male      1  2237  5.54 %
    ## 4 Female      1  3225  7.98 %

**Acute Myocardial Infarction**

    ##   Gender Status  Freq Percent
    ## 1   Male      0 14624 36.21 %
    ## 2 Female      0 21185 52.45 %
    ## 3   Male      1  1929  4.78 %
    ## 4 Female      1  2652  6.57 %

**Stroke**

    ##   Gender Status  Freq Percent
    ## 1   Male      0 15123 37.44 %
    ## 2 Female      0 21886 54.19 %
    ## 3   Male      1  1430  3.54 %
    ## 4 Female      1  1951  4.83 %

**Hospitalized**

    ##   Gender Status  Freq Percent
    ## 1   Male      0  9545 23.63 %
    ## 2 Female      0 15453 38.26 %
    ## 3   Male      1  7008 17.35 %
    ## 4 Female      1  8384 20.76 %

**Readmitted**

    ##   Gender Status  Freq Percent
    ## 1   Male      0 15038 37.23 %
    ## 2 Female      0 22383 55.42 %
    ## 3   Male      1  1515  3.75 %
    ## 4 Female      1  1454   3.6 %

### Continuous predictors

**Age**

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   28.00   64.00   72.00   72.03   80.00  123.00

**Days under treatment**

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##     1.0    12.0    29.0    40.8    56.0   437.0

**Days to hospitalization**

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    1.00    4.00   11.00   20.77   27.00  346.00   24998

*NA's account for the number of patients who did not get hospitalized.*

Logistic model results
----------------------

    ##                               Estimate      Pr...z.. Odds.Ratio
    ## (Intercept)                     -3.525 9.633784e-137      0.029
    ## GenderMale                       0.445  5.148881e-31      1.561
    ## Age                              0.003  9.013988e-02      1.003
    ## Hypertension                     0.341  5.390264e-17      1.407
    ## Diabetes                         0.374  3.555791e-17      1.453
    ## Arthritis                       -0.203  3.402532e-04      0.816
    ## `Congestive Heart Failure`       0.642  1.049644e-41      1.899
    ## `Pulmonary Disease`              0.164  3.607494e-03      1.179
    ## `Acute Myocardial Infarction`   -0.060  3.292860e-01      0.942
    ## Stroke                           0.254  6.815325e-05      1.289
    ## `Dyspnea Severity`               0.061  6.247400e-03      1.063
    ## `Medication Count`               0.028  4.045996e-05      1.028
    ## `Days under treatment`           0.000  6.637374e-01      1.000
