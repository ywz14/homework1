# MA \[46\]15 Homework 1
Yawen Zhang

In this homework I’m analyzing a small subset of the IPUMS dataset. I
start by loading the packages and the data.

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ✔ purrr     1.0.2     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggthemes)
load("ipums_hw_data.RData")
ipums_hw_data
```

    # A tibble: 2,798 × 8
        YEAR  SERIAL  PERWT HHINCOME RACE  RACED POVERTY SPMPOV        
       <int>   <dbl>  <dbl>    <dbl> <fct> <fct>   <dbl> <fct>         
     1  2013  782791  5215.    30000 White White     127 In poverty    
     2  2014 1045362 12573    145000 White White     501 Not in poverty
     3  2013  436423 11906.   185204 White White     501 Not in poverty
     4  2010 1183930  8089.   133000 White White     501 Not in poverty
     5  2016  976108  8428.   165000 White White     501 Not in poverty
     6  2011  556094  9900     92000 White White     407 In poverty    
     7  2016 1214893  7424.   120000 White White     422 Not in poverty
     8  2013  675831  8167.    92700 White White     299 Not in poverty
     9  2012   29649  6401.   122000 White White     443 Not in poverty
    10  2018 1373978  4001.    76000 White White     501 Not in poverty
    # ℹ 2,788 more rows

Descriptions for the variables in the data set are given below.

| Variable | Description                                                                                                                                                                                                                                                                                             |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| YEAR     | reports the four-digit year when the household was enumerated or included in the census, the ACS, and the PRCS.                                                                                                                                                                                         |
| SERIAL   | An identifying number unique to each household record in a given sample.                                                                                                                                                                                                                                |
| HHINCOME | Reports the total money income of all household members age 15+ during the previous year.                                                                                                                                                                                                               |
| PERWT    | Indicates how many persons in the U.S. population are represented by a given person in an IPUMS sample.                                                                                                                                                                                                 |
| RACE     | Simple version of race. The concept of race has changed over the more than 150 years represented in IPUMS. Currently, the Census Bureau and others consider race to be a sociopolitical construct, not a scientific or anthropological one. Many detailed categories consist of national origin groups. |
| RACED    | Detailed version of race.                                                                                                                                                                                                                                                                               |
| POVERTY  | Expresses each family’s total income for the previous year as a percentage of the poverty thresholds established by the Social Security Administration.                                                                                                                                                 |
| SPMPOV   | Indicates whether a family is classified as poor according to the Supplemental Poverty Measure (SPM).                                                                                                                                                                                                   |

The number of observations in the data set is less than 0.01% of the
full IPUMS data for the relevant years. This data was collected from
[IPUMS USA](https://usa.ipums.org/) and was cleaned and reduced in size
to improve ease-of-use. You can search for individual variables
[here](https://usa.ipums.org/usa-action/variables/search).

## Question 1

``` r
library(ggplot2)
ggplot(ipums_hw_data) + 
  geom_point(aes(x = HHINCOME, y = POVERTY, color = RACE), alpha = 0.25) + 
  scale_x_log10() + 
  scale_y_log10() +
  scale_color_colorblind() +
  labs(
    x = "HHINCOME",
    y = "POVERTY"
  ) +
  theme_minimal()
```

    Warning: Removed 3 rows containing missing values or values outside the scale range
    (`geom_point()`).

![](hw1_files/figure-commonmark/q1-1.png)

\##the relationship between these two variables: as household income
increases, povety also increases. higher income is associated with
higher poverty##

## Question 2

``` r
ggplot(ipums_hw_data, aes(x = HHINCOME, y = RACE, fill = RACE)) +
  geom_boxplot(varwidth = TRUE) +
  scale_fill_manual(values = c("black","blue", "yellow", "green", "pink", "red", "purple", "orange")) +
  theme(legend.position = "none")
```

![](hw1_files/figure-commonmark/q2-1.png)

## Question 3

``` r
ggplot(ipums_hw_data, aes(x = RACE, fill = SPMPOV, weight = PERWT)) +
  geom_bar(position = "fill") +
  scale_y_continuous(labels = scales::percent) +
  labs(y = "Proportion", x = "Race") +
  theme(axis.text.x = element_text(angle = 15, hjust = 1),
        legend.title = element_blank())
```

![](hw1_files/figure-commonmark/q3-1.png)

## Question 4

``` r
ggplot(ipums_hw_data, aes(x = RACE, y = HHINCOME)) +
  stat_summary(fun = "mean", geom = "point" ) +
  labs() +
  theme_minimal()
```

![](hw1_files/figure-commonmark/q4-1.png)

\##difference: the graph above studies the home income, the graph on the
website focuses on poverty rate. \##issue: the x-axis is hard to read.##
