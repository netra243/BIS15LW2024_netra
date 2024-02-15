---
title: "Homework 8"
author: "Netra Patel"
date: "2024-02-15"
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
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches <- readr::read_csv("data/sydneybeaches.csv")
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at /Users/netrapatel/Desktop/BIS15LW2024_netra/BIS15L/BIS15LW2024_netra
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-readr::read_csv(here("lab9", "data", "sydneybeaches.csv")) %>%
  clean_names()
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

ANS: No, they are not tidy because it is not organized in such a way that each variable deos not have its own column. It is in wide format.

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`


```r
sydneybeaches_long <- sydneybeaches%>%
  select(site, date, enterococci_cfu_100ml)
```


4. Pivot the data such that the dates are column names and each beach only appears once (wide format). Name the object `sydneybeaches_wide`


```r
sydneybeaches_wide <- sydneybeaches%>%
  select(site, date, enterococci_cfu_100ml)%>% 
  pivot_wider(names_from = "date",
              values_from = "enterococci_cfu_100ml")
```


5. Pivot the data back so that the dates are data and not column names.


```r
sydneybeaches_wide <- sydneybeaches_long%>%
  pivot_longer(-enterococci_cfu_100ml, 
               names_to = "date", 
               values_to = "site")
```


6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.


```r
data <- sydneybeaches_long%>%
  separate(date, into = c("month","day", "year" ), sep = "/")
```


7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.


```r
data_2 <- data%>%
  group_by(year, site)%>%
  summarise(mean_1 = mean(enterococci_cfu_100ml))
```

```
## `summarise()` has grouped output by 'year'. You can override using the
## `.groups` argument.
```


8. Make the output from question 7 easier to read by pivoting it to wide format.


```r
data_2%>%
  pivot_wider(names_from = "year",
              values_from = "mean_1")
```

```
## # A tibble: 11 × 7
##    site                    `2013` `2014` `2015` `2016` `2017` `2018`
##    <chr>                    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Bondi Beach              32.2   11.1   14.3    19.4  13.2      NA
##  2 Bronte Beach             26.8   17.5   NA      61.3  16.8      NA
##  3 Clovelly Beach            9.28  13.8    8.82   11.3   7.93     NA
##  4 Coogee Beach             39.7   52.6   40.3    NA    20.7      NA
##  5 Gordons Bay (East)       24.8   16.7   36.2    39.0  13.7      NA
##  6 Little Bay Beach        122.    19.5   25.5    31.2  18.2      NA
##  7 Malabar Beach           101.    54.5   66.9    91.0  49.8      NA
##  8 Maroubra Beach           47.1    9.23  14.5    26.6  11.6      NA
##  9 South Maroubra Beach     39.3   14.9    8.25   10.7   8.26     NA
## 10 South Maroubra Rockpool  96.4   40.6   47.3    59.3  46.9      NA
## 11 Tamarama Beach           29.7   39.6   57.0    50.3  20.4      NA
```


9. What was the most polluted beach in 2013?


```r
data_2%>%
  filter(year == "2013")%>%
  arrange(desc(mean_1))
```

```
## # A tibble: 11 × 3
## # Groups:   year [1]
##    year  site                    mean_1
##    <chr> <chr>                    <dbl>
##  1 2013  Little Bay Beach        122.  
##  2 2013  Malabar Beach           101.  
##  3 2013  South Maroubra Rockpool  96.4 
##  4 2013  Maroubra Beach           47.1 
##  5 2013  Coogee Beach             39.7 
##  6 2013  South Maroubra Beach     39.3 
##  7 2013  Bondi Beach              32.2 
##  8 2013  Tamarama Beach           29.7 
##  9 2013  Bronte Beach             26.8 
## 10 2013  Gordons Bay (East)       24.8 
## 11 2013  Clovelly Beach            9.28
```


10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)

ANS: Completed.

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
