---
title: "Homework 9"
author: "Netra Patel"
date: "2024-02-20"
output:
  html_document:
    theme: spacelab
    toc: no
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

For this homework, we will take a departure from biological data and use data about California colleges. These data are a subset of the national college scorecard (https://collegescorecard.ed.gov/data/). Load the `ca_college_data.csv` as a new object called `colleges`.

```r
colleges <- readr::read_csv("data/ca_college_data.csv")
```

```
## Rows: 341 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): INSTNM, CITY, STABBR, ZIP
## dbl (6): ADM_RATE, SAT_AVG, PCIP26, COSTT4_A, C150_4_POOLED, PFTFTUG1_EF
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


The variables are a bit hard to decipher, here is a key:  

INSTNM: Institution name  
CITY: California city  
STABBR: Location state  
ZIP: Zip code  
ADM_RATE: Admission rate  
SAT_AVG: SAT average score  
PCIP26: Percentage of degrees awarded in Biological And Biomedical Sciences  
COSTT4_A: Annual cost of attendance  
C150_4_POOLED: 4-year completion rate  
PFTFTUG1_EF: Percentage of undergraduate students who are first-time, full-time degree/certificate-seeking undergraduate students  

1. Use your preferred function(s) to have a look at the data and get an idea of its structure. Make sure you summarize NA's and determine whether or not the data are tidy. You may also consider dealing with any naming issues.


```r
summary(colleges)
```

```
##     INSTNM              CITY              STABBR              ZIP           
##  Length:341         Length:341         Length:341         Length:341        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     ADM_RATE         SAT_AVG         PCIP26           COSTT4_A    
##  Min.   :0.0807   Min.   : 870   Min.   :0.00000   Min.   : 7956  
##  1st Qu.:0.4581   1st Qu.: 985   1st Qu.:0.00000   1st Qu.:12578  
##  Median :0.6370   Median :1078   Median :0.00000   Median :16591  
##  Mean   :0.5901   Mean   :1112   Mean   :0.01981   Mean   :26685  
##  3rd Qu.:0.7461   3rd Qu.:1237   3rd Qu.:0.02457   3rd Qu.:39289  
##  Max.   :1.0000   Max.   :1555   Max.   :0.21650   Max.   :69355  
##  NA's   :240      NA's   :276    NA's   :35        NA's   :124    
##  C150_4_POOLED     PFTFTUG1_EF    
##  Min.   :0.0625   Min.   :0.0064  
##  1st Qu.:0.4265   1st Qu.:0.3212  
##  Median :0.5845   Median :0.5016  
##  Mean   :0.5705   Mean   :0.5577  
##  3rd Qu.:0.7162   3rd Qu.:0.8117  
##  Max.   :0.9569   Max.   :1.0000  
##  NA's   :221      NA's   :53
```


2. Which cities in California have the highest number of colleges?
ANS: LA

```r
colleges%>%
  count(CITY)%>%
  arrange(desc(n))
```

```
## # A tibble: 161 × 2
##    CITY              n
##    <chr>         <int>
##  1 Los Angeles      24
##  2 San Diego        18
##  3 San Francisco    15
##  4 Sacramento       10
##  5 Berkeley          9
##  6 Oakland           9
##  7 Claremont         7
##  8 Pasadena          6
##  9 Fresno            5
## 10 Irvine            5
## # ℹ 151 more rows
```


3. Based on your answer to #2, make a plot that shows the number of colleges in the top 10 cities.


```r
colleges%>%
  count(CITY)%>%
  top_n(10, n)%>%
  ggplot(aes(x = CITY, y = n))+
  geom_col()+
  coord_flip()
```

![](hw9_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

4. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest average cost? Where is it located?

ANS: Claremont

```r
colleges%>%
  group_by(CITY)%>%
  summarise(mean_inst = mean(COSTT4_A , na.rm = T))%>%
  arrange(desc(mean_inst))
```

```
## # A tibble: 161 × 2
##    CITY                mean_inst
##    <chr>                   <dbl>
##  1 Claremont               66498
##  2 Malibu                  66152
##  3 Valencia                64686
##  4 Orange                  64501
##  5 Redlands                61542
##  6 Moraga                  61095
##  7 Atherton                56035
##  8 Thousand Oaks           54373
##  9 Rancho Palos Verdes     50758
## 10 La Verne                50603
## # ℹ 151 more rows
```


5. Based on your answer to #4, make a plot that compares the cost of the individual colleges in the most expensive city. Bonus! Add UC Davis here to see how it compares :>).


```r
colleges%>%
  filter(CITY == "Claremont" & INSTNM == "University of California-Davis")%>%
  ggplot(aes(INSTNM, y = COSTT4_A))+
  geom_col()+
  coord_flip()
```

![](hw9_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


6. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What do you think this means?


```r
colleges%>%
  select(COSTT4_A ,ADM_RATE, C150_4_POOLED)
```

```
## # A tibble: 341 × 3
##    COSTT4_A ADM_RATE C150_4_POOLED
##       <dbl>    <dbl>         <dbl>
##  1     7956       NA        NA    
##  2     8109       NA        NA    
##  3     8278       NA        NA    
##  4     8407       NA        NA    
##  5     8516       NA        NA    
##  6     8577       NA        NA    
##  7     8580       NA         0.233
##  8     9181       NA        NA    
##  9     9281       NA        NA    
## 10     9370       NA        NA    
## # ℹ 331 more rows
```

```r
  ggplot(data = colleges,
         mapping = aes(x = ADM_RATE, y = C150_4_POOLED))+geom_point(na.rm = T)
```

![](hw9_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


7. Is there a relationship between cost and four-year completion rate? (You don't need to do the stats, just produce a plot). What do you think this means?


```r
admission%>%
  ggplot(data = colleges,
         mapping = aes(x = COSTT4_A, y = C150_4_POOLED))+geom_point(na.rm = T)
```

![](hw9_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

8. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Make a new data frame that is restricted to UC institutions. You can remove `Hastings College of Law` and `UC San Francisco` as we are only interested in undergraduate institutions.



```r
univ_calif_final <- colleges%>%
  separate(INSTNM, into = c("UNIV", "CAMPUS"), sep = "-")%>%
  filter(UNIV == "University of California")%>%
  filter(CITY!="San Francisco")
```

```
## Warning: Expected 2 pieces. Additional pieces discarded in 9 rows [140, 145, 165, 173,
## 177, 292, 298, 299, 300].
```

```
## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 264 rows [1, 2, 3, 4, 5,
## 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].
```


Remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

9. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Produce a numerical summary and an appropriate plot.


```r
univ_calif_final%>%
  select(CAMPUS, ADM_RATE)%>%
  arrange(ADM_RATE)
```

```
## # A tibble: 8 × 2
##   CAMPUS        ADM_RATE
##   <chr>            <dbl>
## 1 Berkeley         0.169
## 2 Los Angeles      0.180
## 3 San Diego        0.357
## 4 Santa Barbara    0.358
## 5 Irvine           0.406
## 6 Davis            0.423
## 7 Santa Cruz       0.578
## 8 Riverside        0.663
```


```r
univ_calif_final%>%
  ggplot(aes(x = CAMPUS, y = ADM_RATE))+
    geom_col()+
    coord_flip()
```

![](hw9_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


10. If you wanted to get a degree in biological or biomedical sciences, which campus confers the majority of these degrees? Produce a numerical summary and an appropriate plot.


```r
univ_calif_final%>%
  select(CAMPUS, ADM_RATE, PCIP26)%>%
  arrange(desc(PCIP26))
```

```
## # A tibble: 8 × 3
##   CAMPUS        ADM_RATE PCIP26
##   <chr>            <dbl>  <dbl>
## 1 San Diego        0.357  0.216
## 2 Davis            0.423  0.198
## 3 Santa Cruz       0.578  0.193
## 4 Los Angeles      0.180  0.155
## 5 Riverside        0.663  0.149
## 6 Santa Barbara    0.358  0.108
## 7 Irvine           0.406  0.107
## 8 Berkeley         0.169  0.105
```


```r
univ_calif_final%>%
  ggplot(aes(x = CAMPUS, y = PCIP26))+
  geom_col()
```

![](hw9_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
