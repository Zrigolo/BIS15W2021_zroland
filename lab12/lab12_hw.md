---
title: "Lab 12 Homework"
author: "Zabrisky Roland"
date: "2021-03-01"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(ggmap)
library(albersusa)
```

## Load the Data
We will use two separate data sets for this homework.  

1. The first [data set](https://rcweb.dartmouth.edu/~f002d69/workshops/index_rspatial.html) represent sightings of grizzly bears (Ursos arctos) in Alaska.  
2. The second data set is from Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

1. Load the `grizzly` data and evaluate its structure. As part of this step, produce a summary that provides the range of latitude and longitude so you can build an appropriate bounding box.

```r
grizzly <- read_csv(here("lab12", "data", "bear-sightings.csv")) %>% clean_names()
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   bear.id = col_double(),
##   longitude = col_double(),
##   latitude = col_double()
## )
```


```r
summary(grizzly)
```

```
##     bear_id       longitude         latitude    
##  Min.   :   7   Min.   :-166.2   Min.   :55.02  
##  1st Qu.:2569   1st Qu.:-154.2   1st Qu.:58.13  
##  Median :4822   Median :-151.0   Median :60.97  
##  Mean   :4935   Mean   :-149.1   Mean   :61.41  
##  3rd Qu.:7387   3rd Qu.:-145.6   3rd Qu.:64.13  
##  Max.   :9996   Max.   :-131.3   Max.   :70.37
```
<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

2. Use the range of the latitude and longitude to build an appropriate bounding box for your map.

```r
lat <- c(55.02, 60.37)
long <- c(-166.2, -131.3)
base_map_box <- make_bbox(long, lat, f = 0.05)
```
</div>

3. Load a map from `stamen` in a terrain style projection and display the map.

```r
base_map<-get_map(base_map_box, maptype = "terrain", source = "stamen")
```

```
## Source : http://tile.stamen.com/terrain/6/2/18.png
```

```
## Source : http://tile.stamen.com/terrain/6/3/18.png
```

```
## Source : http://tile.stamen.com/terrain/6/4/18.png
```

```
## Source : http://tile.stamen.com/terrain/6/5/18.png
```

```
## Source : http://tile.stamen.com/terrain/6/6/18.png
```

```
## Source : http://tile.stamen.com/terrain/6/7/18.png
```

```
## Source : http://tile.stamen.com/terrain/6/8/18.png
```

```
## Source : http://tile.stamen.com/terrain/6/2/19.png
```

```
## Source : http://tile.stamen.com/terrain/6/3/19.png
```

```
## Source : http://tile.stamen.com/terrain/6/4/19.png
```

```
## Source : http://tile.stamen.com/terrain/6/5/19.png
```

```
## Source : http://tile.stamen.com/terrain/6/6/19.png
```

```
## Source : http://tile.stamen.com/terrain/6/7/19.png
```

```
## Source : http://tile.stamen.com/terrain/6/8/19.png
```

```
## Source : http://tile.stamen.com/terrain/6/2/20.png
```

```
## Source : http://tile.stamen.com/terrain/6/3/20.png
```

```
## Source : http://tile.stamen.com/terrain/6/4/20.png
```

```
## Source : http://tile.stamen.com/terrain/6/5/20.png
```

```
## Source : http://tile.stamen.com/terrain/6/6/20.png
```

```
## Source : http://tile.stamen.com/terrain/6/7/20.png
```

```
## Source : http://tile.stamen.com/terrain/6/8/20.png
```


```r
ggmap(base_map)
```

![](lab12_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

4. Build a final map that overlays the recorded observations of grizzly bears in Alaska.

```r
ggmap(base_map) + 
  geom_point(data = grizzly, aes(longitude, latitude)) +
  labs(x = "Longitude", y = "Latitude", title = "Bear Sghting Locations Locations")
```

```
## Warning: Removed 255 rows containing missing values (geom_point).
```

![](lab12_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

5. Let's switch to the wolves data. Load the data and evaluate its structure.

```r
wolves<-read_csv(here("lab12", "data", "wolves_data","wolves_dataset.csv" )) %>% clean_names()
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   pop = col_character(),
##   age.cat = col_character(),
##   sex = col_character(),
##   color = col_character()
## )
## i Use `spec()` for the full column specifications.
```

```r
summary(wolves)
```

```
##      pop                 year        age_cat              sex           
##  Length:1986        Min.   :1992   Length:1986        Length:1986       
##  Class :character   1st Qu.:2006   Class :character   Class :character  
##  Mode  :character   Median :2011   Mode  :character   Mode  :character  
##                     Mean   :2010                                        
##                     3rd Qu.:2016                                        
##                     Max.   :2019                                        
##                                                                         
##     color                lat             long            habitat       
##  Length:1986        Min.   :33.89   Min.   :-157.84   Min.   :  254.1  
##  Class :character   1st Qu.:44.60   1st Qu.:-123.73   1st Qu.:10375.2  
##  Mode  :character   Median :46.83   Median :-110.99   Median :11211.3  
##                     Mean   :50.43   Mean   :-116.86   Mean   :12797.4  
##                     3rd Qu.:57.89   3rd Qu.:-110.55   3rd Qu.:11860.8  
##                     Max.   :80.50   Max.   : -82.42   Max.   :34676.6  
##                                                                        
##      human          pop_density      pack_size    standard_habitat  
##  Min.   :   0.02   Min.   : 3.74   Min.   :3.55   Min.   :-1.63390  
##  1st Qu.:  80.60   1st Qu.: 7.40   1st Qu.:5.62   1st Qu.:-0.30620  
##  Median :2787.67   Median :11.63   Median :6.37   Median :-0.19650  
##  Mean   :2335.38   Mean   :14.91   Mean   :6.47   Mean   : 0.01158  
##  3rd Qu.:3973.47   3rd Qu.:25.32   3rd Qu.:8.25   3rd Qu.:-0.11130  
##  Max.   :6228.64   Max.   :33.96   Max.   :9.56   Max.   : 2.88180  
##                                                                     
##  standard_human     standard_pop      standard_packsize standard_latitude  
##  Min.   :-0.9834   Min.   :-1.13460   Min.   :-1.7585   Min.   :-1.805900  
##  1st Qu.:-0.9444   1st Qu.:-0.74630   1st Qu.:-0.5418   1st Qu.:-0.636900  
##  Median : 0.3648   Median :-0.29760   Median :-0.1009   Median :-0.392600  
##  Mean   : 0.1461   Mean   : 0.05084   Mean   :-0.0422   Mean   :-0.000006  
##  3rd Qu.: 0.9383   3rd Qu.: 1.15480   3rd Qu.: 1.0041   3rd Qu.: 0.814300  
##  Max.   : 2.0290   Max.   : 2.07150   Max.   : 1.7742   Max.   : 3.281900  
##                                                                            
##  standard_longitude    cav_binary       cdv_binary       cpv_binary    
##  Min.   :-2.144100   Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.:-0.359500   1st Qu.:1.0000   1st Qu.:0.0000   1st Qu.:1.0000  
##  Median : 0.306900   Median :1.0000   Median :0.0000   Median :1.0000  
##  Mean   :-0.000005   Mean   :0.8529   Mean   :0.2219   Mean   :0.7943  
##  3rd Qu.: 0.330200   3rd Qu.:1.0000   3rd Qu.:0.0000   3rd Qu.:1.0000  
##  Max.   : 1.801500   Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
##                      NA's   :321      NA's   :21       NA's   :7       
##    chv_binary       neo_binary      toxo_binary    
##  Min.   :0.0000   Min.   :0.0000   Min.   :0.0000  
##  1st Qu.:1.0000   1st Qu.:0.0000   1st Qu.:0.0000  
##  Median :1.0000   Median :0.0000   Median :0.0000  
##  Mean   :0.8018   Mean   :0.2804   Mean   :0.4832  
##  3rd Qu.:1.0000   3rd Qu.:1.0000   3rd Qu.:1.0000  
##  Max.   :1.0000   Max.   :1.0000   Max.   :1.0000  
##  NA's   :548      NA's   :538      NA's   :827
```

```r
tail(wolves)
```

```
## # A tibble: 6 x 23
##   pop    year age_cat sex   color   lat  long habitat human pop_density
##   <chr> <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>       <dbl>
## 1 YUCH   2018 P       M     B      65.1 -143.  11221.  80.6        9.55
## 2 YUCH   2018 A       M     G      65.1 -143.  11221.  80.6        9.55
## 3 YUCH   2018 A       M     G      65.1 -143.  11221.  80.6        9.55
## 4 YUCH   2018 A       M     G      65.1 -143.  11221.  80.6        9.55
## 5 YUCH   2018 A       F     G      65.1 -143.  11221.  80.6        9.55
## 6 YUCH   2018 S       F     G      65.1 -143.  11221.  80.6        9.55
## # ... with 13 more variables: pack_size <dbl>, standard_habitat <dbl>,
## #   standard_human <dbl>, standard_pop <dbl>, standard_packsize <dbl>,
## #   standard_latitude <dbl>, standard_longitude <dbl>, cav_binary <dbl>,
## #   cdv_binary <dbl>, cpv_binary <dbl>, chv_binary <dbl>, neo_binary <dbl>,
## #   toxo_binary <dbl>
```


```r
names(wolves)
```

```
##  [1] "pop"                "year"               "age_cat"           
##  [4] "sex"                "color"              "lat"               
##  [7] "long"               "habitat"            "human"             
## [10] "pop_density"        "pack_size"          "standard_habitat"  
## [13] "standard_human"     "standard_pop"       "standard_packsize" 
## [16] "standard_latitude"  "standard_longitude" "cav_binary"        
## [19] "cdv_binary"         "cpv_binary"         "chv_binary"        
## [22] "neo_binary"         "toxo_binary"
```


6. How many distinct wolf populations are included in this study? Mae a new object that restricts the data to the wolf populations in the lower 48 US states.

_17 wolf populations are represented in this study._

```r
wolves%>%
  tabyl(pop)
```

```
##      pop   n     percent
##   AK.PEN 100 0.050352467
##  BAN.JAS  96 0.048338369
##       BC 145 0.073011078
##   DENALI 154 0.077542800
##    ELLES  11 0.005538771
##     GTNP  60 0.030211480
##   INT.AK  35 0.017623364
##  MEXICAN 181 0.091137966
##       MI 102 0.051359517
##       MT 351 0.176737160
##    N.NWT  67 0.033736153
##      ONT  60 0.030211480
##    SE.AK  10 0.005035247
##      SNF  92 0.046324270
##   SS.NWT  34 0.017119839
##      YNP 383 0.192849950
##     YUCH 105 0.052870091
```

```r
wolves_lower_48<-wolves%>%
  filter(lat>31&lat<49&long>-125)
head(wolves_lower_48)
```

```
## # A tibble: 6 x 23
##   pop    year age_cat sex   color   lat  long habitat human pop_density
##   <chr> <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>       <dbl>
## 1 GTNP   2012 P       M     G      43.8 -111.  10375. 3924.        34.0
## 2 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0
## 3 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0
## 4 GTNP   2012 P       M     B      43.8 -111.  10375. 3924.        34.0
## 5 GTNP   2013 A       F     G      43.8 -111.  10375. 3924.        34.0
## 6 GTNP   2013 A       M     G      43.8 -111.  10375. 3924.        34.0
## # ... with 13 more variables: pack_size <dbl>, standard_habitat <dbl>,
## #   standard_human <dbl>, standard_pop <dbl>, standard_packsize <dbl>,
## #   standard_latitude <dbl>, standard_longitude <dbl>, cav_binary <dbl>,
## #   cdv_binary <dbl>, cpv_binary <dbl>, chv_binary <dbl>, neo_binary <dbl>,
## #   toxo_binary <dbl>
```


7. Use the `albersusa` package to make a base map of the lower 48 US states.

```r
us_comp <- usa_sf()
```


```r
ggplot() + 
  geom_sf(data = us_comp, size = 0.125) + 
  theme_linedraw()+
  labs(title = "USA Lower 48")
```

![](lab12_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

8. Use the relimited data to plot the distribution of wolf populations in the lower 48 US states.

```r
ggplot() + 
  geom_sf(data = us_comp, size = 0.125) + 
  geom_point(data = wolves_lower_48,aes(long,lat,color=pop),size=2.5)+
  scale_color_brewer(palette = "Set1")+
  labs(title = "Wolf Populations in the Lower 48 States (USA)",x="Longitude",y="Latitude")
```

![](lab12_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

9. What is the average pack size for the wolves in this study by region?

```r
names(wolves_lower_48)
```

```
##  [1] "pop"                "year"               "age_cat"           
##  [4] "sex"                "color"              "lat"               
##  [7] "long"               "habitat"            "human"             
## [10] "pop_density"        "pack_size"          "standard_habitat"  
## [13] "standard_human"     "standard_pop"       "standard_packsize" 
## [16] "standard_latitude"  "standard_longitude" "cav_binary"        
## [19] "cdv_binary"         "cpv_binary"         "chv_binary"        
## [22] "neo_binary"         "toxo_binary"
```


```r
wolves_lower_48 %>% 
  group_by(pop) %>% 
  summarize(mean_packsize= mean(pack_size)) %>% 
  arrange(desc(mean_packsize))
```

```
## # A tibble: 6 x 2
##   pop     mean_packsize
##   <chr>           <dbl>
## 1 YNP              8.25
## 2 GTNP             8.1 
## 3 MI               7.12
## 4 MT               5.62
## 5 SNF              4.81
## 6 MEXICAN          4.04
```

10. Make a new map that shows the distribution of wolves in the lower 48 US states but which has the size of location markers adjusted by pack size.

```r
wolves_lower_48_new<-wolves_lower_48%>%
  group_by(pop)%>%
  mutate(mean_packsize=mean(pack_size))
head(wolves_lower_48_new)
```

```
## # A tibble: 6 x 24
## # Groups:   pop [1]
##   pop    year age_cat sex   color   lat  long habitat human pop_density
##   <chr> <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>       <dbl>
## 1 GTNP   2012 P       M     G      43.8 -111.  10375. 3924.        34.0
## 2 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0
## 3 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0
## 4 GTNP   2012 P       M     B      43.8 -111.  10375. 3924.        34.0
## 5 GTNP   2013 A       F     G      43.8 -111.  10375. 3924.        34.0
## 6 GTNP   2013 A       M     G      43.8 -111.  10375. 3924.        34.0
## # ... with 14 more variables: pack_size <dbl>, standard_habitat <dbl>,
## #   standard_human <dbl>, standard_pop <dbl>, standard_packsize <dbl>,
## #   standard_latitude <dbl>, standard_longitude <dbl>, cav_binary <dbl>,
## #   cdv_binary <dbl>, cpv_binary <dbl>, chv_binary <dbl>, neo_binary <dbl>,
## #   toxo_binary <dbl>, mean_packsize <dbl>
```

```r
ggplot() + 
  geom_sf(data = us_comp, size = 0.125) + 
  geom_point(data = wolves_lower_48_new,aes(long,lat,color=pop,size=mean_packsize))+
  scale_color_brewer(palette = "Set1")+
  labs(title = "Wolf Populations in the Lower 48 States (USA)",x="Longitude",y="Latitude")
```

![](lab12_hw_files/figure-html/unnamed-chunk-20-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
