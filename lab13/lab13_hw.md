---
title: "Lab 13 Homework"
author: "Zabrisky Roland"
date: "2021-03-02"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
if (!require("tidyverse")) install.packages('tidyverse')
```

```
## Loading required package: tidyverse
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.3     v purrr   0.3.4
## v tibble  3.0.6     v dplyr   1.0.4
## v tidyr   1.1.2     v stringr 1.4.0
## v readr   1.4.0     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


```r
library(tidyverse)
library(shiny)
library(shinydashboard)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Campus = col_character(),
##   Academic_Yr = col_double(),
##   Category = col_character(),
##   Ethnicity = col_character(),
##   `Perc FR` = col_character(),
##   FilteredCountFR = col_double()
## )
```

```r
head(UC_admit)
```

```
## # A tibble: 6 x 6
##   Campus Academic_Yr Category   Ethnicity       `Perc FR` FilteredCountFR
##   <chr>        <dbl> <chr>      <chr>           <chr>               <dbl>
## 1 Davis         2019 Applicants International   21.16%              16522
## 2 Davis         2019 Applicants Unknown         2.51%                1959
## 3 Davis         2019 Applicants White           18.39%              14360
## 4 Davis         2019 Applicants Asian           30.76%              24024
## 5 Davis         2019 Applicants Chicano/Latino  22.44%              17526
## 6 Davis         2019 Applicants American Indian 0.35%                 277
```


**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  

```r
names(UC_admit)
```

```
## [1] "Campus"          "Academic_Yr"     "Category"        "Ethnicity"      
## [5] "Perc FR"         "FilteredCountFR"
```

```r
str(UC_admit)
```

```
## spec_tbl_df [2,160 x 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ Campus         : chr [1:2160] "Davis" "Davis" "Davis" "Davis" ...
##  $ Academic_Yr    : num [1:2160] 2019 2019 2019 2019 2019 ...
##  $ Category       : chr [1:2160] "Applicants" "Applicants" "Applicants" "Applicants" ...
##  $ Ethnicity      : chr [1:2160] "International" "Unknown" "White" "Asian" ...
##  $ Perc FR        : chr [1:2160] "21.16%" "2.51%" "18.39%" "30.76%" ...
##  $ FilteredCountFR: num [1:2160] 16522 1959 14360 24024 17526 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Campus = col_character(),
##   ..   Academic_Yr = col_double(),
##   ..   Category = col_character(),
##   ..   Ethnicity = col_character(),
##   ..   `Perc FR` = col_character(),
##   ..   FilteredCountFR = col_double()
##   .. )
```

```r
glimpse(UC_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ Campus          <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis", ~
## $ Academic_Yr     <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018, ~
## $ Category        <chr> "Applicants", "Applicants", "Applicants", "Applicants"~
## $ Ethnicity       <chr> "International", "Unknown", "White", "Asian", "Chicano~
## $ `Perc FR`       <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.35~
## $ FilteredCountFR <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, 15~
```

```r
naniar::miss_var_summary(UC_admit)
```

```
## # A tibble: 6 x 3
##   variable        n_miss pct_miss
##   <chr>            <int>    <dbl>
## 1 Perc FR              1   0.0463
## 2 FilteredCountFR      1   0.0463
## 3 Campus               0   0     
## 4 Academic_Yr          0   0     
## 5 Category             0   0     
## 6 Ethnicity            0   0
```

```r
summary(UC_admit)
```

```
##     Campus           Academic_Yr     Category          Ethnicity        
##  Length:2160        Min.   :2010   Length:2160        Length:2160       
##  Class :character   1st Qu.:2012   Class :character   Class :character  
##  Mode  :character   Median :2014   Mode  :character   Mode  :character  
##                     Mean   :2014                                        
##                     3rd Qu.:2017                                        
##                     Max.   :2019                                        
##                                                                         
##    Perc FR          FilteredCountFR   
##  Length:2160        Min.   :     1.0  
##  Class :character   1st Qu.:   447.5  
##  Mode  :character   Median :  1837.0  
##                     Mean   :  7142.6  
##                     3rd Qu.:  6899.5  
##                     Max.   :113755.0  
##                     NA's   :1
```







**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**

```r
ui <- dashboardPage(
  dashboardHeader(title = "Admissions by Ethnicity Across the UC System"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select Admission details", choices = c("Campus", "Academic_Yr", "Category"), 
              selected = "Campus"),
      hr(),
      helpText("Source: (https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus.")
  ),
  box(title = "Ethnicity", width = 6,
  plotOutput("plot", width = "600px", height = "500px")
  ) 
  )
  )
)

server <- function(input, output, session) { 
  
  output$plot <- renderPlot({
  UC_admit %>% 
  filter(Ethnicity != "All") %>% 
  ggplot(aes_string(x = input$x,y="FilteredCountFR",fill="Ethnicity"))+
  geom_col(position = "dodge")+
       scale_fill_brewer(palette = "Set1")+
  theme_light(base_size = 18)+
      theme(axis.text.x = element_text(angle = 60, hjust = 1))+
      labs(title = "UC Admission Information",x=NULL,y="Number of Individuals")
  })
  
  
  session$onSessionEnded(stopApp)
  }
shinyApp(ui, server)
```


**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**

```r
ui <- dashboardPage(skin="red",
  dashboardHeader(title = "UC Enrollment"),
  dashboardSidebar(disable = F),
  dashboardBody(selectInput("Campus", " Select Campus:", 
                  choices=unique(UC_admit$Campus)),
  fluidRow(
  box(title = "Plot Options", width = 4,
  selectInput("x", "Student/Applicant Details", choices = c("Ethnicity", "Category","Campus"), 
              selected = "Campus"),
      hr(),
      helpText("Source: (https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus.")
  ), 
  box(title = NULL, width = 6,
  plotOutput("plot", width = "600px", height = "500px")
  ) 
  ) 
  ) 
  )
server <- function(input, output, session) { 
  
  output$plot <- renderPlot({
  UC_admit %>%
      filter(Ethnicity!= "All") %>% 
  ggplot(aes_string(x = "Academic_Yr", y="FilteredCountFR",fill = input$x)) +
  geom_col(position = "dodge")+
       scale_fill_brewer(palette = "Set3")+
  theme_light(base_size = 20)+
     theme(axis.text.x = element_text(angle = 60, hjust = 1))+
      labs(title = "UC Admission Information",x="Academic Year",y="Enrollment")
  })
  
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
