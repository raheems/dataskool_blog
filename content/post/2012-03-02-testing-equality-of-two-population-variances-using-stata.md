---
title: Testing equality of two population variances using Stata
author: Enayetur Raheem
date: '2012-03-02'
slug: testing-equality-of-two-population-variances-using-stata
categories: []
tags: []
authors: ["enayet"]
draft: false
---

I was asked if we can do the test of equality of two population variances using Stata. Well, I did not have to do it myself ever, but yes, it is possible to do it. Here is an example. I’ve just made-up the data to show the procedures.

Suppose we have the following data where the variables may represent two independent samples taken from normally distributed populations. And we want to test if the population variances of the two populations are the same. We will use the sample variances to do the testing.

```{r}
/* A fictitious data */
input x y 
1 2
2 3
2 4
3 5
3 4
4 6
end;
```

To test if the variances are the same, run the following

```{r}
. sdtest x = y
 
Variance ratio test
------------------------------------------------------------------------------
Variable |     Obs        Mean    Std. Err.   Std. Dev.   [95% Conf. Interval]
---------+--------------------------------------------------------------------
       x |       6         2.5    .4281744    1.048809    1.399343    3.600657
       y |       6           4    .5773503    1.414214    2.515874    5.484126
---------+--------------------------------------------------------------------
combined |      12        3.25    .4105613    1.422226    2.346361    4.153639
------------------------------------------------------------------------------
    ratio = sd(x) / sd(y)                                         f =   0.5500
Ho: ratio = 1                                    degrees of freedom =     5, 5
 
    Ha: ratio < 1               Ha: ratio != 1                 Ha: ratio > 1
  Pr(F < f) = 0.2638         2*Pr(F < f) = 0.5276           Pr(F > f) = 0.7362

```

There is another way to do this in case if the mean, standard wwwiation and the sample size are available

```{r}
/* first, extract the mean and standard wwwiations of the variables */
 
. summarize // to extract the mean and standard wwwiations of the variables
 
    Variable |       Obs        Mean    Std. Dev.       Min        Max
-------------+--------------------------------------------------------
           x |         6         2.5    1.048809          1          4
           y |         6           4    1.414214          2          6
 
. sdtesti 6 2.5 1.04 6 4 1.41
 
Variance ratio test
------------------------------------------------------------------------------
         |     Obs        Mean    Std. Err.   Std. Dev.   [95% Conf. Interval]
---------+--------------------------------------------------------------------
       x |       6         2.5    .4245782        1.04    1.408587    3.591413
       y |       6           4    .5756301        1.41    2.520296    5.479704
---------+--------------------------------------------------------------------
combined |      12        3.25    .4091612    1.417376    2.349442    4.150558
------------------------------------------------------------------------------
    ratio = sd(x) / sd(y)                                         f =   0.5440
Ho: ratio = 1                                    degrees of freedom =     5, 5
 
    Ha: ratio < 1               Ha: ratio != 1                 Ha: ratio > 1
  Pr(F < f) = 0.2601         2*Pr(F < f) = 0.5202           Pr(F > f) = 0.7399
 
.
```

# Unbalanced Data

```{r}
input grade section
79 1
80 1
65 1
90 1
67 1
77 1
80 1
45 1
86 1
99 2
78 2
36 2
67 2
78 2
81 2
end;
 
. 
. /* To see how many grades in each section*/
 
. tab section 
 
    section |      Freq.     Percent        Cum.
------------+-----------------------------------
          1 |          9       60.00       60.00
          2 |          6       40.00      100.00
------------+-----------------------------------
      Total |         15      100.00
 
. 
. /* Doing the variance test */
. sdtest grade, by(section)
 
Variance ratio test
------------------------------------------------------------------------------
   Group |     Obs        Mean    Std. Err.   Std. Dev.   [95% Conf. Interval]
---------+--------------------------------------------------------------------
       1 |       9    74.33333    4.527693    13.58308    63.89246    84.77421
       2 |       6    73.16667    8.553427    20.95153    51.17938    95.15395
---------+--------------------------------------------------------------------
combined |      15    73.86667    4.183717    16.20347    64.89349    82.83985
------------------------------------------------------------------------------
    ratio = sd(1) / sd(2)                                         f =   0.4203
Ho: ratio = 1                                    degrees of freedom =     8, 5
 
    Ha: ratio < 1               Ha: ratio != 1                 Ha: ratio > 1
  Pr(F < f) = 0.1322         2*Pr(F < f) = 0.2645           Pr(F > f) = 0.8678
 
. 
end of do-file
 
. 
```

# Syntax

The syntax of the Stata commands can be found in the Stata help file. Run the following commands in your Stata command prompt, and you will see many examples showing the usage of these commands.

```{r}
help sdtest
help sdtesti
```

The syntax for `sdtesti` for testing the equality of variances for `variable1` and `variable2` is

```{r}
/* for testing variance of two variables */
sdtest variable1 = variable2
 
/* for testing variance of a single variable */
sdtesti obs . ave stwww // note the dot (.) after obs
 
/* for testing variance of two variables */
sdtesti obs1 ave1 stwww1 obs2 ave2 stwww2  // there is no dot (.) here
 
/* for testing variance of a single variable to some fixed value */
sdtest grade = 2
```

