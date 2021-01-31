---
title: "Lab 7 Homework"
author: "Zabrisky Roland"
date: "2021-01-31"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

## Data
**1. For this homework, we will use two different data sets. Please load `amniota` and `amphibio`.**  

`amniota` data:  
Myhrvold N, Baldridge E, Chan B, Sivam D, Freeman DL, Ernest SKM (2015). “An amniote life-history
database to perform comparative analyses with birds, mammals, and reptiles.” _Ecology_, *96*, 3109.
doi: 10.1890/15-0846.1 (URL: https://doi.org/10.1890/15-0846.1).

```r
amniota <- readr::read_csv("data/amniota.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_double(),
##   class = col_character(),
##   order = col_character(),
##   family = col_character(),
##   genus = col_character(),
##   species = col_character(),
##   common_name = col_character()
## )
```

```
## See spec(...) for full column specifications.
```

`amphibio` data:  
Oliveira BF, São-Pedro VA, Santos-Barrera G, Penone C, Costa GC (2017). “AmphiBIO, a global database
for amphibian ecological traits.” _Scientific Data_, *4*, 170123. doi: 10.1038/sdata.2017.123 (URL:
https://doi.org/10.1038/sdata.2017.123).

```r
amphibio <- readr::read_csv("data/amphibio.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_double(),
##   id = col_character(),
##   Order = col_character(),
##   Family = col_character(),
##   Genus = col_character(),
##   Species = col_character(),
##   Seeds = col_logical(),
##   OBS = col_logical()
## )
```

```
## See spec(...) for full column specifications.
```

```
## Warning: 125 parsing failures.
##  row col           expected                                                           actual                file
## 1410 OBS 1/0/T/F/TRUE/FALSE Identified as P. appendiculata in Boquimpani-Freitas et al. 2002 'data/amphibio.csv'
## 1416 OBS 1/0/T/F/TRUE/FALSE Identified as T. miliaris in Giaretta and Facure 2004            'data/amphibio.csv'
## 1447 OBS 1/0/T/F/TRUE/FALSE Considered endangered by Soto-Azat et al. 2013                   'data/amphibio.csv'
## 1448 OBS 1/0/T/F/TRUE/FALSE Considered extinct by Soto-Azat et al. 2013                      'data/amphibio.csv'
## 1471 OBS 1/0/T/F/TRUE/FALSE nomem dubitum                                                    'data/amphibio.csv'
## .... ... .................. ................................................................ ...................
## See problems(...) for more details.
```

## Questions  
**2. Do some exploratory analysis of the `amniota` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  


```r
anyNA(amniota)
```

```
## [1] FALSE
```

```r
amniota %>% 
  skimr::skim()
```


Table: Data summary

|                         |           |
|:------------------------|:----------|
|Name                     |Piped data |
|Number of rows           |21322      |
|Number of columns        |36         |
|_______________________  |           |
|Column type frequency:   |           |
|character                |6          |
|numeric                  |30         |
|________________________ |           |
|Group variables          |None       |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|class         |         0|             1|   4|   8|     0|        3|          0|
|order         |         0|             1|   6|  19|     0|       72|          0|
|family        |         0|             1|   6|  19|     0|      465|          0|
|genus         |         0|             1|   2|  20|     0|     4336|          0|
|species       |         0|             1|   2|  21|     0|    11548|          0|
|common_name   |         0|             1|   2| 306|     0|    19625|          0|


**Variable type: numeric**

|skim_variable                         | n_missing| complete_rate|     mean|         sd|        p0|     p25|     p50|     p75|         p100|hist                                     |
|:-------------------------------------|---------:|-------------:|--------:|----------:|---------:|-------:|-------:|-------:|------------:|:----------------------------------------|
|subspecies                            |         0|             1|  -999.00|       0.00|   -999.00| -999.00| -999.00| -999.00|      -999.00|▁▁▇▁▁ |
|female_maturity_d                     |         0|             1|  -723.70|     830.62| -30258.71| -999.00| -999.00| -999.00|      9131.25|▁▁▁▇▁ |
|litter_or_clutch_size_n               |         0|             1|  -383.91|     488.39|   -999.00| -999.00|    1.69|    3.20|       156.00|▅▁▁▁▇ |
|litters_or_clutches_per_y             |         0|             1|  -766.76|     422.48|   -999.00| -999.00| -999.00| -999.00|        52.00|▇▁▁▁▂ |
|adult_body_mass_g                     |         0|             1| 29107.30| 1278639.85|   -999.00|    4.44|   23.61|  135.00| 149000000.00|▇▁▁▁▁ |
|maximum_longevity_y                   |         0|             1|  -737.06|     444.36|   -999.00| -999.00| -999.00|    1.08|       211.00|▇▁▁▁▃ |
|gestation_d                           |         0|             1|  -874.91|     353.92|   -999.00| -999.00| -999.00| -999.00|      7396.92|▇▁▁▁▁ |
|weaning_d                             |         0|             1|  -892.45|     330.67|   -999.00| -999.00| -999.00| -999.00|      1826.25|▇▁▁▁▁ |
|birth_or_hatching_weight_g            |         0|             1|   -88.57|   26484.20|   -999.00| -999.00| -999.00| -999.00|   2250000.00|▇▁▁▁▁ |
|weaning_weight_g                      |         0|             1|  1116.10|  134348.60|   -999.00| -999.00| -999.00| -999.00|  17000000.00|▇▁▁▁▁ |
|egg_mass_g                            |         0|             1|  -739.64|     445.35|   -999.00| -999.00| -999.00|    0.56|      1500.00|▇▁▂▁▁ |
|incubation_d                          |         0|             1|  -820.49|     394.55|   -999.00| -999.00| -999.00| -999.00|      1762.00|▇▂▁▁▁ |
|fledging_age_d                        |         0|             1|  -909.42|     291.29|   -999.00| -999.00| -999.00| -999.00|       345.00|▇▁▁▁▁ |
|longevity_y                           |         0|             1|  -737.82|     443.03|   -999.00| -999.00| -999.00|    1.04|       177.00|▇▁▁▁▃ |
|male_maturity_d                       |         0|             1|  -827.77|     595.69|   -999.00| -999.00| -999.00| -999.00|      9131.25|▇▁▁▁▁ |
|inter_litter_or_interbirth_interval_y |         0|             1|  -932.50|     249.14|   -999.00| -999.00| -999.00| -999.00|         4.85|▇▁▁▁▁ |
|female_body_mass_g                    |         0|             1|    40.59|   27536.51|   -999.00| -999.00| -999.00|   14.50|   3700000.00|▇▁▁▁▁ |
|male_body_mass_g                      |         0|             1|  1242.90|   62044.69|   -999.00| -999.00| -999.00|   13.34|   4545000.00|▇▁▁▁▁ |
|no_sex_body_mass_g                    |         0|             1| 30689.26| 1467346.84|   -999.00| -999.00| -999.00|   27.77| 136000000.00|▇▁▁▁▁ |
|egg_width_mm                          |         0|             1|  -970.48|     168.36|   -999.00| -999.00| -999.00| -999.00|       125.00|▇▁▁▁▁ |
|egg_length_mm                         |         0|             1|  -968.89|     174.10|   -999.00| -999.00| -999.00| -999.00|       455.00|▇▁▁▁▁ |
|fledging_mass_g                       |         0|             1|  -984.64|     211.46|   -999.00| -999.00| -999.00| -999.00|      9992.00|▇▁▁▁▁ |
|adult_svl_cm                          |         0|             1|  -656.15|     490.74|   -999.00| -999.00| -999.00|    9.50|      3049.00|▇▃▁▁▁ |
|male_svl_cm                           |         0|             1|  -985.12|     120.02|   -999.00| -999.00| -999.00| -999.00|       315.20|▇▁▁▁▁ |
|female_svl_cm                         |         0|             1|  -947.35|     223.83|   -999.00| -999.00| -999.00| -999.00|      1125.00|▇▁▁▁▁ |
|birth_or_hatching_svl_cm              |         0|             1|  -940.34|     236.74|   -999.00| -999.00| -999.00| -999.00|       760.00|▇▁▁▁▁ |
|female_svl_at_maturity_cm             |         0|             1|  -989.36|      98.74|   -999.00| -999.00| -999.00| -999.00|       580.00|▇▁▁▁▁ |
|female_body_mass_at_maturity_g        |         0|             1|  -980.61|    1888.55|   -999.00| -999.00| -999.00| -999.00|    194000.00|▇▁▁▁▁ |
|no_sex_svl_cm                         |         0|             1|  -747.14|     442.27|   -999.00| -999.00| -999.00| -999.00|      3300.00|▇▂▁▁▁ |
|no_sex_maturity_d                     |         0|             1|  -942.59|     465.04|   -999.00| -999.00| -999.00| -999.00|     14610.00|▇▁▁▁▁ |


**3. Do some exploratory analysis of the `amphibio` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  


```r
anyNA(amphibio)
```

```
## [1] TRUE
```

```r
amphibio %>% 
  skimr::skim()
```


Table: Data summary

|                         |           |
|:------------------------|:----------|
|Name                     |Piped data |
|Number of rows           |6776       |
|Number of columns        |38         |
|_______________________  |           |
|Column type frequency:   |           |
|character                |5          |
|logical                  |2          |
|numeric                  |31         |
|________________________ |           |
|Group variables          |None       |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|id            |         0|             1|   7|   7|     0|     6776|          0|
|Order         |         0|             1|   5|  11|     0|        3|          0|
|Family        |         0|             1|   7|  20|     0|       61|          0|
|Genus         |         0|             1|   4|  17|     0|      531|          0|
|Species       |         0|             1|   9|  34|     0|     6776|          0|


**Variable type: logical**

|skim_variable | n_missing| complete_rate| mean|count  |
|:-------------|---------:|-------------:|----:|:------|
|Seeds         |      6772|             0|    1|TRU: 4 |
|OBS           |      6776|             0|  NaN|:      |


**Variable type: numeric**

|skim_variable           | n_missing| complete_rate|    mean|      sd|    p0|  p25|    p50|    p75|    p100|hist                                     |
|:-----------------------|---------:|-------------:|-------:|-------:|-----:|----:|------:|------:|-------:|:----------------------------------------|
|Fos                     |      6053|          0.11|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Ter                     |      1104|          0.84|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Aqu                     |      2810|          0.59|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Arb                     |      4347|          0.36|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Leaves                  |      6752|          0.00|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Flowers                 |      6772|          0.00|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Fruits                  |      6774|          0.00|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Arthro                  |      5534|          0.18|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Vert                    |      6657|          0.02|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Diu                     |      5876|          0.13|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Noc                     |      5156|          0.24|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Crepu                   |      6608|          0.02|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Wet_warm                |      5997|          0.11|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Wet_cold                |      6625|          0.02|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Dry_warm                |      6572|          0.03|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Dry_cold                |      6735|          0.01|    1.00|    0.00|  1.00|  1.0|   1.00|   1.00|     1.0|▁▁▇▁▁ |
|Body_mass_g             |      6185|          0.09|   94.56| 1093.77|  0.16|  2.6|   9.29|  31.83| 26000.0|▇▁▁▁▁ |
|Age_at_maturity_min_y   |      6392|          0.06|    2.14|    1.18|  0.25|  1.0|   2.00|   3.00|     7.0|▇▇▆▁▁ |
|Age_at_maturity_max_y   |      6392|          0.06|    2.96|    1.69|  0.30|  2.0|   3.00|   4.00|    12.0|▇▇▂▁▁ |
|Body_size_mm            |      1549|          0.77|   66.65|   91.47|  8.40| 29.0|  43.00|  69.15|  1520.0|▇▁▁▁▁ |
|Size_at_maturity_min_mm |      6529|          0.04|   56.63|   55.57|  8.80| 27.5|  43.00|  58.00|   350.0|▇▁▁▁▁ |
|Size_at_maturity_max_mm |      6528|          0.04|   67.46|   66.34| 10.10| 32.0|  50.00|  75.50|   400.0|▇▁▁▁▁ |
|Longevity_max_y         |      6417|          0.05|   11.68|    9.86|  0.17|  6.0|  10.00|  15.00|   121.8|▇▁▁▁▁ |
|Litter_size_min_n       |      5153|          0.24|  530.87| 1575.73|  1.00| 18.0|  80.00| 300.00| 25000.0|▇▁▁▁▁ |
|Litter_size_max_n       |      5153|          0.24| 1033.70| 2955.30|  1.00| 30.0| 186.00| 700.00| 45054.0|▇▁▁▁▁ |
|Reproductive_output_y   |      2344|          0.65|    1.03|    0.43|  0.08|  1.0|   1.00|   1.00|    15.0|▇▁▁▁▁ |
|Offspring_size_min_mm   |      5446|          0.20|    2.45|    1.57|  0.20|  1.4|   2.00|   3.00|    20.0|▇▁▁▁▁ |
|Offspring_size_max_mm   |      5446|          0.20|    2.86|    1.94|  0.40|  1.6|   2.30|   3.50|    25.0|▇▁▁▁▁ |
|Dir                     |      1079|          0.84|    0.30|    0.46|  0.00|  0.0|   0.00|   1.00|     1.0|▇▁▁▁▃ |
|Lar                     |      1079|          0.84|    0.69|    0.46|  0.00|  0.0|   1.00|   1.00|     1.0|▃▁▁▁▇ |
|Viv                     |      1079|          0.84|    0.01|    0.10|  0.00|  0.0|   0.00|   0.00|     1.0|▇▁▁▁▁ |


**4. How many total NA's are in each data set? Do these values make sense? Are NA's represented by values?**   

_There are 170691 NA's in the amphibio data set, while there are 0 NA's in the amniota data set. In regards to the amniota data, this does not make sense, and it is because the NA's are being represented by -999._


```r
amniota %>% 
  summarise_all(~(sum(is.na(.))))
```

```
## # A tibble: 1 x 36
##   class order family genus species subspecies common_name female_maturity~
##   <int> <int>  <int> <int>   <int>      <int>       <int>            <int>
## 1     0     0      0     0       0          0           0                0
## # ... with 28 more variables: litter_or_clutch_size_n <int>,
## #   litters_or_clutches_per_y <int>, adult_body_mass_g <int>,
## #   maximum_longevity_y <int>, gestation_d <int>, weaning_d <int>,
## #   birth_or_hatching_weight_g <int>, weaning_weight_g <int>, egg_mass_g <int>,
## #   incubation_d <int>, fledging_age_d <int>, longevity_y <int>,
## #   male_maturity_d <int>, inter_litter_or_interbirth_interval_y <int>,
## #   female_body_mass_g <int>, male_body_mass_g <int>, no_sex_body_mass_g <int>,
## #   egg_width_mm <int>, egg_length_mm <int>, fledging_mass_g <int>,
## #   adult_svl_cm <int>, male_svl_cm <int>, female_svl_cm <int>,
## #   birth_or_hatching_svl_cm <int>, female_svl_at_maturity_cm <int>,
## #   female_body_mass_at_maturity_g <int>, no_sex_svl_cm <int>,
## #   no_sex_maturity_d <int>
```

```r
NA_amphibio<- amphibio %>% 
  summarise_all(~(sum(is.na(.))))
sum(NA_amphibio)
```

```
## [1] 170691
```

**5. Make any necessary replacements in the data such that all NA's appear as "NA".**   

```r
amniota_tidy<- amniota %>% 
  na_if("-999") %>% 
  na_if("-30258.711")
```


```r
amniota_tidy %>% 
  summary()
```

```
##     class              order              family             genus          
##  Length:21322       Length:21322       Length:21322       Length:21322      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##    species            subspecies    common_name        female_maturity_d
##  Length:21322       Min.   : NA     Length:21322       Min.   :  23.81  
##  Class :character   1st Qu.: NA     Class :character   1st Qu.: 289.00  
##  Mode  :character   Median : NA     Mode  :character   Median : 365.12  
##                     Mean   :NaN                        Mean   : 726.85  
##                     3rd Qu.: NA                        3rd Qu.: 819.34  
##                     Max.   : NA                        Max.   :9131.25  
##                     NA's   :21322                      NA's   :17853    
##  litter_or_clutch_size_n litters_or_clutches_per_y adult_body_mass_g  
##  Min.   :  0.900         Min.   : 0.120            Min.   :        0  
##  1st Qu.:  2.000         1st Qu.: 1.000            1st Qu.:       15  
##  Median :  2.800         Median : 1.050            Median :       44  
##  Mean   :  3.826         Mean   : 1.752            Mean   :    37493  
##  3rd Qu.:  4.150         3rd Qu.: 2.000            3rd Qu.:      238  
##  Max.   :156.000         Max.   :52.000            Max.   :149000000  
##  NA's   :8244            NA's   :16374             NA's   :4645       
##  maximum_longevity_y  gestation_d        weaning_d      
##  Min.   :  0.083     Min.   :   5.00   Min.   :   1.94  
##  1st Qu.:  6.000     1st Qu.:  29.91   1st Qu.:  27.75  
##  Median : 12.308     Median :  63.92   Median :  51.60  
##  Mean   : 16.466     Mean   : 105.28   Mean   : 113.05  
##  3rd Qu.: 22.000     3rd Qu.: 151.88   3rd Qu.: 129.83  
##  Max.   :211.000     Max.   :7396.92   Max.   :1826.25  
##  NA's   :15822       NA's   :18926     NA's   :19279    
##  birth_or_hatching_weight_g weaning_weight_g     egg_mass_g      
##  Min.   :0.00e+00           Min.   :       1   Min.   :   0.218  
##  1st Qu.:1.30e+00           1st Qu.:      13   1st Qu.:   2.100  
##  Median :5.90e+00           Median :      43   Median :   5.100  
##  Mean   :4.48e+03           Mean   :   41386   Mean   :  22.252  
##  3rd Qu.:4.39e+01           3rd Qu.:     850   3rd Qu.:  20.100  
##  Max.   :2.25e+06           Max.   :17000000   Max.   :1500.000  
##  NA's   :17779              NA's   :20258      NA's   :15907     
##   incubation_d     fledging_age_d   longevity_y      male_maturity_d  
##  Min.   :   2.00   Min.   :  1.0   Min.   :  0.083   Min.   :  30.44  
##  1st Qu.:  17.00   1st Qu.: 16.5   1st Qu.:  5.500   1st Qu.: 365.00  
##  Median :  29.25   Median : 27.5   Median : 10.700   Median : 365.25  
##  Mean   :  46.67   Mean   : 36.8   Mean   : 13.521   Mean   : 787.16  
##  3rd Qu.:  59.50   3rd Qu.: 46.0   3rd Qu.: 18.200   3rd Qu.: 913.00  
##  Max.   :1762.00   Max.   :345.0   Max.   :177.000   Max.   :9131.25  
##  NA's   :17682     NA's   :19478   NA's   :15822     NA's   :19278    
##  inter_litter_or_interbirth_interval_y female_body_mass_g male_body_mass_g 
##  Min.   :0.047                         Min.   :      0    Min.   :      0  
##  1st Qu.:0.318                         1st Qu.:     14    1st Qu.:     16  
##  Median :0.999                         Median :     41    Median :     48  
##  Mean   :0.907                         Mean   :   2076    Mean   :   6197  
##  3rd Qu.:1.000                         3rd Qu.:    220    3rd Qu.:    246  
##  Max.   :4.847                         Max.   :3700000    Max.   :4545000  
##  NA's   :19904                         NA's   :14113      NA's   :14679    
##  no_sex_body_mass_g   egg_width_mm    egg_length_mm    fledging_mass_g  
##  Min.   :        0   Min.   :  2.50   Min.   :  2.50   Min.   :   4.85  
##  1st Qu.:       13   1st Qu.:  8.00   1st Qu.: 10.94   1st Qu.:  14.60  
##  Median :       35   Median : 13.00   Median : 19.98   Median :  24.80  
##  Mean   :    68952   Mean   : 22.99   Mean   : 36.40   Mean   : 452.27  
##  3rd Qu.:      164   3rd Qu.: 35.90   3rd Qu.: 58.92   3rd Qu.: 107.00  
##  Max.   :136000000   Max.   :125.00   Max.   :455.00   Max.   :9992.00  
##  NA's   :11663       NA's   :20727    NA's   :20702    NA's   :21111    
##   adult_svl_cm      male_svl_cm     female_svl_cm      birth_or_hatching_svl_cm
##  Min.   :   1.79   Min.   :  1.57   Min.   :   1.800   Min.   :  0.400         
##  1st Qu.:   9.50   1st Qu.: 21.41   1st Qu.:   5.756   1st Qu.:  2.450         
##  Median :  18.50   Median : 35.85   Median :   8.150   Median :  3.300         
##  Mean   :  38.20   Mean   : 50.44   Mean   :  20.609   Mean   : 12.099         
##  3rd Qu.:  40.50   3rd Qu.: 63.39   3rd Qu.:  17.721   3rd Qu.:  5.256         
##  Max.   :3049.00   Max.   :315.20   Max.   :1125.000   Max.   :759.999         
##  NA's   :14274     NA's   :21040    NA's   :20242      NA's   :20085           
##  female_svl_at_maturity_cm female_body_mass_at_maturity_g no_sex_svl_cm   
##  Min.   :  2.85            Min.   :    30.0               Min.   :   1.7  
##  1st Qu.:  4.90            1st Qu.:    82.5               1st Qu.:   5.7  
##  Median :  6.00            Median : 97050.0               Median :   7.7  
##  Mean   : 18.69            Mean   : 97032.5               Mean   :  20.0  
##  3rd Qu.:  8.40            3rd Qu.:194000.0               3rd Qu.:  11.0  
##  Max.   :580.00            Max.   :194000.0               Max.   :3300.0  
##  NA's   :21120             NA's   :21318                  NA's   :16052   
##  no_sex_maturity_d
##  Min.   :   33.0  
##  1st Qu.:  365.3  
##  Median :  913.1  
##  Mean   : 1604.5  
##  3rd Qu.: 2008.9  
##  Max.   :14610.0  
##  NA's   :20860
```

**6. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amniota` data.**  

```r
amniota_tidy%>%
  naniar::miss_var_summary()
```

```
## # A tibble: 36 x 3
##    variable                       n_miss pct_miss
##    <chr>                           <int>    <dbl>
##  1 subspecies                      21322    100  
##  2 female_body_mass_at_maturity_g  21318    100. 
##  3 female_svl_at_maturity_cm       21120     99.1
##  4 fledging_mass_g                 21111     99.0
##  5 male_svl_cm                     21040     98.7
##  6 no_sex_maturity_d               20860     97.8
##  7 egg_width_mm                    20727     97.2
##  8 egg_length_mm                   20702     97.1
##  9 weaning_weight_g                20258     95.0
## 10 female_svl_cm                   20242     94.9
## # ... with 26 more rows
```

**7. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amphibio` data.**

```r
amphibio %>% 
  naniar::miss_var_summary()
```

```
## # A tibble: 38 x 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 OBS        6776    100  
##  2 Fruits     6774    100. 
##  3 Flowers    6772     99.9
##  4 Seeds      6772     99.9
##  5 Leaves     6752     99.6
##  6 Dry_cold   6735     99.4
##  7 Vert       6657     98.2
##  8 Wet_cold   6625     97.8
##  9 Crepu      6608     97.5
## 10 Dry_warm   6572     97.0
## # ... with 28 more rows
```

**8. For the `amniota` data, calculate the number of NAs in the `egg_mass_g` column sorted by taxonomic class; i.e. how many NA's are present in the `egg_mass_g` column in birds, mammals, and reptiles? Does this results make sense biologically? How do these results affect your interpretation of NA's?**  

_Yes this makes sense. Mammals do not generally lay eggs._



```r
amniota_tidy %>% 
  select(egg_mass_g, class) %>%
  group_by(class) %>% 
  naniar::miss_var_summary()
```

```
## # A tibble: 3 x 4
## # Groups:   class [3]
##   class    variable   n_miss pct_miss
##   <chr>    <chr>       <int>    <dbl>
## 1 Aves     egg_mass_g   4914     50.1
## 2 Mammalia egg_mass_g   4953    100  
## 3 Reptilia egg_mass_g   6040     92.0
```

**9. The `amphibio` data have variables that classify species as fossorial (burrowing), terrestrial, aquatic, or arboreal.Calculate the number of NA's in each of these variables. Do you think that the authors intend us to think that there are NA's in these columns or could they represent something else? Explain.**

_The NA's in these columns most likely mean that they do not belong to that group, rather than the data being unavailable. Several of the animals belong to multiple groups, but based on the percentages, most belong only to a single group. _


```r
amphibio %>% 
  select(Fos, Aqu, Arb, Ter) %>%
  naniar::miss_var_summary()
```

```
## # A tibble: 4 x 3
##   variable n_miss pct_miss
##   <chr>     <int>    <dbl>
## 1 Fos        6053     89.3
## 2 Arb        4347     64.2
## 3 Aqu        2810     41.5
## 4 Ter        1104     16.3
```

```r
(100-89.32999)+(100-64.15289)+(100-41.46989)+(100-16.29280)
```

```
## [1] 188.7544
```

```r
amphibio %>% 
  select(Fos, Aqu, Arb) %>%
  filter(Fos=="1",Aqu== "1")
```

```
## # A tibble: 591 x 3
##      Fos   Aqu   Arb
##    <dbl> <dbl> <dbl>
##  1     1     1     1
##  2     1     1    NA
##  3     1     1    NA
##  4     1     1    NA
##  5     1     1    NA
##  6     1     1    NA
##  7     1     1    NA
##  8     1     1    NA
##  9     1     1    NA
## 10     1     1    NA
## # ... with 581 more rows
```



**10. Now that we know how NA's are represented in the `amniota` data, how would you load the data such that the values which represent NA's are automatically converted?**

_It seems like some of the variables were converted from numeric to logical in the process but otherwise it worked?_

```r
amniota_improved<-readr::read_csv(file="data/amniota.csv",na=c("-999","-30258.711"))
```

```
## Parsed with column specification:
## cols(
##   .default = col_double(),
##   class = col_character(),
##   order = col_character(),
##   family = col_character(),
##   genus = col_character(),
##   species = col_character(),
##   subspecies = col_logical(),
##   common_name = col_character(),
##   gestation_d = col_logical(),
##   weaning_d = col_logical(),
##   weaning_weight_g = col_logical(),
##   male_svl_cm = col_logical(),
##   female_svl_cm = col_logical(),
##   birth_or_hatching_svl_cm = col_logical(),
##   female_svl_at_maturity_cm = col_logical(),
##   female_body_mass_at_maturity_g = col_logical(),
##   no_sex_svl_cm = col_logical()
## )
```

```
## See spec(...) for full column specifications.
```

```
## Warning: 13577 parsing failures.
##  row                      col           expected actual               file
## 9803 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9804 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9805 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9806 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## 9807 birth_or_hatching_svl_cm 1/0/T/F/TRUE/FALSE    4.7 'data/amniota.csv'
## .... ........................ .................. ...... ..................
## See problems(...) for more details.
```

```r
amniota_improved %>% 
  skimr::skim()
```


Table: Data summary

|                         |           |
|:------------------------|:----------|
|Name                     |Piped data |
|Number of rows           |21322      |
|Number of columns        |36         |
|_______________________  |           |
|Column type frequency:   |           |
|character                |6          |
|logical                  |10         |
|numeric                  |20         |
|________________________ |           |
|Group variables          |None       |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|class         |         0|          1.00|   4|   8|     0|        3|          0|
|order         |         0|          1.00|   6|  19|     0|       72|          0|
|family        |         0|          1.00|   6|  19|     0|      465|          0|
|genus         |         0|          1.00|   2|  20|     0|     4336|          0|
|species       |         0|          1.00|   2|  21|     0|    11548|          0|
|common_name   |      1641|          0.92|   2| 306|     0|    19624|          0|


**Variable type: logical**

|skim_variable                  | n_missing| complete_rate| mean|count  |
|:------------------------------|---------:|-------------:|----:|:------|
|subspecies                     |     21322|             0|  NaN|:      |
|gestation_d                    |     21322|             0|  NaN|:      |
|weaning_d                      |     21322|             0|  NaN|:      |
|weaning_weight_g               |     21322|             0|  NaN|:      |
|male_svl_cm                    |     21322|             0|  NaN|:      |
|female_svl_cm                  |     21322|             0|  NaN|:      |
|birth_or_hatching_svl_cm       |     21321|             0|    1|TRU: 1 |
|female_svl_at_maturity_cm      |     21322|             0|  NaN|:      |
|female_body_mass_at_maturity_g |     21322|             0|  NaN|:      |
|no_sex_svl_cm                  |     21322|             0|  NaN|:      |


**Variable type: numeric**

|skim_variable                         | n_missing| complete_rate|     mean|         sd|    p0|    p25|    p50|     p75|        p100|hist                                     |
|:-------------------------------------|---------:|-------------:|--------:|----------:|-----:|------:|------:|-------:|-----------:|:----------------------------------------|
|female_maturity_d                     |     17853|          0.16|   726.85|     860.65| 23.81| 289.00| 365.12|  819.34| 9.13125e+03|▇▁▁▁▁ |
|litter_or_clutch_size_n               |      8244|          0.61|     3.83|       5.17|  0.90|   2.00|   2.80|    4.15| 1.56000e+02|▇▁▁▁▁ |
|litters_or_clutches_per_y             |     16374|          0.23|     1.75|       1.83|  0.12|   1.00|   1.05|    2.00| 5.20000e+01|▇▁▁▁▁ |
|adult_body_mass_g                     |      4645|          0.78| 37492.72| 1445681.23|  0.10|  14.90|  44.35|  238.00| 1.49000e+08|▇▁▁▁▁ |
|maximum_longevity_y                   |     15822|          0.26|    16.47|      16.29|  0.08|   6.00|  12.31|   22.00| 2.11000e+02|▇▁▁▁▁ |
|birth_or_hatching_weight_g            |     17779|          0.17|  4480.02|   64785.04|  0.00|   1.31|   5.89|   43.86| 2.25000e+06|▇▁▁▁▁ |
|egg_mass_g                            |     15907|          0.25|    22.25|      53.44|  0.22|   2.10|   5.10|   20.10| 1.50000e+03|▇▁▁▁▁ |
|incubation_d                          |     17682|          0.17|    46.67|      71.11|  2.00|  17.00|  29.25|   59.50| 1.76200e+03|▇▁▁▁▁ |
|fledging_age_d                        |     19478|          0.09|    36.80|      31.27|  1.00|  16.50|  27.50|   46.00| 3.45000e+02|▇▁▁▁▁ |
|longevity_y                           |     15822|          0.26|    13.52|      11.69|  0.08|   5.50|  10.70|   18.20| 1.77000e+02|▇▁▁▁▁ |
|male_maturity_d                       |     19278|          0.10|   787.16|     904.06| 30.44| 365.00| 365.25|  913.00| 9.13125e+03|▇▁▁▁▁ |
|inter_litter_or_interbirth_interval_y |     19904|          0.07|     0.91|       0.74|  0.05|   0.32|   1.00|    1.00| 4.85000e+00|▇▁▁▁▁ |
|female_body_mass_g                    |     14113|          0.34|  2075.77|   47293.18|  0.30|  14.00|  40.70|  220.00| 3.70000e+06|▇▁▁▁▁ |
|male_body_mass_g                      |     14679|          0.31|  6196.82|  111002.20|  0.30|  16.50|  48.40|  245.52| 4.54500e+06|▇▁▁▁▁ |
|no_sex_body_mass_g                    |     11663|          0.45| 68952.04| 2179569.11|  0.10|  13.10|  34.60|  164.00| 1.36000e+08|▇▁▁▁▁ |
|egg_width_mm                          |     20727|          0.03|    22.99|      20.30|  2.50|   8.00|  13.00|   35.90| 1.25000e+02|▇▂▁▁▁ |
|egg_length_mm                         |     20702|          0.03|    36.40|      38.67|  2.50|  10.94|  19.98|   58.92| 4.55000e+02|▇▁▁▁▁ |
|fledging_mass_g                       |     21111|          0.01|   452.27|    1563.53|  4.85|  14.60|  24.80|  107.00| 9.99200e+03|▇▁▁▁▁ |
|adult_svl_cm                          |     14274|          0.33|    38.20|      91.29|  1.79|   9.50|  18.50|   40.50| 3.04900e+03|▇▁▁▁▁ |
|no_sex_maturity_d                     |     20860|          0.02|  1604.51|    1831.96| 33.00| 365.26| 913.10| 2008.88| 1.46100e+04|▇▁▁▁▁ |

```r
amniota_tidy %>% 
  skimr::skim()
```


Table: Data summary

|                         |           |
|:------------------------|:----------|
|Name                     |Piped data |
|Number of rows           |21322      |
|Number of columns        |36         |
|_______________________  |           |
|Column type frequency:   |           |
|character                |6          |
|numeric                  |30         |
|________________________ |           |
|Group variables          |None       |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|class         |         0|          1.00|   4|   8|     0|        3|          0|
|order         |         0|          1.00|   6|  19|     0|       72|          0|
|family        |         0|          1.00|   6|  19|     0|      465|          0|
|genus         |         0|          1.00|   2|  20|     0|     4336|          0|
|species       |         0|          1.00|   2|  21|     0|    11548|          0|
|common_name   |      1641|          0.92|   2| 306|     0|    19624|          0|


**Variable type: numeric**

|skim_variable                         | n_missing| complete_rate|     mean|         sd|    p0|    p25|      p50|       p75|        p100|hist                                     |
|:-------------------------------------|---------:|-------------:|--------:|----------:|-----:|------:|--------:|---------:|-----------:|:----------------------------------------|
|subspecies                            |     21322|          0.00|      NaN|         NA|    NA|     NA|       NA|        NA|          NA|                                         |
|female_maturity_d                     |     17853|          0.16|   726.85|     860.65| 23.81| 289.00|   365.12|    819.34| 9.13125e+03|▇▁▁▁▁ |
|litter_or_clutch_size_n               |      8244|          0.61|     3.83|       5.17|  0.90|   2.00|     2.80|      4.15| 1.56000e+02|▇▁▁▁▁ |
|litters_or_clutches_per_y             |     16374|          0.23|     1.75|       1.83|  0.12|   1.00|     1.05|      2.00| 5.20000e+01|▇▁▁▁▁ |
|adult_body_mass_g                     |      4645|          0.78| 37492.72| 1445681.23|  0.10|  14.90|    44.35|    238.00| 1.49000e+08|▇▁▁▁▁ |
|maximum_longevity_y                   |     15822|          0.26|    16.47|      16.29|  0.08|   6.00|    12.31|     22.00| 2.11000e+02|▇▁▁▁▁ |
|gestation_d                           |     18926|          0.11|   105.29|     179.49|  5.00|  29.91|    63.92|    151.88| 7.39692e+03|▇▁▁▁▁ |
|weaning_d                             |     19279|          0.10|   113.05|     151.60|  1.94|  27.75|    51.60|    129.83| 1.82625e+03|▇▁▁▁▁ |
|birth_or_hatching_weight_g            |     17779|          0.17|  4480.02|   64785.04|  0.00|   1.31|     5.89|     43.86| 2.25000e+06|▇▁▁▁▁ |
|weaning_weight_g                      |     20258|          0.05| 41386.39|  600265.12|  0.94|  13.16|    43.08|    850.32| 1.70000e+07|▇▁▁▁▁ |
|egg_mass_g                            |     15907|          0.25|    22.25|      53.44|  0.22|   2.10|     5.10|     20.10| 1.50000e+03|▇▁▁▁▁ |
|incubation_d                          |     17682|          0.17|    46.67|      71.11|  2.00|  17.00|    29.25|     59.50| 1.76200e+03|▇▁▁▁▁ |
|fledging_age_d                        |     19478|          0.09|    36.80|      31.27|  1.00|  16.50|    27.50|     46.00| 3.45000e+02|▇▁▁▁▁ |
|longevity_y                           |     15822|          0.26|    13.52|      11.69|  0.08|   5.50|    10.70|     18.20| 1.77000e+02|▇▁▁▁▁ |
|male_maturity_d                       |     19278|          0.10|   787.16|     904.06| 30.44| 365.00|   365.25|    913.00| 9.13125e+03|▇▁▁▁▁ |
|inter_litter_or_interbirth_interval_y |     19904|          0.07|     0.91|       0.74|  0.05|   0.32|     1.00|      1.00| 4.85000e+00|▇▁▁▁▁ |
|female_body_mass_g                    |     14113|          0.34|  2075.77|   47293.18|  0.30|  14.00|    40.70|    220.00| 3.70000e+06|▇▁▁▁▁ |
|male_body_mass_g                      |     14679|          0.31|  6196.82|  111002.20|  0.30|  16.50|    48.40|    245.52| 4.54500e+06|▇▁▁▁▁ |
|no_sex_body_mass_g                    |     11663|          0.45| 68952.04| 2179569.11|  0.10|  13.10|    34.60|    164.00| 1.36000e+08|▇▁▁▁▁ |
|egg_width_mm                          |     20727|          0.03|    22.99|      20.30|  2.50|   8.00|    13.00|     35.90| 1.25000e+02|▇▂▁▁▁ |
|egg_length_mm                         |     20702|          0.03|    36.40|      38.67|  2.50|  10.94|    19.98|     58.92| 4.55000e+02|▇▁▁▁▁ |
|fledging_mass_g                       |     21111|          0.01|   452.27|    1563.53|  4.85|  14.60|    24.80|    107.00| 9.99200e+03|▇▁▁▁▁ |
|adult_svl_cm                          |     14274|          0.33|    38.20|      91.29|  1.79|   9.50|    18.50|     40.50| 3.04900e+03|▇▁▁▁▁ |
|male_svl_cm                           |     21040|          0.01|    50.44|      49.07|  1.57|  21.41|    35.85|     63.39| 3.15200e+02|▇▂▁▁▁ |
|female_svl_cm                         |     20242|          0.05|    20.61|      45.63|  1.80|   5.76|     8.15|     17.72| 1.12500e+03|▇▁▁▁▁ |
|birth_or_hatching_svl_cm              |     20085|          0.06|    12.10|      54.73|  0.40|   2.45|     3.30|      5.26| 7.60000e+02|▇▁▁▁▁ |
|female_svl_at_maturity_cm             |     21120|          0.01|    18.69|      56.08|  2.85|   4.90|     6.00|      8.40| 5.80000e+02|▇▁▁▁▁ |
|female_body_mass_at_maturity_g        |     21318|          0.00| 97032.50|  111968.43| 30.00|  82.50| 97050.00| 194000.00| 1.94000e+05|▇▁▁▁▇ |
|no_sex_svl_cm                         |     16052|          0.25|    20.00|      98.23|  1.70|   5.70|     7.70|     11.00| 3.30000e+03|▇▁▁▁▁ |
|no_sex_maturity_d                     |     20860|          0.02|  1604.51|    1831.96| 33.00| 365.26|   913.10|   2008.88| 1.46100e+04|▇▁▁▁▁ |



## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
