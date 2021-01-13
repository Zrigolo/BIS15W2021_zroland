---
title: "Lab 3 Homework"
author: "Zabrisky Roland"
date: "2021-01-13"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.

```r
msleep
```

```
## # A tibble: 83 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Chee~ Acin~ carni Carn~ lc                  12.1      NA        NA      11.9
##  2 Owl ~ Aotus omni  Prim~ <NA>                17         1.8      NA       7  
##  3 Moun~ Aplo~ herbi Rode~ nt                  14.4       2.4      NA       9.6
##  4 Grea~ Blar~ omni  Sori~ lc                  14.9       2.3       0.133   9.1
##  5 Cow   Bos   herbi Arti~ domesticated         4         0.7       0.667  20  
##  6 Thre~ Brad~ herbi Pilo~ <NA>                14.4       2.2       0.767   9.6
##  7 Nort~ Call~ carni Carn~ vu                   8.7       1.4       0.383  15.3
##  8 Vesp~ Calo~ <NA>  Rode~ <NA>                 7        NA        NA      17  
##  9 Dog   Canis carni Carn~ domesticated        10.1       2.9       0.333  13.9
## 10 Roe ~ Capr~ herbi Arti~ lc                   3        NA        NA      21  
## # ... with 73 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

2. Store these data into a new data frame `sleep`.

```r
sleep<-data.frame(msleep)
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below. 
_The dimensions are 83x11._

```r
dim(sleep)
```

```
## [1] 83 11
```

4. Are there any NAs in the data? How did you determine this? Please show your code.  
_There is at least one NA in the data set._

```r
anyNA(sleep)
```

```
## [1] TRUE
```

5. Show a list of the column names is this data frame.

```r
colnames(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  
_There are 32 herbivores represented in the data._

```r
herbivore<- subset(sleep, vore == "herbi")
dim(herbivore)
```

```
## [1] 32 11
```

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
little_mammals <- subset(sleep, bodywt<=1)
large_mammals <- subset(sleep, bodywt>=200)
```

8. What is the mean weight for both the small and large mammals?

```r
mean(little_mammals$bodywt)
```

```
## [1] 0.2596667
```


```r
mean(large_mammals$bodywt)
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  
_On average, small animals sleep longer than large animals_

```r
mean(large_mammals$sleep_total)
```

```
## [1] 3.3
```


```r
mean(little_mammals$sleep_total)
```

```
## [1] 12.65833
```

10. Which animal is the sleepiest among the entire dataframe?
_The sleepiest animal among the entire data frame is the Little Brown Bat with a daily value of 19.9 hours._

```r
X= sleep$sleep_total
max(X)
```

```
## [1] 19.9
```


```r
big_sleep<-subset(sleep,sleep_total==19.9)
big_sleep
```

```
##                name  genus    vore      order conservation sleep_total
## 43 Little brown bat Myotis insecti Chiroptera         <NA>        19.9
##    sleep_rem sleep_cycle awake brainwt bodywt
## 43         2         0.2   4.1 0.00025   0.01
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
