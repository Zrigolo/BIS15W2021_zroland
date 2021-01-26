---
title: "Midterm 1"
author: "Zabrisky Roland"
date: "2021-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
library("janitor")
library("skimr")
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  

_R is a script language and R studio is a GUI that improves user interaction with R. Git hub is version control software that tracks the history of the code uploaded. All changes made to a repository are tracked making the code transparent. RMarkdown is a clean formatted document that is accessible to all and allows for the viewer to easily interpret the code. The tracking and availability of the code using this combination is what makes work flows in data science transparent and repeatable._


**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**

_The three types of data structures We have discussed are vectors, data matrices and data frames.Data frames can store different classes of data simultaneously and can be manipulated easily and efficiently._

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**

```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## Parsed with column specification:
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```

```r
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17...
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204....
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", ...
```

```r
str(elephants)
```

```
## tibble [288 x 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ Age   : num [1:288] 1.4 17.5 12.8 11.2 12.7 ...
##  $ Height: num [1:288] 120 227 235 210 220 ...
##  $ Sex   : chr [1:288] "M" "M" "M" "M" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Age = col_double(),
##   ..   Height = col_double(),
##   ..   Sex = col_character()
##   .. )
```




**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**

```r
elephants<- rename(elephants, age="Age", height="Height", sex= "Sex")
```

```r
names(elephants)
```

```
## [1] "age"    "height" "sex"
```

```r
elephants$sex <- as.factor(elephants$sex)
class(elephants$sex)
```

```
## [1] "factor"
```





**5. (2 points) How many male and female elephants are represented in the data?**\

_There are 150 females and 138 males represented in the data._

```r
elephants %>% 
  tabyl(sex)
```

```
##  sex   n   percent
##    F 150 0.5208333
##    M 138 0.4791667
```


**6. (2 points) What is the average age all elephants in the data?**

_The average age of all elephants represented in the data is 10.97132._

```r
elephants %>% 
  arrange(desc(age))
```

```
## # A tibble: 288 x 3
##      age height sex  
##    <dbl>  <dbl> <fct>
##  1  32.2   261. F    
##  2  30.5   227. F    
##  3  30.2   278. F    
##  4  29.2   226. F    
##  5  28.8   302. M    
##  6  28.6   235. F    
##  7  28.4   228. F    
##  8  28.2   216. F    
##  9  28.2   266. M    
## 10  28     232. F    
## # ... with 278 more rows
```


```r
elephants %>%
  summarise(average_age= mean(age))
```

```
## # A tibble: 1 x 1
##   average_age
##         <dbl>
## 1        11.0
```


**7. (2 points) How does the average age and height of elephants compare by sex?**
_The average age and height is higher in female elephants._

```r
elephants %>% 
  group_by(sex) %>% 
  summarise(compare_age= mean(age), compare_height= mean(height))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 3
##   sex   compare_age compare_height
##   <fct>       <dbl>          <dbl>
## 1 F           12.8            190.
## 2 M            8.95           185.
```

**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**

_For this subgroup, females still have a higher average age, but now males have the higher average height. Both the minimum and maximum height of the males is larger than that of the females._


```r
elephants %>% 
  filter(age>25) %>% 
  group_by(sex) %>% 
  summarise(compare_age= mean(age), compare_height= mean(height), min_height= min(height), max_height= max(height), n=n())
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 6
##   sex   compare_age compare_height min_height max_height     n
##   <fct>       <dbl>          <dbl>      <dbl>      <dbl> <int>
## 1 F            27.3           233.       206.       278.    25
## 2 M            26.6           273.       237.       304.     8
```


For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**


```r
defauntation <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
```

```
## See spec(...) for full column specifications.
```

```r
glimpse(defauntation)
```

```
## Rows: 24
## Columns: 26
## $ TransectID              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 1...
## $ Distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24...
## $ HuntCat                 <chr> "Moderate", "None", "None", "None", "None",...
## $ NumHouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46,...
## $ LandUse                 <chr> "Park", "Park", "Park", "Logging", "Park", ...
## $ Veg_Rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 1...
## $ Veg_Stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 3...
## $ Veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38...
## $ Veg_DBH                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 5...
## $ Veg_Canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4...
## $ Veg_Understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2...
## $ RA_Apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, ...
## $ RA_Birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 4...
## $ RA_Elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0...
## $ RA_Monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 4...
## $ RA_Rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1...
## $ RA_Ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10...
## $ Rich_AllSpecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21,...
## $ Evenness_AllSpecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0...
## $ Diversity_AllSpecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2...
## $ Rich_BirdSpecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11...
## $ Evenness_BirdSpecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0...
## $ Diversity_BirdSpecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1...
## $ Rich_MammalSpecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 1...
## $ Evenness_MammalSpecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0...
## $ Diversity_MammalSpecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1...
```

```r
names(defauntation)
```

```
##  [1] "TransectID"              "Distance"               
##  [3] "HuntCat"                 "NumHouseholds"          
##  [5] "LandUse"                 "Veg_Rich"               
##  [7] "Veg_Stems"               "Veg_liana"              
##  [9] "Veg_DBH"                 "Veg_Canopy"             
## [11] "Veg_Understory"          "RA_Apes"                
## [13] "RA_Birds"                "RA_Elephant"            
## [15] "RA_Monkeys"              "RA_Rodent"              
## [17] "RA_Ungulate"             "Rich_AllSpecies"        
## [19] "Evenness_AllSpecies"     "Diversity_AllSpecies"   
## [21] "Rich_BirdSpecies"        "Evenness_BirdSpecies"   
## [23] "Diversity_BirdSpecies"   "Rich_MammalSpecies"     
## [25] "Evenness_MammalSpecies"  "Diversity_MammalSpecies"
```

```r
defauntation$HuntCat <- as.factor(defauntation$HuntCat)
defauntation$LandUse <- as.factor(defauntation$LandUse)
```

```r
class(defauntation$HuntCat)
```

```
## [1] "factor"
```

```r
class(defauntation$LandUse)
```

```
## [1] "factor"
```
**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**

_The average diversity of mammals is slightly higher than that of birds in both high and moderate hunting intensity areas._


```r
defauntation %>% 
  select(HuntCat, Diversity_MammalSpecies, Diversity_BirdSpecies) %>% 
  group_by(HuntCat) %>% 
  filter(HuntCat== "Moderate" | HuntCat== "High") %>% 
  summarise(mammal_D_avg= mean(Diversity_MammalSpecies), bird_D_avg= mean(Diversity_BirdSpecies))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 3
##   HuntCat  mammal_D_avg bird_D_avg
##   <fct>           <dbl>      <dbl>
## 1 High             1.74       1.66
## 2 Moderate         1.68       1.62
```


**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  

_Birds and rodents are found closer to villages on average, while monkeys, apes, elephants, and ungulates are more abundant further away from villages._

```r
defauntation %>% 
  filter(Distance>20 | Distance<5) %>% 
  group_by(Distance>20, Distance<5) %>% 
  summarise(across(contains("RA"), mean, na.rm=T))
```

```
## `summarise()` regrouping output by 'Distance > 20' (override with `.groups` argument)
```

```
## # A tibble: 2 x 9
## # Groups:   Distance > 20 [2]
##   `Distance > 20` `Distance < 5` TransectID RA_Apes RA_Birds RA_Elephant
##   <lgl>           <lgl>               <dbl>   <dbl>    <dbl>       <dbl>
## 1 FALSE           TRUE                 19.7    0.08     70.4      0.0967
## 2 TRUE            FALSE                11      7.21     44.5      0.557 
## # ... with 3 more variables: RA_Monkeys <dbl>, RA_Rodent <dbl>,
## #   RA_Ungulate <dbl>
```


**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**

_The area with the highest relative abundance of apes is 20.85 km away from any village._


```r
defauntation %>% 
  select(RA_Apes, Distance) %>% 
  arrange(desc(RA_Apes))
```

```
## # A tibble: 24 x 2
##    RA_Apes Distance
##      <dbl>    <dbl>
##  1   12.9     20.8 
##  2    6.17    19.8 
##  3    4.91    26.8 
##  4    4.49    18.3 
##  5    4.21     5.14
##  6    3.92    11.2 
##  7    3.78    24.1 
##  8    2.48    17.5 
##  9    1.87     7.14
## 10    1.83     6.61
## # ... with 14 more rows
```

