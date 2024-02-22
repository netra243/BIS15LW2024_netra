---
title: "dplyr Superhero"
date: "2024-02-01"
output:
  html_document:
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
---

## Learning Goals  
*At the end of this exercise, you will be able to:*    
1. Develop your dplyr superpowers so you can easily and confidently manipulate dataframes.  
2. Learn helpful new functions that are part of the `janitor` package.  

## Instructions
For the second part of lab today, we are going to spend time practicing the dplyr functions we have learned and add a few new ones. This lab doubles as your homework. Please complete the lab and push your final code to GitHub.  

## Load the libraries

```r
library("tidyverse")
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```



```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```



## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- read.csv("data/heroes_information.csv", na = c("", "-99", "-"))
superhero_powers <- read.csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here. Before you do anything, first have a look at the names of the variables. You can use `rename()` or `clean_names()`.    

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, Alignment)
```

```
##  Alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

1. Who are the publishers of the superheros? Show the proportion of superheros from each publisher. Which publisher has the highest number of superheros?  

ANS: Marvel Comics has highest number of superheroes.

```r
superhero_info%>%
  tabyl(Publisher)%>%
  arrange(n)
```

```
##          Publisher   n     percent valid_percent
##      Hanna-Barbera   1 0.001362398   0.001390821
##      J. K. Rowling   1 0.001362398   0.001390821
##   J. R. R. Tolkien   1 0.001362398   0.001390821
##          Microsoft   1 0.001362398   0.001390821
##          Rebellion   1 0.001362398   0.001390821
##         South Park   1 0.001362398   0.001390821
##        Titan Books   1 0.001362398   0.001390821
##  Universal Studios   1 0.001362398   0.001390821
##      Sony Pictures   2 0.002724796   0.002781641
##          Wildstorm   3 0.004087193   0.004172462
##        ABC Studios   4 0.005449591   0.005563282
##     IDW Publishing   4 0.005449591   0.005563282
##        Icon Comics   4 0.005449591   0.005563282
##           Shueisha   4 0.005449591   0.005563282
##               SyFy   5 0.006811989   0.006954103
##       Team Epic TV   5 0.006811989   0.006954103
##      HarperCollins   6 0.008174387   0.008344924
##          Star Trek   6 0.008174387   0.008344924
##       George Lucas  14 0.019073569   0.019471488
##       Image Comics  14 0.019073569   0.019471488
##               <NA>  15 0.020435967            NA
##  Dark Horse Comics  18 0.024523161   0.025034771
##       NBC - Heroes  19 0.025885559   0.026425591
##          DC Comics 215 0.292915531   0.299026426
##      Marvel Comics 388 0.528610354   0.539638387
```

2. Notice that we have some neutral superheros! Who are they? List their names below.  


```r
superhero_info%>%
  select(name, Alignment, Publisher)%>%
  filter(Alignment == "neutral")
```

```
##               name Alignment     Publisher
## 1          Bizarro   neutral     DC Comics
## 2      Black Flash   neutral     DC Comics
## 3     Captain Cold   neutral     DC Comics
## 4          Copycat   neutral Marvel Comics
## 5         Deadpool   neutral Marvel Comics
## 6      Deathstroke   neutral     DC Comics
## 7          Etrigan   neutral     DC Comics
## 8         Galactus   neutral Marvel Comics
## 9        Gladiator   neutral Marvel Comics
## 10          Indigo   neutral     DC Comics
## 11      Juggernaut   neutral Marvel Comics
## 12 Living Tribunal   neutral Marvel Comics
## 13            Lobo   neutral     DC Comics
## 14         Man-Bat   neutral     DC Comics
## 15   One-Above-All   neutral Marvel Comics
## 16           Raven   neutral     DC Comics
## 17        Red Hood   neutral     DC Comics
## 18        Red Hulk   neutral Marvel Comics
## 19        Robin VI   neutral     DC Comics
## 20         Sandman   neutral Marvel Comics
## 21          Sentry   neutral Marvel Comics
## 22        Sinestro   neutral     DC Comics
## 23    The Comedian   neutral     DC Comics
## 24            Toad   neutral Marvel Comics
```


## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?


```r
superhero_info%>%
  select(-name, -Alignment, -Race)
```

```
##     Gender               Eye.color       Hair.color Height         Publisher
## 1     Male                  yellow          No Hair  203.0     Marvel Comics
## 2     Male                    blue          No Hair  191.0 Dark Horse Comics
## 3     Male                    blue          No Hair  185.0         DC Comics
## 4     Male                   green          No Hair  203.0     Marvel Comics
## 5     Male                    blue            Black     NA     Marvel Comics
## 6     Male                    blue          No Hair  193.0     Marvel Comics
## 7     Male                    blue            Blond     NA      NBC - Heroes
## 8     Male                    blue            Blond  185.0         DC Comics
## 9   Female                    blue            Blond  173.0     Marvel Comics
## 10    Male                   brown            Brown  178.0     Marvel Comics
## 11    Male                    <NA>             <NA>  191.0     Marvel Comics
## 12    Male                    blue            White  188.0     Marvel Comics
## 13    Male                   brown            Black  193.0     Marvel Comics
## 14    Male                    blue            Blond  180.0         DC Comics
## 15    Male                    <NA>             <NA>     NA         Wildstorm
## 16    Male                    <NA>             <NA>     NA      NBC - Heroes
## 17    Male                    blue            Black  178.0         DC Comics
## 18    Male                    <NA>          No Hair  244.0 Dark Horse Comics
## 19    Male                    <NA>             <NA>     NA         Wildstorm
## 20    Male                     red             <NA>  257.0         DC Comics
## 21    Male                   brown            Black  188.0     Marvel Comics
## 22    Male                    <NA>             <NA>     NA      NBC - Heroes
## 23    Male                    blue            Blond  183.0     Marvel Comics
## 24    Male                    <NA>             <NA>     NA Dark Horse Comics
## 25  Female                  yellow            Black  165.0     Marvel Comics
## 26  Female                   brown            Black  163.0     Marvel Comics
## 27  Female                    <NA>             <NA>     NA      Image Comics
## 28    Male                    blue            Blond  183.0         DC Comics
## 29    Male                   green          No Hair  180.0     Marvel Comics
## 30    Male                    blue            Blond  211.0     Marvel Comics
## 31    Male                    blue            Blond  183.0     Marvel Comics
## 32    Male                  yellow          No Hair   61.0         DC Comics
## 33    Male                    <NA>             <NA>     NA      Image Comics
## 34    Male                    blue            Blond  229.0     Marvel Comics
## 35    Male                     red            Black  213.0     Marvel Comics
## 36    Male                    blue            Blond     NA         DC Comics
## 37    Male                    blue            Black  178.0         DC Comics
## 38    Male                    blue            Blond  185.0         DC Comics
## 39  Female                    blue            Blond  175.0     Marvel Comics
## 40    Male                    blue            Blond  183.0     Marvel Comics
## 41  Female                  violet           Purple  173.0     Marvel Comics
## 42  Female                   white           Orange  193.0     Marvel Comics
## 43    Male                   brown            Brown  185.0     Marvel Comics
## 44  Female                  purple             Pink  165.0     Marvel Comics
## 45  Female                   black            Black  163.0     Marvel Comics
## 46    Male                    <NA>             <NA>     NA         DC Comics
## 47    Male                   brown            Black     NA              <NA>
## 48    Male                   brown              Red  183.0     Marvel Comics
## 49    Male                    blue            Brown  198.0         DC Comics
## 50    Male                    blue              Red  178.0         DC Comics
## 51    Male                    <NA>             <NA>     NA         DC Comics
## 52  Female                   black            Black  168.0         DC Comics
## 53    Male                   brown           Auburn  183.0         DC Comics
## 54    Male                    <NA>              Red     NA         DC Comics
## 55    Male                   brown            Black     NA         DC Comics
## 56  Female                    blue            Black  180.0     Marvel Comics
## 57    Male                  yellow            Black  183.0     Marvel Comics
## 58    Male                   brown            Black     NA         DC Comics
## 59    Male                    <NA>             <NA>     NA         DC Comics
## 60    Male                    <NA>             <NA>  203.0         DC Comics
## 61    Male                   green Strawberry Blond  183.0     Marvel Comics
## 62    Male                   brown            Black  165.0     Marvel Comics
## 63  Female                    <NA>             <NA>     NA         DC Comics
## 64  Female                   green              Red  170.0         DC Comics
## 65  Female                    <NA>             <NA>     NA         DC Comics
## 66  Female                   green            Black  165.0         DC Comics
## 67  Female                    <NA>             <NA>     NA         DC Comics
## 68  Female                    blue            Blond  168.0         DC Comics
## 69    Male                    blue            black  188.0         DC Comics
## 70    Male                    blue            Black  178.0         DC Comics
## 71    Male                    blue            Black  178.0         DC Comics
## 72    Male                   brown            Black  198.0     Marvel Comics
## 73  Female                   green              Red  178.0         DC Comics
## 74    Male                   black            White  175.0     Marvel Comics
## 75    Male                    blue             Blue  180.0     Marvel Comics
## 76    Male                   green            Green  173.0         DC Comics
## 77    Male                    <NA>             <NA>     NA     Marvel Comics
## 78    Male                    <NA>             <NA>     NA         DC Comics
## 79    Male                    <NA>          No Hair  201.0     Marvel Comics
## 80    Male                    <NA>             <NA>     NA     Marvel Comics
## 81  Female                    blue            Black  188.0         DC Comics
## 82    Male                    <NA>             <NA>     NA       Icon Comics
## 83    Male                    blue            Brown  165.0     Marvel Comics
## 84    Male                    <NA>             <NA>     NA              SyFy
## 85    Male                    <NA>             <NA>     NA      Image Comics
## 86  Female                    blue            Blond  180.0     Marvel Comics
## 87  Female                    blue            Black     NA              <NA>
## 88    <NA>                    <NA>             <NA>     NA     Marvel Comics
## 89    Male                    <NA>             <NA>     NA     Marvel Comics
## 90    Male                    <NA>             <NA>     NA     Marvel Comics
## 91    Male                    <NA>             <NA>     NA     Hanna-Barbera
## 92    Male                   brown          No Hair  198.0     Marvel Comics
## 93    Male                   black            Black  191.0         DC Comics
## 94    Male                     red            Black     NA     Marvel Comics
## 95    Male                   brown            Black  191.0         DC Comics
## 96    Male                    blue            Black  188.0     Marvel Comics
## 97  Female                    blue            Blond  165.0         DC Comics
## 98  Female                    blue            Blond  170.0         DC Comics
## 99  Female                   green            Blond  178.0     Marvel Comics
## 100   Male                    <NA>             <NA>     NA         DC Comics
## 101   Male                    <NA>             <NA>     NA     Marvel Comics
## 102   Male                   brown            Brown  183.0     Marvel Comics
## 103   Male                   brown          No Hair  185.0         DC Comics
## 104 Female                   green            Black  170.0     Marvel Comics
## 105   Male                   black          No Hair  188.0         DC Comics
## 106   Male                   brown            Black  183.0     Marvel Comics
## 107 Female                   green           Auburn  170.0     Marvel Comics
## 108 Female                    blue            Blond  170.0     Marvel Comics
## 109   Male                     red            White  191.0     Marvel Comics
## 110   Male                    blue            Black  185.0     Marvel Comics
## 111   Male                     red            White  188.0     Marvel Comics
## 112   Male                   brown            Black  188.0     Marvel Comics
## 113   <NA>                   black          No Hair     NA     Marvel Comics
## 114 Female                    <NA>             <NA>  168.0     Marvel Comics
## 115 Female                   green          Magenta  165.0     Marvel Comics
## 116   Male                    <NA>             <NA>     NA     Marvel Comics
## 117   Male                    <NA>            Brown     NA     Marvel Comics
## 118   Male                   brown            Brown  175.0     Marvel Comics
## 119   Male                   brown            Brown  178.0     Marvel Comics
## 120 Female                    blue            Brown  218.0     Marvel Comics
## 121   Male                   black          No Hair     NA     Marvel Comics
## 122   Male                   white          No Hair   30.5     Marvel Comics
## 123   Male                    blue            Brown     NA         DC Comics
## 124   Male                    <NA>             <NA>     NA         DC Comics
## 125   Male                    blue            Brown  183.0         DC Comics
## 126   Male                   brown            Black     NA         DC Comics
## 127   Male                   brown            Black  183.0      George Lucas
## 128   Male                    <NA>             <NA>     NA     Marvel Comics
## 129 Female                    <NA>             <NA>     NA      Image Comics
## 130 Female                    blue            Blond  165.0     Marvel Comics
## 131 Female                    <NA>             <NA>     NA     Marvel Comics
## 132   Male                    blue            Blond  196.0         DC Comics
## 133   Male                    <NA>             <NA>     NA     Marvel Comics
## 134   <NA>                    blue            Blond  193.0     Marvel Comics
## 135   <NA>                   brown    Brown / Black     NA     Marvel Comics
## 136   Male                   green          No Hair  198.0         DC Comics
## 137   Male                   green            Blond  170.0         DC Comics
## 138   Male                   brown    Brown / White  183.0     Marvel Comics
## 139   Male                    <NA>             <NA>  193.0              <NA>
## 140 Female                   green            Blond  157.0 Dark Horse Comics
## 141   Male                    blue            blond  183.0     Marvel Comics
## 142 Female                   brown            Black  170.0         DC Comics
## 143   Male                    <NA>             <NA>     NA     Marvel Comics
## 144   Male                    <NA>             <NA>     NA         DC Comics
## 145   Male                    blue            White  203.0     Marvel Comics
## 146 Female                    blue            Black  175.0     Marvel Comics
## 147   Male                    <NA>             <NA>     NA              SyFy
## 148   Male                    blue            Blond  183.0     Marvel Comics
## 149   Male                    blue            blond  188.0     Marvel Comics
## 150   Male                    blue           Silver  193.0         DC Comics
## 151   Male                    blue            Blond  198.0     Marvel Comics
## 152   Male                   brown            Brown     NA         DC Comics
## 153   Male                    blue            Brown  188.0      Team Epic TV
## 154   Male                    <NA>            Black     NA        South Park
## 155   Male                    blue            Blond  188.0     Marvel Comics
## 156 Female                    blue            Blond  180.0     Marvel Comics
## 157   Male                    blue            Black  193.0         DC Comics
## 158   Male                    blue            Black  175.0         DC Comics
## 159   Male                    <NA>             <NA>     NA Dark Horse Comics
## 160   Male                     red            Green     NA     Marvel Comics
## 161   <NA>                    <NA>             <NA>     NA     Marvel Comics
## 162   Male                   green              Red  185.0     Marvel Comics
## 163 Female                    blue            Blond  173.0     Marvel Comics
## 164 Female                    <NA>             <NA>     NA     Marvel Comics
## 165 Female                   green            Black  175.0         DC Comics
## 166   <NA>                   brown            Brown  170.0     Marvel Comics
## 167   Male                   white            White  201.0     Marvel Comics
## 168 Female                    <NA>             <NA>     NA     Marvel Comics
## 169   Male                   brown            Brown  175.0     Marvel Comics
## 170   Male                    <NA>             <NA>     NA         DC Comics
## 171   Male                   brown            Black  180.0     Marvel Comics
## 172 Female                   green            Blond  163.0         DC Comics
## 173 Female                   green            Brown  170.0         DC Comics
## 174 Female                   brown            Brown  175.0         DC Comics
## 175   Male                   brown       Red / Grey  185.0      Team Epic TV
## 176   Male                    <NA>             <NA>  178.0              <NA>
## 177   Male                   green              Red  183.0         DC Comics
## 178 Female                    blue            Blond     NA      NBC - Heroes
## 179   <NA>                    <NA>            White     NA     Marvel Comics
## 180   Male                   brown            black  226.0     Marvel Comics
## 181   Male                    blue            Black  178.0         DC Comics
## 182   Male                    <NA>             <NA>     NA      Image Comics
## 183   Male                    grey            Brown     NA     HarperCollins
## 184   Male                    <NA>             <NA>     NA         DC Comics
## 185   Male                  silver            Black  226.0     Marvel Comics
## 186 Female                     red            White  183.0     Marvel Comics
## 187   Male                   brown            Brown  191.0     Marvel Comics
## 188   Male                   brown            Black  183.0     Marvel Comics
## 189   Male                    blue Strawberry Blond     NA     Marvel Comics
## 190   Male                   brown          No Hair  180.0     Marvel Comics
## 191 Female                   green              Red  168.0     Marvel Comics
## 192   Male                    <NA>             <NA>     NA      Image Comics
## 193   Male                    <NA>             <NA>     NA      Image Comics
## 194   Male                   brown            Black  198.0         DC Comics
## 195   Male                    blue            Black     NA         DC Comics
## 196   Male                   brown            Brown  191.0     Marvel Comics
## 197   <NA>                    blue            Blond  175.0     Marvel Comics
## 198 Female                    blue            Blond  165.0     Marvel Comics
## 199   Male                   brown            Blond     NA     HarperCollins
## 200 Female                    <NA>             <NA>     NA       ABC Studios
## 201   Male                    blue              Red  183.0     Marvel Comics
## 202   Male                   brown            Brown  185.0     Marvel Comics
## 203   Male                    <NA>             <NA>     NA Universal Studios
## 204   Male                     red          No Hair  267.0         DC Comics
## 205   <NA>                    <NA>             <NA>     NA              <NA>
## 206 Female                   brown            Blond  168.0     Marvel Comics
## 207   Male            yellow / red             <NA>  170.0      George Lucas
## 208   Male                  yellow          No Hair  198.0      George Lucas
## 209   Male                    blue            Blond  122.0 Dark Horse Comics
## 210   Male                  yellow            Brown     NA         Star Trek
## 211 Female                    blue            Blond  173.0     Marvel Comics
## 212   Male                    blue            Black  183.0         DC Comics
## 213   Male                   brown          No Hair  188.0     Marvel Comics
## 214   Male                   brown            Brown  185.0         DC Comics
## 215   Male                   brown             Grey  193.0     Marvel Comics
## 216   Male                    blue            White  193.0         DC Comics
## 217   Male                     red          No Hair  185.0     Marvel Comics
## 218   Male                    <NA>             <NA>  188.0     Marvel Comics
## 219   Male                   brown            Black  193.0     Marvel Comics
## 220   Male                    <NA>             <NA>     NA      NBC - Heroes
## 221   Male                    blue            Green  198.0     Marvel Comics
## 222   Male                   brown            Brown  201.0     Marvel Comics
## 223   Male                   brown            Brown  201.0     Marvel Comics
## 224   Male                    blue            Blond  188.0         DC Comics
## 225   Male                   brown            Brown  175.0     Marvel Comics
## 226   Male                    grey            Black  188.0     Marvel Comics
## 227 Female                    blue            Black  173.0     Marvel Comics
## 228   Male                   green          No Hair     NA    IDW Publishing
## 229 Female                    blue            Black  175.0         DC Comics
## 230   Male                     red            White  244.0         DC Comics
## 231   Male                   white          No Hair  196.0     Marvel Comics
## 232   Male                  yellow          No Hair  185.0     Marvel Comics
## 233   Male                   white          No Hair     NA         DC Comics
## 234   Male                     red          No Hair  193.0     Marvel Comics
## 235   <NA>                    <NA>             <NA>     NA     Marvel Comics
## 236 Female                   brown            Brown  168.0 Dark Horse Comics
## 237   Male                    blue           Auburn  180.0     Marvel Comics
## 238 Female                    blue            Black  175.0     Marvel Comics
## 239 Female                    blue            Blond     NA      NBC - Heroes
## 240   Male                    blue              Red  185.0         DC Comics
## 241 Female                    blue            Blond  178.0     Marvel Comics
## 242 Female                    blue            Blond  168.0         DC Comics
## 243 Female                    <NA>             <NA>     NA     HarperCollins
## 244   Male                    <NA>             <NA>     NA         DC Comics
## 245   Male                   brown            Brown  168.0              <NA>
## 246   Male                     red          No Hair  193.0         DC Comics
## 247   Male                   white              Red  188.0     Marvel Comics
## 248   Male                     red            Black  191.0     Marvel Comics
## 249   Male                    blue            Black  183.0     Marvel Comics
## 250   <NA>                    blue            Brown  196.0     Marvel Comics
## 251   Male                   brown            Black  188.0     Marvel Comics
## 252   Male                   black             Blue     NA     Marvel Comics
## 253 Female                    <NA>             <NA>     NA         DC Comics
## 254   <NA> yellow (without irises)   Orange / White  175.0     Marvel Comics
## 255 Female                    <NA>              Red     NA         DC Comics
## 256   Male                     red          No Hair  975.0     Marvel Comics
## 257 Female                   brown            Black  165.0     Marvel Comics
## 258   <NA>                   white           Yellow  193.0     Marvel Comics
## 259 Female                   green              Red  173.0     Marvel Comics
## 260   Male                   brown            Black     NA         DC Comics
## 261   Male                    blue           Auburn  188.0         DC Comics
## 262   <NA>                     red          No Hair     NA     Marvel Comics
## 263   Male                    blue    Brown / White  180.0         DC Comics
## 264   Male                    <NA>             <NA>     NA              <NA>
## 265   Male                    blue            Blond  183.0         DC Comics
## 266   Male                    <NA>             <NA>  183.0         DC Comics
## 267   Male                  yellow           Auburn  157.0         DC Comics
## 268   <NA>                   brown            Black  183.0     Marvel Comics
## 269   Male                    blue            Blond  142.0     Marvel Comics
## 270   <NA>                    blue             Grey  188.0     Marvel Comics
## 271 Female                   brown            Black  211.0     Marvel Comics
## 272 Female                    blue            White  180.0     Marvel Comics
## 273   Male                   black            Black  876.0     Marvel Comics
## 274   Male                     red            Brown  185.0     Marvel Comics
## 275 Female                  yellow            Black  183.0     Marvel Comics
## 276   Male                    <NA>             <NA>     NA         DC Comics
## 277   Male                    <NA>             <NA>     NA              SyFy
## 278   Male                   black            Black     NA         DC Comics
## 279   Male                    blue            Blond  185.0     Marvel Comics
## 280   Male                     red          No Hair  188.0     Marvel Comics
## 281   <NA>                    <NA>             <NA>     NA     Marvel Comics
## 282   Male                    <NA>             <NA>     NA     Marvel Comics
## 283   Male                    <NA>             <NA>     NA     Marvel Comics
## 284 Female                   green              Red   62.5         DC Comics
## 285   Male                    blue             Blue  198.0     Marvel Comics
## 286 Female                   green              Red  168.0     Marvel Comics
## 287   <NA>                    <NA>             <NA>  108.0              <NA>
## 288   Male                    <NA>             <NA>     NA         DC Comics
## 289   Male                    <NA>             <NA>  175.0          Shueisha
## 290   Male                    <NA>             <NA>     NA     Marvel Comics
## 291   Male                    <NA>             <NA>     NA     Marvel Comics
## 292   Male                    <NA>             <NA>     NA     Marvel Comics
## 293   Male                   brown            Black  183.0     Marvel Comics
## 294   Male                  yellow            Black  198.0         DC Comics
## 295 Female                    blue            White  178.0         DC Comics
## 296   Male                    blue            Brown  178.0     Marvel Comics
## 297   Male                  purple             <NA>  170.0      George Lucas
## 298   Male                   green            Blond  188.0         DC Comics
## 299   Male                    blue           Auburn  180.0     Marvel Comics
## 300   Male                    blue           Auburn  178.0     Marvel Comics
## 301   Male                    <NA>             <NA>  183.0     Marvel Comics
## 302   Male                   green            Brown  178.0     Marvel Comics
## 303   Male                  yellow             <NA>  701.0     Marvel Comics
## 304   Male                   brown            Black     NA     Marvel Comics
## 305   Male                    blue              Red  188.0         DC Comics
## 306   Male                   brown            Brown  188.0         DC Comics
## 307   Male                   brown            Brown  183.0      George Lucas
## 308   Male                   brown            Black  188.0     Sony Pictures
## 309 Female                    blue            Blond  170.0         DC Comics
## 310   Male                   green            Black     NA     J. K. Rowling
## 311   Male                    blue            Blond  183.0     Marvel Comics
## 312   Male                     red            Brown  185.0         DC Comics
## 313   Male                    blue            Blond  191.0     Marvel Comics
## 314 Female                    blue            Black  165.0     Marvel Comics
## 315 Female                   green              Red  175.0         DC Comics
## 316   Male                    blue            Brown  185.0         DC Comics
## 317 Female                   green              Red  175.0         DC Comics
## 318 Female                    <NA>             <NA>     NA         DC Comics
## 319 Female                    blue              Red  170.0         DC Comics
## 320   Male                    blue          No Hair  180.0         DC Comics
## 321 Female                   green            Black  213.0     Marvel Comics
## 322   Male                    gold            Black  259.0 Dark Horse Comics
## 323 Female                    blue              Red  173.0     Marvel Comics
## 324   Male                     red              Red  185.0     Marvel Comics
## 325   Male                    blue            Brown  196.0     Marvel Comics
## 326   Male                    <NA>             <NA>     NA      NBC - Heroes
## 327 Female                    <NA>             <NA>     NA       Icon Comics
## 328   Male                    blue             Grey  180.0     Marvel Comics
## 329 Female                    blue              Red  170.0     Marvel Comics
## 330 Female                   green              Red  168.0     Marvel Comics
## 331   Male                   brown           Yellow   79.0     Marvel Comics
## 332   Male                   green            Green  244.0     Marvel Comics
## 333   Male                    blue            Blond  178.0     Marvel Comics
## 334 Female                    blue            Black  180.0         DC Comics
## 335 Female                    blue            Blond  170.0     Marvel Comics
## 336   Male                   brown            Black  175.0     Marvel Comics
## 337   Male                   brown            Brown  188.0     Marvel Comics
## 338   Male                    blue              Red  183.0     Marvel Comics
## 339   Male                   brown            Brown  173.0     Marvel Comics
## 340   Male                  yellow           Auburn  170.0         DC Comics
## 341   Male                    <NA>             <NA>  183.0      George Lucas
## 342 Female                    <NA>           Purple     NA         DC Comics
## 343   Male                    blue          No Hair  180.0     Marvel Comics
## 344 Female                    blue            Blond  168.0     Marvel Comics
## 345   Male                    blue            Blond  180.0     Marvel Comics
## 346   Male                    blue            Black  198.0     Marvel Comics
## 347   Male                    blue          No Hair     NA     Marvel Comics
## 348 Female                    <NA>             <NA>     NA         DC Comics
## 349   Male                    <NA>             <NA>     NA              <NA>
## 350   Male            blue / white            Brown  155.0     Marvel Comics
## 351   Male                    blue            Brown   71.0 Dark Horse Comics
## 352   Male                    blue            Blond  183.0       Titan Books
## 353   Male                   hazel            Brown  178.0         Star Trek
## 354   Male                  yellow             <NA>  193.0      George Lucas
## 355   Male                    <NA>             <NA>     NA              <NA>
## 356 Female                   green              Red  168.0     Marvel Comics
## 357   Male                    <NA>             <NA>     NA         Star Trek
## 358 Female                    blue            Blond  168.0     Marvel Comics
## 359 Female                    <NA>             <NA>     NA         DC Comics
## 360 Female                   green            Brown     NA         DC Comics
## 361 Female                   brown            Brown  170.0     Marvel Comics
## 362 Female                    <NA>             <NA>     NA      NBC - Heroes
## 363   Male                    blue            Black  188.0     Marvel Comics
## 364   Male                    <NA>             <NA>     NA       ABC Studios
## 365   Male                    <NA>             <NA>     NA       ABC Studios
## 366   Male                    <NA>             <NA>     NA Dark Horse Comics
## 367   Male                    blue            Blond  183.0         DC Comics
## 368   Male                   green            Black  185.0         DC Comics
## 369   Male                   brown            Black  183.0     Marvel Comics
## 370   Male                   green            Green  196.0         DC Comics
## 371 Female                    blue            Black  165.0     Marvel Comics
## 372 Female                     red            Black  165.0     Marvel Comics
## 373   Male                    <NA>             <NA>  188.0         Rebellion
## 374   Male                    blue              Red  287.0     Marvel Comics
## 375   Male                    <NA>             <NA>     NA     Marvel Comics
## 376   Male                   hazel            Brown  178.0     Marvel Comics
## 377 Female                   green            Brown     NA      George Lucas
## 378   Male                   white          No Hair  213.0      George Lucas
## 379   Male                   brown            Brown  191.0     Marvel Comics
## 380   Male                   brown            Brown  173.0         DC Comics
## 381 Female                    <NA>             <NA>     NA         Star Trek
## 382 Female                    <NA>             <NA>     NA              <NA>
## 383   Male                    <NA>            Black     NA         DC Comics
## 384   Male                    blue            Blond     NA       Icon Comics
## 385   Male                   green              Red     NA         DC Comics
## 386   Male                    <NA>             <NA>     NA         DC Comics
## 387   Male                     red          No Hair  244.0         DC Comics
## 388 Female                    blue            Blond     NA         DC Comics
## 389   Male                     red          No Hair  234.0         DC Comics
## 390   Male                  yellow            Black   30.5              <NA>
## 391   Male                   black          No Hair     NA         DC Comics
## 392   Male                    blue          No Hair  201.0     Marvel Comics
## 393   Male                     red          No Hair  188.0     Marvel Comics
## 394   Male                   black          No Hair     NA              <NA>
## 395   Male                   brown            Black  191.0     Marvel Comics
## 396   Male                   brown            Black  183.0     Marvel Comics
## 397   Male                    blue            White   64.0         DC Comics
## 398   Male                   green            Black  180.0         DC Comics
## 399   Male                    <NA>             <NA>     NA      George Lucas
## 400 Female                    <NA>            Black     NA     Marvel Comics
## 401 Female                   brown            Black  175.0     Marvel Comics
## 402   Male                   green          No Hair  178.0     Marvel Comics
## 403   Male                    <NA>             <NA>     NA     Marvel Comics
## 404   Male            green / blue            Black  175.0     Marvel Comics
## 405   Male                    blue          No Hair     NA    IDW Publishing
## 406   Male                   green          No Hair  188.0         DC Comics
## 407 Female                    blue              Red  165.0         DC Comics
## 408   Male                    blue              Red  155.0         DC Comics
## 409   Male                    blue              Red  191.0         DC Comics
## 410   <NA>                  yellow             <NA>  198.0     Marvel Comics
## 411   <NA>                    blue          No Hair     NA     Marvel Comics
## 412 Female                    <NA>             <NA>     NA Dark Horse Comics
## 413   Male                     red          No Hair  203.0     Marvel Comics
## 414   Male                     red            Black  229.0         DC Comics
## 415   Male                   green            Black  193.0     Marvel Comics
## 416   Male                    blue            Blond  188.0     Marvel Comics
## 417   Male                   brown            Black  198.0     Marvel Comics
## 418   Male                    <NA>             <NA>     NA      NBC - Heroes
## 419   Male                    blue            Blond  168.0      George Lucas
## 420 Female                    <NA>             <NA>     NA     Marvel Comics
## 421 Female                   green            Green     NA     Marvel Comics
## 422   Male                   brown            Brown  180.0     Marvel Comics
## 423   <NA>                     red            Black  183.0     Marvel Comics
## 424   Male                    grey            White  188.0     Marvel Comics
## 425   Male                    blue            Blond     NA         DC Comics
## 426   Male                   black             <NA>  183.0     Marvel Comics
## 427   <NA>                    blue           Silver     NA      Image Comics
## 428   Male                   brown            Brown     NA         DC Comics
## 429   Male                     red          No Hair  213.0     Marvel Comics
## 430   Male                   brown           Auburn  188.0     Marvel Comics
## 431   Male                    blue            White  188.0     Marvel Comics
## 432 Female                   green            Black  168.0     Marvel Comics
## 433   Male                     red          No Hair  201.0         DC Comics
## 434 Female                   green              Red  170.0     Marvel Comics
## 435   Male                    blue            Black  183.0      Team Epic TV
## 436   Male                   brown            Brown  213.0         Microsoft
## 437   Male                   black            Black     NA         DC Comics
## 438   Male                    <NA>             <NA>     NA      NBC - Heroes
## 439   Male                    blue            Black  193.0     Marvel Comics
## 440 Female                   brown              Red  180.0         DC Comics
## 441 Female                    <NA>             <NA>     NA      NBC - Heroes
## 442 Female                   green              Red  180.0     Marvel Comics
## 443 Female                    blue            Blond  165.0     Marvel Comics
## 444   Male                   white            Black  198.0     Marvel Comics
## 445 Female                    blue              Red  175.0         DC Comics
## 446   Male                   green            Brown  196.0         DC Comics
## 447   Male                   black          No Hair  185.0         DC Comics
## 448 Female                    <NA>             <NA>     NA     Marvel Comics
## 449   Male                    blue            Black  185.0         DC Comics
## 450   Male                   brown            Black     NA      NBC - Heroes
## 451   Male                    blue             <NA>     NA    IDW Publishing
## 452   Male                    grey            Brown  183.0         DC Comics
## 453   Male                   brown            Brown  188.0     Marvel Comics
## 454 Female                    <NA>             <NA>     NA         Wildstorm
## 455 Female                    blue              Red     NA         DC Comics
## 456 Female                     red              Red  178.0         DC Comics
## 457   Male                   brown            Brown  185.0     Marvel Comics
## 458   Male                    <NA>             <NA>  183.0         DC Comics
## 459   Male                    blue            Brown     NA     Marvel Comics
## 460   Male                    <NA>             <NA>     NA         DC Comics
## 461   Male                     red            Black  196.0     Marvel Comics
## 462   Male                    blue            Blond     NA         DC Comics
## 463 Female                    blue            Blond  175.0     Marvel Comics
## 464   Male                   white           Brownn  366.0     Marvel Comics
## 465   Male                    <NA>             <NA>     NA         DC Comics
## 466   Male                    <NA>             <NA>     NA      NBC - Heroes
## 467   Male                    <NA>             <NA>     NA         DC Comics
## 468   Male                    gold             Gold  196.0     Marvel Comics
## 469   Male                    blue            White  193.0         DC Comics
## 470 Female                    <NA>             <NA>     NA      NBC - Heroes
## 471   Male                   brown            Brown  188.0     Marvel Comics
## 472 Female                    blue            Blond  180.0     Marvel Comics
## 473   Male             white / red            Black  188.0     Marvel Comics
## 474   Male                   white          No Hair  178.0     Marvel Comics
## 475   Male                   brown            Black  175.0     Marvel Comics
## 476   Male                    blue            Blond  188.0     Marvel Comics
## 477   Male                    blue            Blond  201.0 Dark Horse Comics
## 478 Female                    blue              Red  173.0     Marvel Comics
## 479   Male                    blue            Brown  180.0     Marvel Comics
## 480   Male                   brown          No Hair  180.0     Marvel Comics
## 481 Female yellow (without irises)     Red / Orange  178.0     Marvel Comics
## 482   Male                    <NA>             <NA>     NA     Marvel Comics
## 483   Male                    grey            Black  188.0     Marvel Comics
## 484 Female                    blue            Blond  180.0     Marvel Comics
## 485 Female                    blue            Blond  168.0     Marvel Comics
## 486   Male                    <NA>             <NA>  168.0          Shueisha
## 487   Male                   brown             <NA>     NA      NBC - Heroes
## 488 Female                    blue          No Hair  185.0     Marvel Comics
## 489 Female                   black            Black     NA     Marvel Comics
## 490   Male                   brown    Brown / White  185.0     Marvel Comics
## 491   Male                  yellow           Indigo  175.0     Marvel Comics
## 492   Male                    blue            Black  178.0         DC Comics
## 493 Female                    blue            Blond     NA      NBC - Heroes
## 494 Female                    <NA>             <NA>     NA              SyFy
## 495   Male                    <NA>             <NA>     NA         DC Comics
## 496   Male                    blue            Black  180.0     Marvel Comics
## 497   Male                   brown            Brown  185.0     Marvel Comics
## 498 Female                   white              Red  163.0     Marvel Comics
## 499   Male                    blue            White  206.0     Marvel Comics
## 500   Male                    <NA>             <NA>     NA         DC Comics
## 501   Male                     red            Blond  211.0     Marvel Comics
## 502   Male                   brown            Black  180.0      Team Epic TV
## 503   Male                    <NA>          No Hair  175.0          Shueisha
## 504   <NA>                    <NA>             <NA>     NA     Marvel Comics
## 505   Male                     red          No Hair  305.0     Marvel Comics
## 506 Female                    blue              Red  178.0         DC Comics
## 507   Male                   brown            Brown     NA         DC Comics
## 508   Male                    <NA>             <NA>     NA      Image Comics
## 509   Male                    blue            Blond     NA         DC Comics
## 510   <NA>                    <NA>             <NA>     NA         DC Comics
## 511   Male                    <NA>             <NA>  170.0     Sony Pictures
## 512   <NA>                    <NA>             <NA>     NA     Marvel Comics
## 513 Female                    <NA>             <NA>     NA     Marvel Comics
## 514   Male                    blue            Blond  183.0     Marvel Comics
## 515   Male                    blue            Black  157.0         DC Comics
## 516   Male                    <NA>             <NA>     NA      NBC - Heroes
## 517   Male                    <NA>             <NA>     NA         DC Comics
## 518 Female                    blue            Black  168.0         DC Comics
## 519 Female                   green              Red  168.0     Marvel Comics
## 520   Male                   green             Grey  183.0     Marvel Comics
## 521   Male                    <NA>             <NA>     NA         DC Comics
## 522   Male                    blue            Black  185.0         DC Comics
## 523 Female                    blue              Red  168.0         DC Comics
## 524 Female                   green              Red  168.0         DC Comics
## 525 Female                   green            Green  170.0     Marvel Comics
## 526 Female                    blue            blond  180.0         DC Comics
## 527   Male                    <NA>             <NA>     NA     Marvel Comics
## 528   Male                    <NA>             <NA>  213.0 Dark Horse Comics
## 529   Male                    blue          No Hair  183.0     Marvel Comics
## 530   Male                    blue Strawberry Blond  180.0         DC Comics
## 531   Male                   green            Blond     NA     Marvel Comics
## 532 Female                    blue           Purple  180.0     Marvel Comics
## 533   Male                    blue            Black  183.0     Marvel Comics
## 534   Male                  purple           Purple  180.0     Marvel Comics
## 535   Male                    blue            Blond  178.0     Marvel Comics
## 536   Male                    <NA>             <NA>     NA         Star Trek
## 537   Male                    <NA>             <NA>     NA     HarperCollins
## 538   Male                    blue            Blond  188.0         DC Comics
## 539   Male                    blue           Silver  183.0     Marvel Comics
## 540   Male                   brown            Brown  163.0     Marvel Comics
## 541   Male                   green             Grey  193.0         DC Comics
## 542 Female                    <NA>             <NA>     NA              SyFy
## 543   Male                   brown            Black  178.0              <NA>
## 544   Male                    <NA>          No Hair     NA    IDW Publishing
## 545 Female                  indigo            Black  165.0         DC Comics
## 546   Male                   green              Red  178.0         DC Comics
## 547   Male                    blue          No Hair  191.0     Marvel Comics
## 548   Male                   green              Red  180.0         DC Comics
## 549   Male                    blue            Black  183.0         DC Comics
## 550   Male                  yellow            Black  213.0     Marvel Comics
## 551   Male                    <NA>             <NA>     NA       Icon Comics
## 552   Male                    blue            Black  165.0         DC Comics
## 553   Male                    blue          No Hair  188.0     Marvel Comics
## 554   Male                   green          No Hair  185.0         DC Comics
## 555   Male                    <NA>             <NA>     NA      Image Comics
## 556   Male                    <NA>             <NA>     NA      Image Comics
## 557 Female                    <NA>             <NA>     NA     HarperCollins
## 558 Female                   hazel            Brown  297.0      George Lucas
## 559   Male                   brown            Brown  196.0     Marvel Comics
## 560   Male                    blue            Brown  185.0         DC Comics
## 561   Male                    <NA>             <NA>     NA         DC Comics
## 562   Male                    blue            Blond     NA         DC Comics
## 563 Female                   green            Black  180.0     Marvel Comics
## 564   Male                    blue            Black  178.0         DC Comics
## 565   Male                    blue              Red  183.0         DC Comics
## 566   Male                    blue            Black  165.0         DC Comics
## 567   Male                    blue            Black  137.0         DC Comics
## 568 Female                   green              Red     NA         DC Comics
## 569   Male                   brown            Brown  122.0     Marvel Comics
## 570 Female                   green    Brown / White  173.0     Marvel Comics
## 571   Male                    blue            Blond  191.0     Marvel Comics
## 572   Male                    blue              Red  168.0         DC Comics
## 573   Male                   amber            Blond  198.0     Marvel Comics
## 574 Female                    blue            Black  170.0     Marvel Comics
## 575   Male                   brown            Brown  185.0     Marvel Comics
## 576   Male                     red           Orange  305.0     Marvel Comics
## 577   Male                    <NA>             <NA>  279.0  J. R. R. Tolkien
## 578   Male                    <NA>             <NA>     NA      Image Comics
## 579   Male                    blue            Brown  183.0         DC Comics
## 580   Male                    blue            Blond  178.0     Marvel Comics
## 581   Male                   brown            Brown  193.0     Marvel Comics
## 582 Female                    blue            Brown  170.0     Marvel Comics
## 583 Female                   green              Red     NA     Marvel Comics
## 584   Male                   brown            Brown  211.0     Marvel Comics
## 585   Male                    <NA>             <NA>     NA     Marvel Comics
## 586   Male                    blue            Blond  188.0     Marvel Comics
## 587   <NA>                     red             <NA>  185.0     Marvel Comics
## 588 Female                   black            Black  173.0         DC Comics
## 589 Female                   hazel            Brown  168.0     Marvel Comics
## 590   Male                   brown            Black  178.0     Marvel Comics
## 591   Male                   brown              Red  191.0     Marvel Comics
## 592 Female                   green            Green  201.0     Marvel Comics
## 593 Female                    blue          No Hair  183.0     Marvel Comics
## 594   Male                   brown            Brown  175.0     Marvel Comics
## 595 Female           yellow / blue            Black  173.0     Marvel Comics
## 596 Female                    <NA>             <NA>     NA         DC Comics
## 597 Female                    blue            Black  188.0     Marvel Comics
## 598 Female                   brown            Black     NA     Marvel Comics
## 599 Female                    <NA>             <NA>     NA         DC Comics
## 600 Female                    <NA>             <NA>     NA         DC Comics
## 601   Male                   white          No Hair  193.0     Marvel Comics
## 602 Female                   brown            Black  157.0     Marvel Comics
## 603   Male                    bown            Black     NA         DC Comics
## 604   Male                   black            Black  201.0         DC Comics
## 605 Female                    blue           Purple  175.0         DC Comics
## 606 Female                   black             <NA>     NA         DC Comics
## 607 Female                    blue Strawberry Blond  168.0     Marvel Comics
## 608   Male                   green            Black  198.0     Marvel Comics
## 609   Male                    <NA>             <NA>     NA     Marvel Comics
## 610 Female                   white            Blond  178.0     Marvel Comics
## 611   Male                   white          No Hair     NA         DC Comics
## 612   Male                   black            White  279.0         DC Comics
## 613 Female                   green      Red / White  165.0     Marvel Comics
## 614   Male                    <NA>             <NA>  188.0         DC Comics
## 615   Male                   brown            Black  211.0      Image Comics
## 616   Male                   white          No Hair     NA         DC Comics
## 617   Male                    <NA>             <NA>     NA     Marvel Comics
## 618   Male                    <NA>             <NA>     NA         DC Comics
## 619 Female                   green            Brown     NA         DC Comics
## 620   Male                    <NA>             <NA>     NA     Marvel Comics
## 621 Female                    blue            Brown  170.0     Marvel Comics
## 622 Female                    blue            Blond  165.0     Marvel Comics
## 623   Male                   hazel            Brown  178.0     Marvel Comics
## 624   <NA>                     red            Brown  178.0     Marvel Comics
## 625   Male                   brown            Black  157.0     Marvel Comics
## 626 Female                   green            Black  178.0     Marvel Comics
## 627 Female                    <NA>             <NA>     NA     Marvel Comics
## 628 Female                   brown            Brown  173.0     Marvel Comics
## 629 Female                     red            White  178.0     Marvel Comics
## 630   Male                   brown            Black  185.0         Star Trek
## 631   Male                   brown            Blond  183.0     Marvel Comics
## 632 Female                    <NA>             <NA>     NA     Marvel Comics
## 633   Male                    blue            Blond  188.0     Marvel Comics
## 634   Male                    <NA>             <NA>     NA     Marvel Comics
## 635 Female                   green           Auburn  193.0         DC Comics
## 636 Female                    blue            Blond  165.0         DC Comics
## 637   Male                   brown            Black  170.0         DC Comics
## 638   Male                   brown          No Hair  201.0         DC Comics
## 639 Female                    <NA>            Blond     NA       ABC Studios
## 640   Male                     red            Black  183.0         DC Comics
## 641 Female                    blue            White  180.0     Marvel Comics
## 642   Male                    <NA>             <NA>  183.0      George Lucas
## 643   Male                   brown            black  173.0     Marvel Comics
## 644   Male                    blue            Black  170.0         DC Comics
## 645   Male                    blue     Black / Blue  180.0         DC Comics
## 646 Female                    blue            Blond  165.0         DC Comics
## 647   Male                    blue            Black  191.0         DC Comics
## 648   Male                     red          No Hair     NA         DC Comics
## 649   Male                  yellow          No Hair  196.0     Marvel Comics
## 650   Male                    <NA>             <NA>     NA      NBC - Heroes
## 651   Male                   brown            Black  180.0     Marvel Comics
## 652   Male                    <NA>             <NA>  183.0 Dark Horse Comics
## 653   Male                     red             <NA>     NA Dark Horse Comics
## 654   Male                     red             <NA>     NA Dark Horse Comics
## 655 Female                    <NA>             <NA>     NA Dark Horse Comics
## 656   Male                   brown            Brown  188.0     Marvel Comics
## 657 Female                   brown            Black  163.0     Marvel Comics
## 658   Male                     red          No Hair  201.0     Marvel Comics
## 659   Male                    <NA>             <NA>     NA              <NA>
## 660   Male                   brown            Black  188.0         DC Comics
## 661   Male                    blue          No Hair  183.0     Marvel Comics
## 662   Male                    blue            Blond  198.0     Marvel Comics
## 663 Female                    blue            Blond  175.0     Marvel Comics
## 664   Male                   brown            Black  185.0     Marvel Comics
## 665   Male                    <NA>             <NA>     NA     Marvel Comics
## 666   Male                   brown            Black  175.0     Marvel Comics
## 667   Male                    blue            Blond  198.0     Marvel Comics
## 668 Female                   green              Red  218.0     Marvel Comics
## 669   Male                    grey          No Hair  185.0     Marvel Comics
## 670 Female                   green           Auburn  178.0     Marvel Comics
## 671   Male                   brown            White  163.0     Marvel Comics
## 672   Male                    <NA>             <NA>     NA     HarperCollins
## 673   Male                   black            Brown  175.0     Marvel Comics
## 674   Male                    blue            Brown  188.0     Marvel Comics
## 675   Male                   black            Blond  191.0     Marvel Comics
## 676 Female                    <NA>             <NA>     NA      NBC - Heroes
## 677   Male                    blue            Blond  183.0         DC Comics
## 678   Male                  yellow            Black     NA         DC Comics
## 679 Female                  purple            Brown  168.0         DC Comics
## 680   Male                   green          No Hair  188.0     Marvel Comics
## 681   Male                    <NA>             <NA>  183.0         DC Comics
## 682 Female                    blue            Blond  168.0     Marvel Comics
## 683   Male                     red             <NA>  206.0     Marvel Comics
## 684   Male                    blue            White   15.2     Marvel Comics
## 685 Female                    blue Strawberry Blond  168.0     Marvel Comics
## 686 Female                   hazel            Black  175.0      Team Epic TV
## 687 Female                    blue            Blond  191.0     Marvel Comics
## 688   Male                   green          No Hair  165.0     Marvel Comics
## 689   Male                    <NA>            Black  168.0          Shueisha
## 690   Male                    blue Strawberry Blond  191.0     Marvel Comics
## 691   Male                   brown            Black  175.0     Marvel Comics
## 692   Male                   brown            Brown  229.0     Marvel Comics
## 693   Male                    <NA>             <NA>  226.0     Marvel Comics
## 694 Female                    blue           Silver  168.0     Marvel Comics
## 695   Male                   brown            Black  178.0         DC Comics
## 696 Female                   green              Red  165.0     Marvel Comics
## 697   Male                    <NA>             <NA>     NA     Marvel Comics
## 698   Male                    <NA>             <NA>     NA      Image Comics
## 699 Female                  violet            Black  137.0 Dark Horse Comics
## 700   Male                    gold          No Hair  191.0     Marvel Comics
## 701   <NA>                     red          No Hair  191.0     Marvel Comics
## 702 Female                   amber            Black  175.0         DC Comics
## 703   Male                   black            Black     NA     Marvel Comics
## 704   Male                   brown          No Hair  180.0     Marvel Comics
## 705   Male                    blue            Black  183.0     Marvel Comics
## 706   Male                   brown            Brown  185.0     Marvel Comics
## 707 Female                    blue            Blond  180.0     Marvel Comics
## 708   Male                     red            Blond  188.0     Marvel Comics
## 709   Male                   brown            Black  173.0         DC Comics
## 710   Male                   brown            Black  218.0     Marvel Comics
## 711 Female                    blue           Auburn  163.0     Marvel Comics
## 712   Male                    <NA>             <NA>     NA     Marvel Comics
## 713   Male                    <NA>             <NA>     NA     Marvel Comics
## 714 Female                   brown            Black     NA         DC Comics
## 715 Female                    blue            Blond  178.0     Marvel Comics
## 716   Male                    <NA>             <NA>     NA         DC Comics
## 717   Male                   brown            Brown  175.0     Marvel Comics
## 718   <NA>                   brown            Black  140.0     Marvel Comics
## 719 Female                   green           Auburn  366.0     Marvel Comics
## 720   Male                    blue            Black  160.0     Marvel Comics
## 721 Female                    blue            Blond  165.0         DC Comics
## 722   Male                     red            Black  188.0     Marvel Comics
## 723 Female                    blue            Black  183.0         DC Comics
## 724 Female                    <NA>             <NA>     NA     Marvel Comics
## 725   Male                   brown            Black  196.0     Marvel Comics
## 726 Female                   green            Black  155.0     Marvel Comics
## 727   Male                    blue            Brown  175.0     Marvel Comics
## 728   Male                    blue          No Hair  188.0     Marvel Comics
## 729   Male                    blue            Blond  183.0     Marvel Comics
## 730 Female                    blue Strawberry Blond  165.0     Marvel Comics
## 731   Male                   white          No Hair  304.8     Marvel Comics
## 732   Male                   brown            White   66.0      George Lucas
## 733 Female                    blue            Black  170.0         DC Comics
## 734   Male                     red            Brown  185.0         DC Comics
##         Skin.color Weight
## 1             <NA>    441
## 2             blue     65
## 3              red     90
## 4             <NA>    441
## 5             <NA>     NA
## 6             <NA>    122
## 7             <NA>     NA
## 8             <NA>     88
## 9             <NA>     61
## 10            <NA>     81
## 11            <NA>    104
## 12            <NA>    108
## 13            <NA>     90
## 14            <NA>     90
## 15            <NA>     NA
## 16            <NA>     NA
## 17            <NA>     72
## 18           black    169
## 19            <NA>     NA
## 20            <NA>    173
## 21            <NA>    101
## 22            <NA>     NA
## 23            <NA>     68
## 24            <NA>     NA
## 25            <NA>     57
## 26            <NA>     54
## 27            <NA>     NA
## 28            <NA>     83
## 29            <NA>     90
## 30            <NA>    122
## 31            <NA>     86
## 32            <NA>     NA
## 33            <NA>     NA
## 34            <NA>    358
## 35            grey    135
## 36            <NA>     NA
## 37            <NA>    106
## 38            <NA>    146
## 39            <NA>     63
## 40            blue     68
## 41            <NA>     57
## 42            gold     98
## 43            <NA>    270
## 44            <NA>     59
## 45            <NA>     50
## 46            <NA>     NA
## 47            <NA>     NA
## 48            <NA>    101
## 49            <NA>    126
## 50            <NA>     68
## 51            <NA>     NA
## 52            <NA>     54
## 53            <NA>     81
## 54            <NA>     NA
## 55            <NA>     72
## 56            <NA>     63
## 57             red     67
## 58            <NA>     NA
## 59            <NA>     NA
## 60            <NA>    180
## 61            <NA>     77
## 62            <NA>     54
## 63            <NA>     NA
## 64            <NA>     57
## 65            <NA>     NA
## 66            <NA>     52
## 67            <NA>     NA
## 68            <NA>     61
## 69            <NA>     95
## 70            <NA>     77
## 71            <NA>     79
## 72            <NA>    133
## 73            <NA>     NA
## 74            <NA>     63
## 75            blue    181
## 76           green     68
## 77            <NA>     NA
## 78            <NA>     NA
## 79            <NA>    216
## 80            <NA>     NA
## 81            <NA>    135
## 82            <NA>     NA
## 83            <NA>     71
## 84            <NA>     NA
## 85            <NA>     NA
## 86            <NA>     54
## 87            <NA>     NA
## 88            <NA>     NA
## 89            <NA>     NA
## 90            <NA>     NA
## 91            <NA>     NA
## 92            <NA>    124
## 93           white    155
## 94            <NA>     NA
## 95            <NA>    113
## 96            <NA>     95
## 97            <NA>     58
## 98            <NA>     59
## 99            <NA>     54
## 100           <NA>     NA
## 101           <NA>     NA
## 102           <NA>     86
## 103           <NA>     90
## 104           <NA>     52
## 105           <NA>     92
## 106           <NA>     90
## 107           <NA>     59
## 108           <NA>     61
## 109          white    104
## 110           <NA>     86
## 111           <NA>     88
## 112           <NA>     97
## 113           <NA>     NA
## 114           <NA>     68
## 115           pink     56
## 116           <NA>     NA
## 117           <NA>     NA
## 118           <NA>     77
## 119           <NA>    230
## 120           <NA>    495
## 121           <NA>     NA
## 122           <NA>     NA
## 123           <NA>     NA
## 124           <NA>     NA
## 125           <NA>     86
## 126           <NA>     NA
## 127           <NA>     NA
## 128           <NA>     NA
## 129           <NA>     NA
## 130           <NA>     55
## 131           <NA>     NA
## 132           <NA>     97
## 133           <NA>     NA
## 134           <NA>    110
## 135           <NA>     NA
## 136          green    135
## 137           <NA>     61
## 138           <NA>     99
## 139           <NA>     NA
## 140           <NA>     52
## 141           <NA>     90
## 142           <NA>     59
## 143           <NA>     NA
## 144           <NA>     NA
## 145           <NA>    158
## 146           <NA>     74
## 147           <NA>     NA
## 148           <NA>     81
## 149           <NA>    108
## 150         silver     90
## 151           <NA>    116
## 152           <NA>     NA
## 153           <NA>     NA
## 154           <NA>     NA
## 155           <NA>    108
## 156           <NA>     74
## 157           <NA>    101
## 158           <NA>     74
## 159           <NA>     NA
## 160           <NA>     NA
## 161           <NA>     NA
## 162           <NA>     86
## 163           <NA>     61
## 164           <NA>     NA
## 165           <NA>     61
## 166           <NA>     62
## 167           grey     97
## 168           <NA>     NA
## 169           <NA>     63
## 170           <NA>     NA
## 171           <NA>     81
## 172           <NA>     50
## 173           <NA>     55
## 174           <NA>     54
## 175           <NA>     86
## 176           <NA>     NA
## 177           <NA>    170
## 178           <NA>     NA
## 179           <NA>     NA
## 180           <NA>     70
## 181           <NA>     78
## 182           <NA>     NA
## 183           <NA>     NA
## 184           <NA>     NA
## 185           <NA>    225
## 186           blue     67
## 187           <NA>     79
## 188           <NA>     99
## 189           <NA>     NA
## 190           <NA>    104
## 191           <NA>     50
## 192           <NA>     NA
## 193           <NA>     NA
## 194           <NA>    173
## 195           <NA>     NA
## 196           <NA>     88
## 197           <NA>     68
## 198           <NA>     52
## 199           <NA>     NA
## 200           <NA>     NA
## 201           <NA>     90
## 202           <NA>     81
## 203           <NA>     NA
## 204           grey    817
## 205           <NA>     NA
## 206           <NA>     56
## 207    red / black     NA
## 208           <NA>    135
## 209           <NA>     27
## 210           <NA>     NA
## 211           <NA>     52
## 212           <NA>     90
## 213           <NA>     95
## 214           <NA>     91
## 215           <NA>    178
## 216           <NA>    101
## 217           <NA>     95
## 218           <NA>    383
## 219           <NA>     90
## 220           <NA>     NA
## 221           <NA>    171
## 222           <NA>    187
## 223           <NA>    132
## 224           <NA>     89
## 225           <NA>    110
## 226           <NA>     81
## 227          white     54
## 228          green     NA
## 229           <NA>     63
## 230           <NA>    412
## 231           <NA>    104
## 232           <NA>     NA
## 233           blue     NA
## 234          green    306
## 235           <NA>     NA
## 236           <NA>     56
## 237           <NA>     74
## 238           <NA>     59
## 239           <NA>     NA
## 240           <NA>     80
## 241           <NA>     65
## 242           <NA>     57
## 243           <NA>     NA
## 244           <NA>     NA
## 245           <NA>     NA
## 246         yellow    203
## 247           <NA>     95
## 248          green    106
## 249            red     88
## 250           <NA>     96
## 251           <NA>    108
## 252           <NA>     NA
## 253           <NA>     NA
## 254           <NA>     50
## 255           <NA>     NA
## 256          green     18
## 257           <NA>     56
## 258           <NA>     99
## 259           <NA>     56
## 260           <NA>     NA
## 261           <NA>     91
## 262           <NA>     NA
## 263           <NA>     81
## 264           <NA>     NA
## 265           <NA>     88
## 266           <NA>     86
## 267           <NA>     52
## 268           <NA>     81
## 269           <NA>     45
## 270           <NA>     92
## 271           <NA>    104
## 272           <NA>    167
## 273           <NA>     16
## 274           <NA>     81
## 275          green     77
## 276           <NA>     NA
## 277           <NA>     NA
## 278           <NA>     NA
## 279           <NA>     86
## 280           <NA>     99
## 281           <NA>     NA
## 282           <NA>     NA
## 283           <NA>     NA
## 284           <NA>    630
## 285         purple    268
## 286           <NA>     50
## 287           grey     NA
## 288           <NA>     NA
## 289           <NA>     62
## 290           <NA>     NA
## 291           <NA>     NA
## 292           <NA>     NA
## 293           <NA>     90
## 294           <NA>    270
## 295           <NA>    115
## 296           <NA>     79
## 297          green     NA
## 298           <NA>     88
## 299           <NA>     83
## 300           <NA>     77
## 301           <NA>     88
## 302           <NA>     79
## 303           <NA>      4
## 304           <NA>     NA
## 305           <NA>     95
## 306           <NA>     90
## 307           <NA>     79
## 308           <NA>     NA
## 309           <NA>     63
## 310           <NA>     NA
## 311           <NA>     79
## 312           <NA>     89
## 313           <NA>    104
## 314           <NA>     57
## 315           <NA>     61
## 316           <NA>     88
## 317           <NA>     54
## 318           <NA>     NA
## 319           <NA>     65
## 320           <NA>     81
## 321           <NA>    225
## 322           <NA>    158
## 323           <NA>     61
## 324           <NA>     81
## 325           <NA>    146
## 326           <NA>     NA
## 327           <NA>     NA
## 328           <NA>     83
## 329           <NA>     NA
## 330           <NA>     48
## 331           <NA>     18
## 332          green    630
## 333           <NA>     77
## 334           <NA>     59
## 335           <NA>     58
## 336           <NA>     77
## 337           <NA>    119
## 338           <NA>    207
## 339           <NA>     65
## 340           <NA>     65
## 341           <NA>     79
## 342           <NA>     NA
## 343           <NA>     81
## 344           <NA>     54
## 345           <NA>     79
## 346           <NA>    191
## 347           <NA>      2
## 348           <NA>     NA
## 349           <NA>     NA
## 350           <NA>     79
## 351           <NA>     14
## 352           <NA>     NA
## 353           <NA>     77
## 354 orange / white     NA
## 355           <NA>     NA
## 356           <NA>     52
## 357           <NA>     NA
## 358           <NA>     55
## 359           <NA>     NA
## 360           <NA>     NA
## 361           <NA>     56
## 362           <NA>     NA
## 363           <NA>    113
## 364           <NA>     NA
## 365           <NA>     NA
## 366           <NA>     NA
## 367           <NA>     NA
## 368           <NA>     90
## 369           <NA>     88
## 370          white     86
## 371           <NA>     49
## 372           <NA>     52
## 373           <NA>     NA
## 374           <NA>    855
## 375           <NA>     NA
## 376           <NA>     81
## 377           <NA>     NA
## 378           gray     NA
## 379           <NA>    104
## 380           <NA>     72
## 381           <NA>     NA
## 382           <NA>     NA
## 383           <NA>     NA
## 384           <NA>     NA
## 385           <NA>     NA
## 386           <NA>     NA
## 387          green    356
## 388           blue     NA
## 389           pink    324
## 390           <NA>     NA
## 391           <NA>     NA
## 392           <NA>    203
## 393            red     97
## 394            red     NA
## 395           <NA>     99
## 396           <NA>    106
## 397           <NA>     18
## 398           <NA>     79
## 399           <NA>     NA
## 400           <NA>     NA
## 401           <NA>     58
## 402           <NA>     63
## 403           <NA>     NA
## 404           <NA>     59
## 405          green     NA
## 406           <NA>     95
## 407           <NA>     54
## 408           <NA>     65
## 409           <NA>     95
## 410           <NA>    360
## 411           gold     NA
## 412           <NA>     NA
## 413           <NA>    230
## 414     blue-white    288
## 415           <NA>    236
## 416           <NA>     36
## 417           <NA>    191
## 418           <NA>     NA
## 419           <NA>     77
## 420           <NA>     NA
## 421           <NA>     NA
## 422           <NA>     79
## 423           <NA>    383
## 424           <NA>     86
## 425           <NA>     NA
## 426           <NA>     NA
## 427           <NA>     NA
## 428           <NA>     NA
## 429          green    225
## 430           <NA>     90
## 431           <NA>     97
## 432          green     52
## 433          green    135
## 434           <NA>     56
## 435           <NA>     81
## 436           <NA>     NA
## 437           <NA>     NA
## 438           <NA>     NA
## 439           <NA>    110
## 440           <NA>     72
## 441           <NA>     NA
## 442           <NA>     59
## 443           <NA>     54
## 444           <NA>    140
## 445           <NA>     72
## 446           <NA>     90
## 447           <NA>     90
## 448           <NA>     NA
## 449           <NA>     86
## 450           <NA>     NA
## 451          green     NA
## 452           <NA>     77
## 453           <NA>    101
## 454           <NA>     NA
## 455           <NA>     NA
## 456           <NA>     61
## 457           <NA>     81
## 458           <NA>     86
## 459           <NA>     NA
## 460           <NA>     NA
## 461           <NA>    128
## 462           <NA>     NA
## 463           <NA>     61
## 464           <NA>    338
## 465           <NA>     NA
## 466           <NA>     NA
## 467           <NA>     NA
## 468           <NA>    248
## 469           <NA>     90
## 470           <NA>     NA
## 471           <NA>    101
## 472           <NA>     59
## 473           <NA>     79
## 474           <NA>     79
## 475           <NA>     72
## 476           <NA>     70
## 477           <NA>    158
## 478           <NA>     61
## 479           <NA>     70
## 480           <NA>     79
## 481           blue     54
## 482           <NA>     NA
## 483           <NA>    125
## 484           <NA>     85
## 485           <NA>    101
## 486           <NA>     54
## 487           <NA>     NA
## 488           blue     83
## 489           <NA>     NA
## 490           <NA>     99
## 491           <NA>     88
## 492           <NA>     79
## 493           <NA>     NA
## 494           <NA>     NA
## 495           <NA>     NA
## 496           <NA>     83
## 497           <NA>     86
## 498           gold     59
## 499           <NA>    293
## 500           <NA>     NA
## 501           <NA>    191
## 502           <NA>     65
## 503           <NA>     69
## 504           <NA>     NA
## 505           <NA>    405
## 506           <NA>     59
## 507           <NA>     NA
## 508           <NA>     NA
## 509           <NA>     NA
## 510           <NA>     NA
## 511           <NA>    117
## 512           <NA>     NA
## 513           <NA>     NA
## 514           <NA>     89
## 515           <NA>     79
## 516           <NA>     NA
## 517           <NA>     NA
## 518           <NA>     54
## 519           <NA>     52
## 520           <NA>     87
## 521           <NA>     NA
## 522           <NA>     80
## 523           <NA>     55
## 524          green     50
## 525           <NA>     52
## 526           <NA>     81
## 527           <NA>     NA
## 528           <NA>    234
## 529           <NA>     86
## 530           <NA>     81
## 531           <NA>     NA
## 532           <NA>     70
## 533           <NA>     90
## 534         purple     74
## 535           <NA>     68
## 536           <NA>     NA
## 537           <NA>     NA
## 538           <NA>     83
## 539           <NA>     79
## 540           <NA>     56
## 541           <NA>     97
## 542           <NA>     NA
## 543           <NA>     83
## 544          green     NA
## 545           <NA>     50
## 546           <NA>     70
## 547           <NA>    117
## 548           <NA>     83
## 549           <NA>     81
## 550            red    630
## 551           <NA>     NA
## 552           <NA>     56
## 553           <NA>    108
## 554           <NA>    146
## 555           <NA>     NA
## 556           <NA>     NA
## 557           <NA>     NA
## 558           <NA>     NA
## 559           <NA>    320
## 560           <NA>     85
## 561           <NA>     NA
## 562           <NA>     NA
## 563           <NA>     72
## 564           <NA>     79
## 565           <NA>    101
## 566           <NA>     56
## 567           <NA>     38
## 568           <NA>     NA
## 569           <NA>     25
## 570           <NA>     54
## 571           <NA>    104
## 572           <NA>     63
## 573           <NA>    171
## 574           <NA>     61
## 575           <NA>    203
## 576           <NA>    900
## 577           <NA>     NA
## 578           <NA>     NA
## 579           <NA>     63
## 580           <NA>     74
## 581           <NA>    113
## 582           <NA>     59
## 583           <NA>     NA
## 584           <NA>    310
## 585           <NA>     NA
## 586           <NA>     87
## 587           <NA>    149
## 588           blue     54
## 589           <NA>     50
## 590           <NA>     79
## 591           <NA>     88
## 592           <NA>    315
## 593           <NA>    153
## 594           <NA>     79
## 595           <NA>     52
## 596           <NA>     NA
## 597           <NA>    191
## 598           <NA>     NA
## 599           <NA>     NA
## 600           <NA>     NA
## 601         silver    101
## 602           <NA>     50
## 603           <NA>     NA
## 604            red     92
## 605           <NA>     72
## 606           <NA>     NA
## 607           <NA>     52
## 608           <NA>    180
## 609           <NA>     NA
## 610           <NA>     49
## 611           <NA>     NA
## 612           <NA>    437
## 613           <NA>     65
## 614           <NA>    113
## 615           <NA>    405
## 616          white     NA
## 617           <NA>     NA
## 618           <NA>     NA
## 619           <NA>     NA
## 620           <NA>     NA
## 621           <NA>     54
## 622           <NA>     56
## 623           <NA>     74
## 624           <NA>     77
## 625           <NA>     56
## 626           <NA>     59
## 627           <NA>     NA
## 628           <NA>     55
## 629           <NA>     58
## 630           <NA>     81
## 631           <NA>     83
## 632           <NA>     NA
## 633           <NA>     79
## 634           <NA>     NA
## 635         orange     71
## 636           <NA>     62
## 637           <NA>     63
## 638           <NA>    131
## 639           <NA>     NA
## 640          white     91
## 641           <NA>     57
## 642           <NA>     NA
## 643           <NA>     77
## 644           <NA>     68
## 645           <NA>     77
## 646           <NA>     54
## 647           <NA>    101
## 648          green     NA
## 649         yellow     47
## 650           <NA>     NA
## 651           <NA>     74
## 652         silver    146
## 653           <NA>    176
## 654           <NA>    198
## 655         silver    149
## 656           <NA>     99
## 657           <NA>     54
## 658         purple    443
## 659           <NA>     NA
## 660           <NA>    101
## 661           <NA>    225
## 662           <NA>    288
## 663           <NA>    143
## 664           <NA>    101
## 665           <NA>     NA
## 666           <NA>     74
## 667           <NA>    288
## 668           <NA>    158
## 669           grey    203
## 670           <NA>     81
## 671           <NA>     54
## 672           <NA>     NA
## 673          green     76
## 674           <NA>     97
## 675           <NA>    117
## 676           <NA>     NA
## 677           <NA>     81
## 678            red     NA
## 679           <NA>     59
## 680          green     86
## 681           <NA>     82
## 682           <NA>    105
## 683         silver    331
## 684           <NA>     58
## 685           <NA>     54
## 686           <NA>     56
## 687           <NA>    214
## 688           <NA>     79
## 689           <NA>     73
## 690           <NA>    117
## 691           <NA>     50
## 692           <NA>    334
## 693           <NA>     NA
## 694           <NA>     52
## 695           <NA>     71
## 696           <NA>     54
## 697           <NA>     NA
## 698           <NA>     NA
## 699           <NA>     41
## 700            red    135
## 701           <NA>    135
## 702           <NA>     63
## 703           <NA>     NA
## 704           <NA>     79
## 705           <NA>    162
## 706           <NA>     95
## 707           <NA>     54
## 708           <NA>    108
## 709           <NA>     67
## 710           <NA>    158
## 711           <NA>     50
## 712           <NA>     NA
## 713           <NA>     NA
## 714           <NA>     NA
## 715           <NA>     65
## 716           <NA>     NA
## 717           <NA>    117
## 718           <NA>     39
## 719           <NA>    473
## 720           <NA>    135
## 721           <NA>     51
## 722           <NA>    171
## 723           <NA>     74
## 724           <NA>     NA
## 725           <NA>    117
## 726           <NA>     50
## 727           <NA>     61
## 728           <NA>     95
## 729           <NA>     83
## 730           <NA>     52
## 731          white     NA
## 732          green     17
## 733           <NA>     57
## 734           <NA>     81
```


## Not Human
4. List all of the superheros that are not human.

```r
superhero_info%>%
  select(name, Race)%>%
  filter(Race != "Human")
```

```
##                          name               Race
## 1                  Abe Sapien      Icthyo Sapien
## 2                    Abin Sur            Ungaran
## 3                 Abomination  Human / Radiation
## 4                     Abraxas      Cosmic Entity
## 5                        Ajax             Cyborg
## 6                       Alien    Xenomorph XX121
## 7                       Amazo            Android
## 8                       Angel            Vampire
## 9                  Angel Dust             Mutant
## 10               Anti-Monitor      God / Eternal
## 11                 Anti-Venom           Symbiote
## 12                 Apocalypse             Mutant
## 13                    Aqualad          Atlantean
## 14                    Aquaman          Atlantean
## 15                  Archangel             Mutant
## 16                     Ardina              Alien
## 17                      Atlas             Mutant
## 18                      Atlas      God / Eternal
## 19                     Aurora             Mutant
## 20                     Azazel           Neyaphem
## 21                      Beast             Mutant
## 22                   Beyonder      God / Eternal
## 23                  Big Barda            New God
## 24                Bill Harken              Alpha
## 25               Bionic Woman             Cyborg
## 26                    Birdman      God / Eternal
## 27                     Bishop             Mutant
## 28                    Bizarro            Bizarro
## 29                 Black Bolt            Inhuman
## 30               Black Canary          Metahuman
## 31                Black Flash      God / Eternal
## 32                   Blackout              Demon
## 33                  Blackwulf              Alien
## 34                      Blade            Vampire
## 35                      Blink             Mutant
## 36                  Bloodhawk             Mutant
## 37                  Boba Fett      Human / Clone
## 38                  Boom-Boom             Mutant
## 39                   Brainiac            Android
## 40                 Brundlefly             Mutant
## 41                      Cable             Mutant
## 42              Cameron Hicks              Alpha
## 43               Captain Atom  Human / Radiation
## 44             Captain Marvel         Human-Kree
## 45             Captain Planet      God / Eternal
## 46           Captain Universe      God / Eternal
## 47                    Carnage           Symbiote
## 48                    Century              Alien
## 49                    Cerebra             Mutant
## 50                    Chamber             Mutant
## 51                   Colossus             Mutant
## 52                    Copycat             Mutant
## 53                    Crystal            Inhuman
## 54                     Cyborg             Cyborg
## 55            Cyborg Superman             Cyborg
## 56                    Cyclops             Mutant
## 57                   Darkseid            New God
## 58                   Darkstar             Mutant
## 59                 Darth Maul Dathomirian Zabrak
## 60                Darth Vader             Cyborg
## 61                       Data            Android
## 62                    Dazzler             Mutant
## 63                   Deadpool             Mutant
## 64                   Deathlok             Cyborg
## 65                 Demogoblin              Demon
## 66                 Doc Samson  Human / Radiation
## 67                  Donatello             Mutant
## 68                 Donna Troy             Amazon
## 69                   Doomsday              Alien
## 70               Dr Manhattan     Human / Cosmic
## 71         Drax the Destroyer    Human / Altered
## 72                    Etrigan              Demon
## 73              Evil Deadpool             Mutant
## 74                   Evilhawk              Alien
## 75                     Exodus             Mutant
## 76                      Faora         Kryptonian
## 77              Fin Fang Foom    Kakarantharaian
## 78                   Firestar             Mutant
## 79          Franklin Richards             Mutant
## 80                   Galactus      Cosmic Entity
## 81                     Gambit             Mutant
## 82                     Gamora      Zen-Whoberian
## 83                Garbage Man             Mutant
## 84                  Gary Bell              Alpha
## 85                General Zod         Kryptonian
## 86                Ghost Rider              Demon
## 87                  Gladiator          Strontian
## 88                   Godzilla              Kaiju
## 89                       Goku             Saiyan
## 90              Gorilla Grodd            Gorilla
## 91                     Greedo             Rodian
## 92                      Groot     Flora Colossus
## 93                Guy Gardner    Human-Vuldarian
## 94                      Havok             Mutant
## 95                       Hela          Asgardian
## 96                    Hellboy              Demon
## 97                   Hercules           Demi-God
## 98                       Hulk  Human / Radiation
## 99                Human Torch  Human / Radiation
## 100                      Husk             Mutant
## 101                    Hybrid           Symbiote
## 102                  Hyperion            Eternal
## 103                    Iceman             Mutant
## 104                    Indigo              Alien
## 105                       Ink             Mutant
## 106           Invisible Woman  Human / Radiation
## 107             Jar Jar Binks             Gungan
## 108                 Jean Grey             Mutant
## 109                   Jubilee             Mutant
## 110                  Junkpile             Mutant
## 111                     K-2SO            Android
## 112               Killer Croc          Metahuman
## 113                   Kilowog         Bolovaxian
## 114                 King Kong             Animal
## 115                King Shark             Animal
## 116                    Krypto         Kryptonian
## 117          Lady Deathstrike             Cyborg
## 118                    Legion             Mutant
## 119                  Leonardo             Mutant
## 120           Living Tribunal      Cosmic Entity
## 121                      Lobo           Czarnian
## 122                      Loki          Asgardian
## 123                   Magneto             Mutant
## 124           Man of Miracles      God / Eternal
## 125                    Mantis         Human-Kree
## 126         Martian Manhunter            Martian
## 127              Master Chief    Human / Altered
## 128                    Medusa            Inhuman
## 129                      Mera          Atlantean
## 130                   Metallo            Android
## 131              Michelangelo             Mutant
## 132          Mister Fantastic  Human / Radiation
## 133              Mister Knife            Spartoi
## 134           Mister Mxyzptlk      God / Eternal
## 135           Mister Sinister    Human / Altered
## 136                     MODOK             Cyborg
## 137                      Mogo             Planet
## 138               Mr Immortal             Mutant
## 139                  Mystique             Mutant
## 140                     Namor          Atlantean
## 141                    Nebula          Luphomoid
## 142 Negasonic Teenage Warhead             Mutant
## 143              Nina Theroux              Alpha
## 144                      Nova     Human / Cosmic
## 145                      Odin      God / Eternal
## 146             One-Above-All      Cosmic Entity
## 147                 Onslaught             Mutant
## 148                 Parademon          Parademon
## 149                   Phoenix             Mutant
## 150                  Plantman             Mutant
## 151                   Polaris             Mutant
## 152                Power Girl         Kryptonian
## 153                 Power Man             Mutant
## 154                  Predator             Yautja
## 155               Professor X             Mutant
## 156                  Psylocke             Mutant
## 157                         Q      God / Eternal
## 158               Quicksilver             Mutant
## 159             Rachel Pirzad              Alpha
## 160                   Raphael             Mutant
## 161                  Red Hulk  Human / Radiation
## 162               Red Tornado            Android
## 163                     Rhino  Human / Radiation
## 164            Rocket Raccoon             Animal
## 165                Sabretooth             Mutant
## 166                    Sauron              Maiar
## 167         Scarlet Spider II              Clone
## 168             Scarlet Witch             Mutant
## 169            Sebastian Shaw             Mutant
## 170                    Sentry             Mutant
## 171               Shadow Lass           Talokite
## 172                 Shadowcat             Mutant
## 173                 She-Thing  Human / Radiation
## 174                       Sif          Asgardian
## 175             Silver Surfer              Alien
## 176                  Sinestro          Korugaran
## 177                     Siren          Atlantean
## 178                Snake-Eyes             Animal
## 179            Solomon Grundy             Zombie
## 180                     Spawn              Demon
## 181                   Spectre      God / Eternal
## 182            Spider-Carnage           Symbiote
## 183                     Spock       Human-Vulcan
## 184                     Spyke             Mutant
## 185                 Star-Lord      Human-Spartoi
## 186                  Starfire         Tamaranean
## 187                    Static             Mutant
## 188               Steppenwolf            New God
## 189                     Storm             Mutant
## 190                   Sunspot             Mutant
## 191            Superboy-Prime         Kryptonian
## 192                 Supergirl         Kryptonian
## 193                  Superman         Kryptonian
## 194               Swamp Thing      God / Eternal
## 195                     Swarm             Mutant
## 196                    T-1000            Android
## 197                     T-800             Cyborg
## 198                     T-850             Cyborg
## 199                       T-X             Cyborg
## 200                    Thanos            Eternal
## 201                     Thing  Human / Radiation
## 202                      Thor          Asgardian
## 203                 Thor Girl          Asgardian
## 204                      Toad             Mutant
## 205                     Toxin           Symbiote
## 206                     Toxin           Symbiote
## 207                    Trigon      God / Eternal
## 208                    Triton            Inhuman
## 209                    Ultron            Android
## 210               Utgard-Loki        Frost Giant
## 211                    Vegeta             Saiyan
## 212                     Venom           Symbiote
## 213                 Venom III           Symbiote
## 214                 Venompool           Symbiote
## 215                    Vision            Android
## 216                   Warpath             Mutant
## 217                 Wolverine             Mutant
## 218               Wonder Girl           Demi-God
## 219              Wonder Woman             Amazon
## 220                      X-23     Mutant / Clone
## 221                      Ymir        Frost Giant
## 222                      Yoda     Yoda's species
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <- superhero_info%>%
  select(name, Alignment, Race, Gender, Height, Hair.color)%>%
  filter(Alignment == "good")
```


```r
bad_guys <- superhero_info%>%
  select(name, Alignment, Race, Gender, Height, Hair.color)%>%
  filter(Alignment == "bad")
```


6. For the good guys, use the `tabyl` function to summarize their "race".


```r
good_guys%>%
  tabyl(Race)
```

```
##               Race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```


7. Among the good guys, Who are the Vampires?


```r
good_guys%>%
  select(name, Alignment, Race)%>%
  filter(Race == "Vampire")
```

```
##    name Alignment    Race
## 1 Angel      good Vampire
## 2 Blade      good Vampire
```



8. Among the bad guys, who are the male humans over 200 inches in height?


```r
bad_guys%>%
  select(name, Gender, Height)%>%
  filter(Gender == "Male" & Height >= 200.0)
```

```
##              name Gender Height
## 1     Abomination   Male    203
## 2           Alien   Male    244
## 3           Amazo   Male    257
## 4      Apocalypse   Male    213
## 5            Bane   Male    203
## 6        Darkseid   Male    267
## 7     Doctor Doom   Male    201
## 8  Doctor Doom II   Male    201
## 9        Doomsday   Male    244
## 10    Killer Croc   Male    244
## 11        Kingpin   Male    201
## 12         Lizard   Male    203
## 13          MODOK   Male    366
## 14      Omega Red   Male    211
## 15      Onslaught   Male    305
## 16       Predator   Male    213
## 17         Sauron   Male    279
## 18       Scorpion   Male    211
## 19 Solomon Grundy   Male    279
## 20         Thanos   Male    201
## 21         Ultron   Male    206
## 22      Venom III   Male    229
```


9. Are there more good guys or bad guys with green hair?  
ANS: There are more good guys

```r
good_guys%>%
  select(name, Hair.color, Alignment)%>%
  filter(Hair.color == "Green")
```

```
##             name Hair.color Alignment
## 1      Beast Boy      Green      good
## 2 Captain Planet      Green      good
## 3     Doc Samson      Green      good
## 4           Hulk      Green      good
## 5           Lyja      Green      good
## 6        Polaris      Green      good
## 7       She-Hulk      Green      good
```


```r
bad_guys%>%
  select(name, Hair.color, Alignment)%>%
  filter(Hair.color == "Green")
```

```
##    name Hair.color Alignment
## 1 Joker      Green       bad
```

10. Let's explore who the really small superheros are. In the `superhero_info` data, which have a weight less than 50? Be sure to sort your results by weight lowest to highest.  


```r
superhero_info%>%
  select(name, Weight)%>%
  filter(Weight <= 50)%>%
  arrange(Weight)
```

```
##                 name Weight
## 1        Iron Monger      2
## 2              Groot      4
## 3          Jack-Jack     14
## 4           Galactus     16
## 5               Yoda     17
## 6      Fin Fang Foom     18
## 7    Howard the Duck     18
## 8             Krypto     18
## 9     Rocket Raccoon     25
## 10              Dash     27
## 11          Longshot     36
## 12           Robin V     38
## 13           Wiz Kid     39
## 14       Violet Parr     41
## 15 Franklin Richards     45
## 16             Swarm     47
## 17      Hope Summers     48
## 18              Jolt     49
## 19          Snowbird     49
## 20             Armor     50
## 21           Cheetah     50
## 22           Crystal     50
## 23             Feral     50
## 24      Goblin Queen     50
## 25        Poison Ivy     50
## 26             Raven     50
## 27         Shadowcat     50
## 28        Silverclaw     50
## 29          Venom II     50
## 30              Wasp     50
## 31              X-23     50
```


11. Let's make a new variable that is the ratio of height to weight. Call this variable `height_weight_ratio`.  


```r
superhero_info%>%
  mutate(height_weight_ratio = Height/Weight)%>%
  select(name, height_weight_ratio)
```

```
##                          name height_weight_ratio
## 1                      A-Bomb          0.46031746
## 2                  Abe Sapien          2.93846154
## 3                    Abin Sur          2.05555556
## 4                 Abomination          0.46031746
## 5                     Abraxas                  NA
## 6               Absorbing Man          1.58196721
## 7                 Adam Monroe                  NA
## 8                Adam Strange          2.10227273
## 9                    Agent 13          2.83606557
## 10                  Agent Bob          2.19753086
## 11                 Agent Zero          1.83653846
## 12                 Air-Walker          1.74074074
## 13                       Ajax          2.14444444
## 14                 Alan Scott          2.00000000
## 15                Alex Mercer                  NA
## 16               Alex Woolsly                  NA
## 17          Alfred Pennyworth          2.47222222
## 18                      Alien          1.44378698
## 19           Allan Quatermain                  NA
## 20                      Amazo          1.48554913
## 21                       Ammo          1.86138614
## 22             Ando Masahashi                  NA
## 23                      Angel          2.69117647
## 24                      Angel                  NA
## 25                 Angel Dust          2.89473684
## 26            Angel Salvadore          3.01851852
## 27                     Angela                  NA
## 28                 Animal Man          2.20481928
## 29                  Annihilus          2.00000000
## 30                    Ant-Man          1.72950820
## 31                 Ant-Man II          2.12790698
## 32               Anti-Monitor                  NA
## 33                 Anti-Spawn                  NA
## 34                 Anti-Venom          0.63966480
## 35                 Apocalypse          1.57777778
## 36                   Aquababy                  NA
## 37                    Aqualad          1.67924528
## 38                    Aquaman          1.26712329
## 39                    Arachne          2.77777778
## 40                  Archangel          2.69117647
## 41                   Arclight          3.03508772
## 42                     Ardina          1.96938776
## 43                       Ares          0.68518519
## 44                      Ariel          2.79661017
## 45                      Armor          3.26000000
## 46                    Arsenal                  NA
## 47                  Astro Boy                  NA
## 48                      Atlas          1.81188119
## 49                      Atlas          1.57142857
## 50                       Atom          2.61764706
## 51                       Atom                  NA
## 52                  Atom Girl          3.11111111
## 53                    Atom II          2.25925926
## 54                   Atom III                  NA
## 55                    Atom IV                  NA
## 56                     Aurora          2.85714286
## 57                     Azazel          2.73134328
## 58                     Azrael                  NA
## 59                      Aztar                  NA
## 60                       Bane          1.12777778
## 61                    Banshee          2.37662338
## 62                     Bantam          3.05555556
## 63                    Batgirl                  NA
## 64                    Batgirl          2.98245614
## 65                Batgirl III                  NA
## 66                 Batgirl IV          3.17307692
## 67                  Batgirl V                  NA
## 68                 Batgirl VI          2.75409836
## 69                     Batman          1.97894737
## 70                     Batman          2.31168831
## 71                  Batman II          2.25316456
## 72                 Battlestar          1.48872180
## 73                 Batwoman V                  NA
## 74                       Beak          2.77777778
## 75                      Beast          0.99447514
## 76                  Beast Boy          2.54411765
## 77                     Beetle                  NA
## 78                     Ben 10                  NA
## 79              Beta Ray Bill          0.93055556
## 80                   Beyonder                  NA
## 81                  Big Barda          1.39259259
## 82                  Big Daddy                  NA
## 83                    Big Man          2.32394366
## 84                Bill Harken                  NA
## 85              Billy Kincaid                  NA
## 86                     Binary          3.33333333
## 87               Bionic Woman                  NA
## 88                 Bird-Brain                  NA
## 89                   Bird-Man                  NA
## 90                Bird-Man II                  NA
## 91                    Birdman                  NA
## 92                     Bishop          1.59677419
## 93                    Bizarro          1.23225806
## 94               Black Abbott                  NA
## 95                 Black Adam          1.69026549
## 96                 Black Bolt          1.97894737
## 97               Black Canary          2.84482759
## 98               Black Canary          2.88135593
## 99                  Black Cat          3.29629630
## 100               Black Flash                  NA
## 101             Black Goliath                  NA
## 102          Black Knight III          2.12790698
## 103           Black Lightning          2.05555556
## 104               Black Mamba          3.26923077
## 105               Black Manta          2.04347826
## 106             Black Panther          2.03333333
## 107               Black Widow          2.88135593
## 108            Black Widow II          2.78688525
## 109                  Blackout          1.83653846
## 110                 Blackwing          2.15116279
## 111                 Blackwulf          2.13636364
## 112                     Blade          1.93814433
## 113               Blaquesmith                  NA
## 114                    Bling!          2.47058824
## 115                     Blink          2.94642857
## 116                  Blizzard                  NA
## 117                  Blizzard                  NA
## 118               Blizzard II          2.27272727
## 119                      Blob          0.77391304
## 120                  Bloodaxe          0.44040404
## 121                 Bloodhawk                  NA
## 122               Bloodwraith                  NA
## 123               Blue Beetle                  NA
## 124               Blue Beetle                  NA
## 125            Blue Beetle II          2.12790698
## 126           Blue Beetle III                  NA
## 127                 Boba Fett                  NA
## 128                      Bolt                  NA
## 129                Bomb Queen                  NA
## 130                 Boom-Boom          3.00000000
## 131                    Boomer                  NA
## 132              Booster Gold          2.02061856
## 133                       Box                  NA
## 134                   Box III          1.75454545
## 135                    Box IV                  NA
## 136                  Brainiac          1.46666667
## 137                Brainiac 5          2.78688525
## 138            Brother Voodoo          1.84848485
## 139                Brundlefly                  NA
## 140                     Buffy          3.01923077
## 141                  Bullseye          2.03333333
## 142                 Bumblebee          2.88135593
## 143                 Bumbleboy                  NA
## 144                   Bushido                  NA
## 145                     Cable          1.28481013
## 146                  Callisto          2.36486486
## 147             Cameron Hicks                  NA
## 148                Cannonball          2.25925926
## 149           Captain America          1.74074074
## 150              Captain Atom          2.14444444
## 151           Captain Britain          1.70689655
## 152              Captain Cold                  NA
## 153              Captain Epic                  NA
## 154         Captain Hindsight                  NA
## 155          Captain Mar-vell          1.74074074
## 156            Captain Marvel          2.43243243
## 157            Captain Marvel          1.91089109
## 158         Captain Marvel II          2.36486486
## 159          Captain Midnight                  NA
## 160            Captain Planet                  NA
## 161          Captain Universe                  NA
## 162                   Carnage          2.15116279
## 163                       Cat          2.83606557
## 164                    Cat II                  NA
## 165                  Catwoman          2.86885246
## 166             Cecilia Reyes          2.74193548
## 167                   Century          2.07216495
## 168                   Cerebra                  NA
## 169                   Chamber          2.77777778
## 170                 Chameleon                  NA
## 171                Changeling          2.22222222
## 172                   Cheetah          3.26000000
## 173                Cheetah II          3.09090909
## 174               Cheetah III          3.24074074
## 175                   Chromos          2.15116279
## 176              Chuck Norris                  NA
## 177             Citizen Steel          1.07647059
## 178             Claire Bennet                  NA
## 179                      Clea                  NA
## 180                     Cloak          3.22857143
## 181                Clock King          2.28205128
## 182                Cogliostro                  NA
## 183              Colin Wagner                  NA
## 184              Colossal Boy                  NA
## 185                  Colossus          1.00444444
## 186                   Copycat          2.73134328
## 187                   Corsair          2.41772152
## 188               Cottonmouth          1.84848485
## 189          Crimson Crusader                  NA
## 190            Crimson Dynamo          1.73076923
## 191                   Crystal          3.36000000
## 192                     Curse                  NA
## 193                    Cy-Gor                  NA
## 194                    Cyborg          1.14450867
## 195           Cyborg Superman                  NA
## 196                   Cyclops          2.17045455
## 197                    Cypher          2.57352941
## 198                    Dagger          3.17307692
## 199              Danny Cooper                  NA
## 200             Daphne Powell                  NA
## 201                 Daredevil          2.03333333
## 202                  Darkhawk          2.28395062
## 203                   Darkman                  NA
## 204                  Darkseid          0.32680539
## 205                  Darkside                  NA
## 206                  Darkstar          3.00000000
## 207                Darth Maul                  NA
## 208               Darth Vader          1.46666667
## 209                      Dash          4.51851852
## 210                      Data                  NA
## 211                   Dazzler          3.32692308
## 212                   Deadman          2.03333333
## 213                  Deadpool          1.97894737
## 214                  Deadshot          2.03296703
## 215                  Deathlok          1.08426966
## 216               Deathstroke          1.91089109
## 217                Demogoblin          1.94736842
## 218                 Destroyer          0.49086162
## 219               Diamondback          2.14444444
## 220                DL Hawkins                  NA
## 221                Doc Samson          1.15789474
## 222               Doctor Doom          1.07486631
## 223            Doctor Doom II          1.52272727
## 224               Doctor Fate          2.11235955
## 225            Doctor Octopus          1.59090909
## 226            Doctor Strange          2.32098765
## 227                    Domino          3.20370370
## 228                 Donatello                  NA
## 229                Donna Troy          2.77777778
## 230                  Doomsday          0.59223301
## 231              Doppelganger          1.88461538
## 232                  Dormammu                  NA
## 233              Dr Manhattan                  NA
## 234        Drax the Destroyer          0.63071895
## 235                       Ego                  NA
## 236                Elastigirl          3.00000000
## 237                   Electro          2.43243243
## 238                   Elektra          2.96610169
## 239               Elle Bishop                  NA
## 240             Elongated Man          2.31250000
## 241                Emma Frost          2.73846154
## 242               Enchantress          2.94736842
## 243                    Energy                  NA
## 244                     ERG-1                  NA
## 245                Ethan Hunt                  NA
## 246                   Etrigan          0.95073892
## 247             Evil Deadpool          1.97894737
## 248                  Evilhawk          1.80188679
## 249                    Exodus          2.07954545
## 250             Fabian Cortez          2.04166667
## 251                    Falcon          1.74074074
## 252             Fallen One II                  NA
## 253                     Faora                  NA
## 254                     Feral          3.50000000
## 255           Fighting Spirit                  NA
## 256             Fin Fang Foom         54.16666667
## 257                  Firebird          2.94642857
## 258                  Firelord          1.94949495
## 259                  Firestar          3.08928571
## 260                 Firestorm                  NA
## 261                 Firestorm          2.06593407
## 262                     Fixer                  NA
## 263                     Flash          2.22222222
## 264              Flash Gordon                  NA
## 265                  Flash II          2.07954545
## 266                 Flash III          2.12790698
## 267                  Flash IV          3.01923077
## 268                     Forge          2.25925926
## 269         Franklin Richards          3.15555556
## 270            Franklin Storm          2.04347826
## 271                    Frenzy          2.02884615
## 272                    Frigga          1.07784431
## 273                  Galactus         54.75000000
## 274                    Gambit          2.28395062
## 275                    Gamora          2.37662338
## 276               Garbage Man                  NA
## 277                 Gary Bell                  NA
## 278               General Zod                  NA
## 279                   Genesis          2.15116279
## 280               Ghost Rider          1.89898990
## 281            Ghost Rider II                  NA
## 282                 Giant-Man                  NA
## 283              Giant-Man II                  NA
## 284                   Giganta          0.09920635
## 285                 Gladiator          0.73880597
## 286              Goblin Queen          3.36000000
## 287                  Godzilla                  NA
## 288                       Gog                  NA
## 289                      Goku          2.82258065
## 290                   Goliath                  NA
## 291                   Goliath                  NA
## 292                   Goliath                  NA
## 293                Goliath IV          2.03333333
## 294             Gorilla Grodd          0.73333333
## 295           Granny Goodness          1.54782609
## 296                   Gravity          2.25316456
## 297                    Greedo                  NA
## 298               Green Arrow          2.13636364
## 299              Green Goblin          2.16867470
## 300           Green Goblin II          2.31168831
## 301          Green Goblin III          2.07954545
## 302           Green Goblin IV          2.25316456
## 303                     Groot        175.25000000
## 304                  Guardian                  NA
## 305               Guy Gardner          1.97894737
## 306                Hal Jordan          2.08888889
## 307                  Han Solo          2.31645570
## 308                   Hancock                  NA
## 309              Harley Quinn          2.69841270
## 310              Harry Potter                  NA
## 311                     Havok          2.31645570
## 312                      Hawk          2.07865169
## 313                   Hawkeye          1.83653846
## 314                Hawkeye II          2.89473684
## 315                  Hawkgirl          2.86885246
## 316                   Hawkman          2.10227273
## 317                 Hawkwoman          3.24074074
## 318              Hawkwoman II                  NA
## 319             Hawkwoman III          2.61538462
## 320                 Heat Wave          2.22222222
## 321                      Hela          0.94666667
## 322                   Hellboy          1.63924051
## 323                   Hellcat          2.83606557
## 324                 Hellstorm          2.28395062
## 325                  Hercules          1.34246575
## 326             Hiro Nakamura                  NA
## 327                  Hit-Girl                  NA
## 328                 Hobgoblin          2.16867470
## 329                    Hollow                  NA
## 330              Hope Summers          3.50000000
## 331           Howard the Duck          4.38888889
## 332                      Hulk          0.38730159
## 333               Human Torch          2.31168831
## 334                  Huntress          3.05084746
## 335                      Husk          2.93103448
## 336                    Hybrid          2.27272727
## 337                 Hydro-Man          1.57983193
## 338                  Hyperion          0.88405797
## 339                    Iceman          2.66153846
## 340                   Impulse          2.61538462
## 341             Indiana Jones          2.31645570
## 342                    Indigo                  NA
## 343                       Ink          2.22222222
## 344           Invisible Woman          3.11111111
## 345                 Iron Fist          2.27848101
## 346                  Iron Man          1.03664921
## 347               Iron Monger                  NA
## 348                      Isis                  NA
## 349                Jack Bauer                  NA
## 350            Jack of Hearts          1.96202532
## 351                 Jack-Jack          5.07142857
## 352                James Bond                  NA
## 353             James T. Kirk          2.31168831
## 354             Jar Jar Binks                  NA
## 355              Jason Bourne                  NA
## 356                 Jean Grey          3.23076923
## 357           Jean-Luc Picard                  NA
## 358             Jennifer Kale          3.05454545
## 359               Jesse Quick                  NA
## 360              Jessica Cruz                  NA
## 361             Jessica Jones          3.03571429
## 362           Jessica Sanders                  NA
## 363                    Jigsaw          1.66371681
## 364                Jim Powell                  NA
## 365                 JJ Powell                  NA
## 366             Johann Krauss                  NA
## 367          John Constantine                  NA
## 368              John Stewart          2.05555556
## 369               John Wraith          2.07954545
## 370                     Joker          2.27906977
## 371                      Jolt          3.36734694
## 372                   Jubilee          3.17307692
## 373               Judge Dredd                  NA
## 374                Juggernaut          0.33567251
## 375                  Junkpile                  NA
## 376                   Justice          2.19753086
## 377                  Jyn Erso                  NA
## 378                     K-2SO                  NA
## 379                      Kang          1.83653846
## 380                Karate Kid          2.40277778
## 381           Kathryn Janeway                  NA
## 382          Katniss Everdeen                  NA
## 383                  Kevin 11                  NA
## 384                  Kick-Ass                  NA
## 385                 Kid Flash                  NA
## 386              Kid Flash II                  NA
## 387               Killer Croc          0.68539326
## 388              Killer Frost                  NA
## 389                   Kilowog          0.72222222
## 390                 King Kong                  NA
## 391                King Shark                  NA
## 392                   Kingpin          0.99014778
## 393                      Klaw          1.93814433
## 394              Kool-Aid Man                  NA
## 395                 Kraven II          1.92929293
## 396         Kraven the Hunter          1.72641509
## 397                    Krypto          3.55555556
## 398               Kyle Rayner          2.27848101
## 399                  Kylo Ren                  NA
## 400             Lady Bullseye                  NA
## 401          Lady Deathstrike          3.01724138
## 402                    Leader          2.82539683
## 403                     Leech                  NA
## 404                    Legion          2.96610169
## 405                  Leonardo                  NA
## 406                Lex Luthor          1.97894737
## 407                Light Lass          3.05555556
## 408             Lightning Lad          2.38461538
## 409            Lightning Lord          2.01052632
## 410              Living Brain          0.55000000
## 411           Living Tribunal                  NA
## 412               Liz Sherman                  NA
## 413                    Lizard          0.88260870
## 414                      Lobo          0.79513889
## 415                      Loki          0.81779661
## 416                  Longshot          5.22222222
## 417                 Luke Cage          1.03664921
## 418             Luke Campbell                  NA
## 419            Luke Skywalker          2.18181818
## 420                      Luna                  NA
## 421                      Lyja                  NA
## 422                   Mach-IV          2.27848101
## 423               Machine Man          0.47780679
## 424                   Magneto          2.18604651
## 425                     Magog                  NA
## 426                     Magus                  NA
## 427           Man of Miracles                  NA
## 428                   Man-Bat                  NA
## 429                 Man-Thing          0.94666667
## 430                  Man-Wolf          2.08888889
## 431                  Mandarin          1.93814433
## 432                    Mantis          3.23076923
## 433         Martian Manhunter          1.48888889
## 434               Marvel Girl          3.03571429
## 435              Master Brood          2.25925926
## 436              Master Chief                  NA
## 437                     Match                  NA
## 438              Matt Parkman                  NA
## 439                  Maverick          1.75454545
## 440                    Maxima          2.50000000
## 441              Maya Herrera                  NA
## 442                    Medusa          3.05084746
## 443                  Meltdown          3.05555556
## 444                  Mephisto          1.41428571
## 445                      Mera          2.43055556
## 446                   Metallo          2.17777778
## 447                Metamorpho          2.05555556
## 448                 Meteorite                  NA
## 449                    Metron          2.15116279
## 450             Micah Sanders                  NA
## 451              Michelangelo                  NA
## 452                 Micro Lad          2.37662338
## 453                     Mimic          1.86138614
## 454              Minna Murray                  NA
## 455                    Misfit                  NA
## 456              Miss Martian          2.91803279
## 457          Mister Fantastic          2.28395062
## 458             Mister Freeze          2.12790698
## 459              Mister Knife                  NA
## 460           Mister Mxyzptlk                  NA
## 461           Mister Sinister          1.53125000
## 462              Mister Zsasz                  NA
## 463               Mockingbird          2.86885246
## 464                     MODOK          1.08284024
## 465                      Mogo                  NA
## 466           Mohinder Suresh                  NA
## 467                    Moloch                  NA
## 468                Molten Man          0.79032258
## 469                   Monarch          2.14444444
## 470             Monica Dawson                  NA
## 471               Moon Knight          1.86138614
## 472                 Moonstone          3.05084746
## 473                    Morlun          2.37974684
## 474                     Morph          2.25316456
## 475              Moses Magnum          2.43055556
## 476               Mr Immortal          2.68571429
## 477             Mr Incredible          1.27215190
## 478              Ms Marvel II          2.83606557
## 479              Multiple Man          2.57142857
## 480                  Mysterio          2.27848101
## 481                  Mystique          3.29629630
## 482                     Namor                  NA
## 483                     Namor          1.50400000
## 484                    Namora          2.11764706
## 485                  Namorita          1.66336634
## 486            Naruto Uzumaki          3.11111111
## 487           Nathan Petrelli                  NA
## 488                    Nebula          2.22891566
## 489 Negasonic Teenage Warhead                  NA
## 490                 Nick Fury          1.86868687
## 491              Nightcrawler          1.98863636
## 492                 Nightwing          2.25316456
## 493              Niki Sanders                  NA
## 494              Nina Theroux                  NA
## 495               Nite Owl II                  NA
## 496                 Northstar          2.16867470
## 497                      Nova          2.15116279
## 498                      Nova          2.76271186
## 499                      Odin          0.70307167
## 500                 Offspring                  NA
## 501                 Omega Red          1.10471204
## 502                Omniscient          2.76923077
## 503             One Punch Man          2.53623188
## 504             One-Above-All                  NA
## 505                 Onslaught          0.75308642
## 506                    Oracle          3.01694915
## 507                    Osiris                  NA
## 508                 Overtkill                  NA
## 509                Ozymandias                  NA
## 510                 Parademon                  NA
## 511                Paul Blart          1.45299145
## 512                   Penance                  NA
## 513                 Penance I                  NA
## 514                Penance II          2.05617978
## 515                   Penguin          1.98734177
## 516            Peter Petrelli                  NA
## 517                   Phantom                  NA
## 518              Phantom Girl          3.11111111
## 519                   Phoenix          3.23076923
## 520                  Plantman          2.10344828
## 521               Plastic Lad                  NA
## 522               Plastic Man          2.31250000
## 523                 Plastique          3.05454545
## 524                Poison Ivy          3.36000000
## 525                   Polaris          3.26923077
## 526                Power Girl          2.22222222
## 527                 Power Man                  NA
## 528                  Predator          0.91025641
## 529               Professor X          2.12790698
## 530            Professor Zoom          2.22222222
## 531              Proto-Goblin                  NA
## 532                  Psylocke          2.57142857
## 533                  Punisher          2.03333333
## 534                Purple Man          2.43243243
## 535                      Pyro          2.61764706
## 536                         Q                  NA
## 537                   Quantum                  NA
## 538                  Question          2.26506024
## 539               Quicksilver          2.31645570
## 540                     Quill          2.91071429
## 541              Ra's Al Ghul          1.98969072
## 542             Rachel Pirzad                  NA
## 543                     Rambo          2.14457831
## 544                   Raphael                  NA
## 545                     Raven          3.30000000
## 546                       Ray          2.54285714
## 547             Razor-Fist II          1.63247863
## 548                 Red Arrow          2.16867470
## 549                  Red Hood          2.25925926
## 550                  Red Hulk          0.33809524
## 551                  Red Mist                  NA
## 552                 Red Robin          2.94642857
## 553                 Red Skull          1.74074074
## 554               Red Tornado          1.26712329
## 555               Redeemer II                  NA
## 556              Redeemer III                  NA
## 557              Renata Soliz                  NA
## 558                       Rey                  NA
## 559                     Rhino          0.61250000
## 560                 Rick Flag          2.17647059
## 561                   Riddler                  NA
## 562                Rip Hunter                  NA
## 563                   Ripcord          2.50000000
## 564                     Robin          2.25316456
## 565                  Robin II          1.81188119
## 566                 Robin III          2.94642857
## 567                   Robin V          3.60526316
## 568                  Robin VI                  NA
## 569            Rocket Raccoon          4.88000000
## 570                     Rogue          3.20370370
## 571                     Ronin          1.83653846
## 572                 Rorschach          2.66666667
## 573                Sabretooth          1.15789474
## 574                      Sage          2.78688525
## 575                   Sandman          0.91133005
## 576                 Sasquatch          0.33888889
## 577                    Sauron                  NA
## 578             Savage Dragon                  NA
## 579                 Scarecrow          2.90476190
## 580            Scarlet Spider          2.40540541
## 581         Scarlet Spider II          1.70796460
## 582             Scarlet Witch          2.88135593
## 583                   Scorpia                  NA
## 584                  Scorpion          0.68064516
## 585            Sebastian Shaw                  NA
## 586                    Sentry          2.16091954
## 587               Shadow King          1.24161074
## 588               Shadow Lass          3.20370370
## 589                 Shadowcat          3.36000000
## 590                 Shang-Chi          2.25316456
## 591               Shatterstar          2.17045455
## 592                  She-Hulk          0.63809524
## 593                 She-Thing          1.19607843
## 594                   Shocker          2.21518987
## 595                    Shriek          3.32692308
## 596          Shrinking Violet                  NA
## 597                       Sif          0.98429319
## 598                      Silk                  NA
## 599              Silk Spectre                  NA
## 600           Silk Spectre II                  NA
## 601             Silver Surfer          1.91089109
## 602                Silverclaw          3.14000000
## 603                 Simon Baz                  NA
## 604                  Sinestro          2.18478261
## 605                     Siren          2.43055556
## 606                  Siren II                  NA
## 607                     Siryn          3.23076923
## 608                     Skaar          1.10000000
## 609                Snake-Eyes                  NA
## 610                  Snowbird          3.63265306
## 611                     Sobek                  NA
## 612            Solomon Grundy          0.63844394
## 613                  Songbird          2.53846154
## 614               Space Ghost          1.66371681
## 615                     Spawn          0.52098765
## 616                   Spectre                  NA
## 617                 Speedball                  NA
## 618                    Speedy                  NA
## 619                    Speedy                  NA
## 620            Spider-Carnage                  NA
## 621               Spider-Girl          3.14814815
## 622               Spider-Gwen          2.94642857
## 623                Spider-Man          2.40540541
## 624                Spider-Man          2.31168831
## 625                Spider-Man          2.80357143
## 626              Spider-Woman          3.01694915
## 627           Spider-Woman II                  NA
## 628          Spider-Woman III          3.14545455
## 629           Spider-Woman IV          3.06896552
## 630                     Spock          2.28395062
## 631                     Spyke          2.20481928
## 632                   Stacy X                  NA
## 633                 Star-Lord          2.37974684
## 634                  Stardust                  NA
## 635                  Starfire          2.71830986
## 636                  Stargirl          2.66129032
## 637                    Static          2.69841270
## 638                     Steel          1.53435115
## 639          Stephanie Powell                  NA
## 640               Steppenwolf          2.01098901
## 641                     Storm          3.15789474
## 642              Stormtrooper                  NA
## 643                   Sunspot          2.24675325
## 644                  Superboy          2.50000000
## 645            Superboy-Prime          2.33766234
## 646                 Supergirl          3.05555556
## 647                  Superman          1.89108911
## 648               Swamp Thing                  NA
## 649                     Swarm          4.17021277
## 650                     Sylar                  NA
## 651                     Synch          2.43243243
## 652                    T-1000          1.25342466
## 653                     T-800                  NA
## 654                     T-850                  NA
## 655                       T-X                  NA
## 656                Taskmaster          1.89898990
## 657                   Tempest          3.01851852
## 658                    Thanos          0.45372460
## 659                  The Cape                  NA
## 660              The Comedian          1.86138614
## 661                     Thing          0.81333333
## 662                      Thor          0.68750000
## 663                 Thor Girl          1.22377622
## 664               Thunderbird          1.83168317
## 665            Thunderbird II                  NA
## 666           Thunderbird III          2.36486486
## 667             Thunderstrike          0.68750000
## 668                   Thundra          1.37974684
## 669               Tiger Shark          0.91133005
## 670                     Tigra          2.19753086
## 671                  Tinkerer          3.01851852
## 672                     Titan                  NA
## 673                      Toad          2.30263158
## 674                     Toxin          1.93814433
## 675                     Toxin          1.63247863
## 676             Tracy Strauss                  NA
## 677                 Trickster          2.25925926
## 678                    Trigon                  NA
## 679           Triplicate Girl          2.84745763
## 680                    Triton          2.18604651
## 681                  Two-Face          2.23170732
## 682                 Ultragirl          1.60000000
## 683                    Ultron          0.62235650
## 684               Utgard-Loki          0.26206897
## 685                  Vagabond          3.11111111
## 686              Valerie Hart          3.12500000
## 687                  Valkyrie          0.89252336
## 688                  Vanisher          2.08860759
## 689                    Vegeta          2.30136986
## 690                     Venom          1.63247863
## 691                  Venom II          3.50000000
## 692                 Venom III          0.68562874
## 693                 Venompool                  NA
## 694                Vertigo II          3.23076923
## 695                      Vibe          2.50704225
## 696                Vindicator          3.05555556
## 697                Vindicator                  NA
## 698                  Violator                  NA
## 699               Violet Parr          3.34146341
## 700                    Vision          1.41481481
## 701                 Vision II          1.41481481
## 702                     Vixen          2.77777778
## 703                    Vulcan                  NA
## 704                   Vulture          2.27848101
## 705                    Walrus          1.12962963
## 706               War Machine          1.94736842
## 707                   Warbird          3.33333333
## 708                   Warlock          1.74074074
## 709                      Warp          2.58208955
## 710                   Warpath          1.37974684
## 711                      Wasp          3.26000000
## 712                   Watcher                  NA
## 713                 Weapon XI                  NA
## 714              White Canary                  NA
## 715               White Queen          2.73846154
## 716                  Wildfire                  NA
## 717            Winter Soldier          1.49572650
## 718                   Wiz Kid          3.58974359
## 719                 Wolfsbane          0.77378436
## 720                 Wolverine          1.18518519
## 721               Wonder Girl          3.23529412
## 722                Wonder Man          1.09941520
## 723              Wonder Woman          2.47297297
## 724                    Wondra                  NA
## 725            Wyatt Wingfoot          1.67521368
## 726                      X-23          3.10000000
## 727                     X-Man          2.86885246
## 728               Yellow Claw          1.97894737
## 729              Yellowjacket          2.20481928
## 730           Yellowjacket II          3.17307692
## 731                      Ymir                  NA
## 732                      Yoda          3.88235294
## 733                   Zatanna          2.98245614
## 734                      Zoom          2.28395062
```


12. Who has the highest height to weight ratio?  
ANS: Groot has highest ratio

```r
  superhero_info%>%
  mutate(height_weight_ratio = Height/Weight)%>%
  select(name, height_weight_ratio)%>%
  arrange(desc(height_weight_ratio))
```

```
##                          name height_weight_ratio
## 1                       Groot        175.25000000
## 2                    Galactus         54.75000000
## 3               Fin Fang Foom         54.16666667
## 4                    Longshot          5.22222222
## 5                   Jack-Jack          5.07142857
## 6              Rocket Raccoon          4.88000000
## 7                        Dash          4.51851852
## 8             Howard the Duck          4.38888889
## 9                       Swarm          4.17021277
## 10                       Yoda          3.88235294
## 11                   Snowbird          3.63265306
## 12                    Robin V          3.60526316
## 13                    Wiz Kid          3.58974359
## 14                     Krypto          3.55555556
## 15                      Feral          3.50000000
## 16               Hope Summers          3.50000000
## 17                   Venom II          3.50000000
## 18                       Jolt          3.36734694
## 19                    Crystal          3.36000000
## 20               Goblin Queen          3.36000000
## 21                 Poison Ivy          3.36000000
## 22                  Shadowcat          3.36000000
## 23                Violet Parr          3.34146341
## 24                     Binary          3.33333333
## 25                    Warbird          3.33333333
## 26                    Dazzler          3.32692308
## 27                     Shriek          3.32692308
## 28                      Raven          3.30000000
## 29                  Black Cat          3.29629630
## 30                   Mystique          3.29629630
## 31                Black Mamba          3.26923077
## 32                    Polaris          3.26923077
## 33                      Armor          3.26000000
## 34                    Cheetah          3.26000000
## 35                       Wasp          3.26000000
## 36                Cheetah III          3.24074074
## 37                  Hawkwoman          3.24074074
## 38                Wonder Girl          3.23529412
## 39                  Jean Grey          3.23076923
## 40                     Mantis          3.23076923
## 41                    Phoenix          3.23076923
## 42                      Siryn          3.23076923
## 43                 Vertigo II          3.23076923
## 44                      Cloak          3.22857143
## 45                     Domino          3.20370370
## 46                      Rogue          3.20370370
## 47                Shadow Lass          3.20370370
## 48                 Batgirl IV          3.17307692
## 49                     Dagger          3.17307692
## 50                    Jubilee          3.17307692
## 51            Yellowjacket II          3.17307692
## 52                      Storm          3.15789474
## 53          Franklin Richards          3.15555556
## 54                Spider-Girl          3.14814815
## 55           Spider-Woman III          3.14545455
## 56                 Silverclaw          3.14000000
## 57               Valerie Hart          3.12500000
## 58                  Atom Girl          3.11111111
## 59            Invisible Woman          3.11111111
## 60             Naruto Uzumaki          3.11111111
## 61               Phantom Girl          3.11111111
## 62                   Vagabond          3.11111111
## 63                       X-23          3.10000000
## 64                 Cheetah II          3.09090909
## 65                   Firestar          3.08928571
## 66            Spider-Woman IV          3.06896552
## 67                     Bantam          3.05555556
## 68                 Light Lass          3.05555556
## 69                   Meltdown          3.05555556
## 70                  Supergirl          3.05555556
## 71                 Vindicator          3.05555556
## 72              Jennifer Kale          3.05454545
## 73                  Plastique          3.05454545
## 74                   Huntress          3.05084746
## 75                     Medusa          3.05084746
## 76                  Moonstone          3.05084746
## 77              Jessica Jones          3.03571429
## 78                Marvel Girl          3.03571429
## 79                   Arclight          3.03508772
## 80                      Buffy          3.01923077
## 81                   Flash IV          3.01923077
## 82            Angel Salvadore          3.01851852
## 83                    Tempest          3.01851852
## 84                   Tinkerer          3.01851852
## 85           Lady Deathstrike          3.01724138
## 86                     Oracle          3.01694915
## 87               Spider-Woman          3.01694915
## 88                  Boom-Boom          3.00000000
## 89                   Darkstar          3.00000000
## 90                 Elastigirl          3.00000000
## 91                    Batgirl          2.98245614
## 92                    Zatanna          2.98245614
## 93                    Elektra          2.96610169
## 94                     Legion          2.96610169
## 95                Enchantress          2.94736842
## 96                      Blink          2.94642857
## 97                   Firebird          2.94642857
## 98                  Red Robin          2.94642857
## 99                  Robin III          2.94642857
## 100               Spider-Gwen          2.94642857
## 101                Abe Sapien          2.93846154
## 102                      Husk          2.93103448
## 103              Miss Martian          2.91803279
## 104                     Quill          2.91071429
## 105                 Scarecrow          2.90476190
## 106                Angel Dust          2.89473684
## 107                Hawkeye II          2.89473684
## 108              Black Canary          2.88135593
## 109               Black Widow          2.88135593
## 110                 Bumblebee          2.88135593
## 111             Scarlet Witch          2.88135593
## 112                  Catwoman          2.86885246
## 113                  Hawkgirl          2.86885246
## 114               Mockingbird          2.86885246
## 115                     X-Man          2.86885246
## 116                    Aurora          2.85714286
## 117           Triplicate Girl          2.84745763
## 118              Black Canary          2.84482759
## 119                  Agent 13          2.83606557
## 120                       Cat          2.83606557
## 121                   Hellcat          2.83606557
## 122              Ms Marvel II          2.83606557
## 123                    Leader          2.82539683
## 124                      Goku          2.82258065
## 125                Spider-Man          2.80357143
## 126                     Ariel          2.79661017
## 127            Black Widow II          2.78688525
## 128                Brainiac 5          2.78688525
## 129                      Sage          2.78688525
## 130                   Arachne          2.77777778
## 131                      Beak          2.77777778
## 132                   Chamber          2.77777778
## 133                Donna Troy          2.77777778
## 134                     Vixen          2.77777778
## 135                Omniscient          2.76923077
## 136                      Nova          2.76271186
## 137                Batgirl VI          2.75409836
## 138             Cecilia Reyes          2.74193548
## 139                Emma Frost          2.73846154
## 140               White Queen          2.73846154
## 141                    Azazel          2.73134328
## 142                   Copycat          2.73134328
## 143                  Starfire          2.71830986
## 144              Harley Quinn          2.69841270
## 145                    Static          2.69841270
## 146                     Angel          2.69117647
## 147                 Archangel          2.69117647
## 148               Mr Immortal          2.68571429
## 149                 Rorschach          2.66666667
## 150                    Iceman          2.66153846
## 151                  Stargirl          2.66129032
## 152                      Atom          2.61764706
## 153                      Pyro          2.61764706
## 154             Hawkwoman III          2.61538462
## 155                   Impulse          2.61538462
## 156                      Warp          2.58208955
## 157                    Cypher          2.57352941
## 158              Multiple Man          2.57142857
## 159                  Psylocke          2.57142857
## 160                 Beast Boy          2.54411765
## 161                       Ray          2.54285714
## 162                  Songbird          2.53846154
## 163             One Punch Man          2.53623188
## 164                      Vibe          2.50704225
## 165                    Maxima          2.50000000
## 166                   Ripcord          2.50000000
## 167                  Superboy          2.50000000
## 168              Wonder Woman          2.47297297
## 169         Alfred Pennyworth          2.47222222
## 170                    Bling!          2.47058824
## 171            Captain Marvel          2.43243243
## 172                   Electro          2.43243243
## 173                Purple Man          2.43243243
## 174                     Synch          2.43243243
## 175                      Mera          2.43055556
## 176              Moses Magnum          2.43055556
## 177                     Siren          2.43055556
## 178                   Corsair          2.41772152
## 179            Scarlet Spider          2.40540541
## 180                Spider-Man          2.40540541
## 181                Karate Kid          2.40277778
## 182             Lightning Lad          2.38461538
## 183                    Morlun          2.37974684
## 184                 Star-Lord          2.37974684
## 185                   Banshee          2.37662338
## 186                    Gamora          2.37662338
## 187                 Micro Lad          2.37662338
## 188                  Callisto          2.36486486
## 189         Captain Marvel II          2.36486486
## 190           Thunderbird III          2.36486486
## 191            Superboy-Prime          2.33766234
## 192                   Big Man          2.32394366
## 193            Doctor Strange          2.32098765
## 194                  Han Solo          2.31645570
## 195                     Havok          2.31645570
## 196             Indiana Jones          2.31645570
## 197               Quicksilver          2.31645570
## 198             Elongated Man          2.31250000
## 199               Plastic Man          2.31250000
## 200                    Batman          2.31168831
## 201           Green Goblin II          2.31168831
## 202               Human Torch          2.31168831
## 203             James T. Kirk          2.31168831
## 204                Spider-Man          2.31168831
## 205                      Toad          2.30263158
## 206                    Vegeta          2.30136986
## 207                  Darkhawk          2.28395062
## 208                    Gambit          2.28395062
## 209                 Hellstorm          2.28395062
## 210          Mister Fantastic          2.28395062
## 211                     Spock          2.28395062
## 212                      Zoom          2.28395062
## 213                Clock King          2.28205128
## 214                     Joker          2.27906977
## 215                 Iron Fist          2.27848101
## 216               Kyle Rayner          2.27848101
## 217                   Mach-IV          2.27848101
## 218                  Mysterio          2.27848101
## 219                   Vulture          2.27848101
## 220               Blizzard II          2.27272727
## 221                    Hybrid          2.27272727
## 222                  Question          2.26506024
## 223                   Atom II          2.25925926
## 224                Cannonball          2.25925926
## 225                     Forge          2.25925926
## 226              Master Brood          2.25925926
## 227                  Red Hood          2.25925926
## 228                 Trickster          2.25925926
## 229                 Batman II          2.25316456
## 230                   Gravity          2.25316456
## 231           Green Goblin IV          2.25316456
## 232                     Morph          2.25316456
## 233                 Nightwing          2.25316456
## 234                     Robin          2.25316456
## 235                 Shang-Chi          2.25316456
## 236                   Sunspot          2.24675325
## 237                  Two-Face          2.23170732
## 238                    Nebula          2.22891566
## 239                Changeling          2.22222222
## 240                     Flash          2.22222222
## 241                 Heat Wave          2.22222222
## 242                       Ink          2.22222222
## 243                Power Girl          2.22222222
## 244            Professor Zoom          2.22222222
## 245                   Shocker          2.21518987
## 246                Animal Man          2.20481928
## 247                     Spyke          2.20481928
## 248              Yellowjacket          2.20481928
## 249                 Agent Bob          2.19753086
## 250                   Justice          2.19753086
## 251                     Tigra          2.19753086
## 252                   Magneto          2.18604651
## 253                    Triton          2.18604651
## 254                  Sinestro          2.18478261
## 255            Luke Skywalker          2.18181818
## 256                   Metallo          2.17777778
## 257                 Rick Flag          2.17647059
## 258                   Cyclops          2.17045455
## 259               Shatterstar          2.17045455
## 260              Green Goblin          2.16867470
## 261                 Hobgoblin          2.16867470
## 262                 Northstar          2.16867470
## 263                 Red Arrow          2.16867470
## 264                    Sentry          2.16091954
## 265                 Blackwing          2.15116279
## 266                   Carnage          2.15116279
## 267                   Chromos          2.15116279
## 268                   Genesis          2.15116279
## 269                    Metron          2.15116279
## 270                      Nova          2.15116279
## 271                     Rambo          2.14457831
## 272                      Ajax          2.14444444
## 273              Captain Atom          2.14444444
## 274               Diamondback          2.14444444
## 275                   Monarch          2.14444444
## 276                 Blackwulf          2.13636364
## 277               Green Arrow          2.13636364
## 278                Ant-Man II          2.12790698
## 279          Black Knight III          2.12790698
## 280            Blue Beetle II          2.12790698
## 281                 Flash III          2.12790698
## 282             Mister Freeze          2.12790698
## 283               Professor X          2.12790698
## 284                    Namora          2.11764706
## 285               Doctor Fate          2.11235955
## 286                  Plantman          2.10344828
## 287              Adam Strange          2.10227273
## 288                   Hawkman          2.10227273
## 289                Hal Jordan          2.08888889
## 290                  Man-Wolf          2.08888889
## 291                  Vanisher          2.08860759
## 292                    Exodus          2.07954545
## 293                  Flash II          2.07954545
## 294          Green Goblin III          2.07954545
## 295               John Wraith          2.07954545
## 296                      Hawk          2.07865169
## 297                   Century          2.07216495
## 298                 Firestorm          2.06593407
## 299                Penance II          2.05617978
## 300                  Abin Sur          2.05555556
## 301           Black Lightning          2.05555556
## 302              John Stewart          2.05555556
## 303                Metamorpho          2.05555556
## 304               Black Manta          2.04347826
## 305            Franklin Storm          2.04347826
## 306             Fabian Cortez          2.04166667
## 307             Black Panther          2.03333333
## 308                  Bullseye          2.03333333
## 309                 Daredevil          2.03333333
## 310                   Deadman          2.03333333
## 311                Goliath IV          2.03333333
## 312                  Punisher          2.03333333
## 313                  Deadshot          2.03296703
## 314                    Frenzy          2.02884615
## 315              Booster Gold          2.02061856
## 316               Steppenwolf          2.01098901
## 317            Lightning Lord          2.01052632
## 318                Alan Scott          2.00000000
## 319                 Annihilus          2.00000000
## 320              Ra's Al Ghul          1.98969072
## 321              Nightcrawler          1.98863636
## 322                   Penguin          1.98734177
## 323                    Batman          1.97894737
## 324                Black Bolt          1.97894737
## 325                  Deadpool          1.97894737
## 326             Evil Deadpool          1.97894737
## 327               Guy Gardner          1.97894737
## 328                Lex Luthor          1.97894737
## 329               Yellow Claw          1.97894737
## 330                    Ardina          1.96938776
## 331            Jack of Hearts          1.96202532
## 332                  Firelord          1.94949495
## 333                Demogoblin          1.94736842
## 334               War Machine          1.94736842
## 335                     Blade          1.93814433
## 336                      Klaw          1.93814433
## 337                  Mandarin          1.93814433
## 338                     Toxin          1.93814433
## 339                 Kraven II          1.92929293
## 340            Captain Marvel          1.91089109
## 341               Deathstroke          1.91089109
## 342             Silver Surfer          1.91089109
## 343               Ghost Rider          1.89898990
## 344                Taskmaster          1.89898990
## 345                  Superman          1.89108911
## 346              Doppelganger          1.88461538
## 347                 Nick Fury          1.86868687
## 348                      Ammo          1.86138614
## 349                     Mimic          1.86138614
## 350               Moon Knight          1.86138614
## 351              The Comedian          1.86138614
## 352            Brother Voodoo          1.84848485
## 353               Cottonmouth          1.84848485
## 354                Agent Zero          1.83653846
## 355                  Blackout          1.83653846
## 356                   Hawkeye          1.83653846
## 357                      Kang          1.83653846
## 358                     Ronin          1.83653846
## 359               Thunderbird          1.83168317
## 360                     Atlas          1.81188119
## 361                  Robin II          1.81188119
## 362                  Evilhawk          1.80188679
## 363                   Box III          1.75454545
## 364                  Maverick          1.75454545
## 365                Air-Walker          1.74074074
## 366           Captain America          1.74074074
## 367          Captain Mar-vell          1.74074074
## 368                    Falcon          1.74074074
## 369                 Red Skull          1.74074074
## 370                   Warlock          1.74074074
## 371            Crimson Dynamo          1.73076923
## 372                   Ant-Man          1.72950820
## 373         Kraven the Hunter          1.72641509
## 374         Scarlet Spider II          1.70796460
## 375           Captain Britain          1.70689655
## 376                Black Adam          1.69026549
## 377                   Aqualad          1.67924528
## 378            Wyatt Wingfoot          1.67521368
## 379                    Jigsaw          1.66371681
## 380               Space Ghost          1.66371681
## 381                  Namorita          1.66336634
## 382                   Hellboy          1.63924051
## 383             Razor-Fist II          1.63247863
## 384                     Toxin          1.63247863
## 385                     Venom          1.63247863
## 386                 Ultragirl          1.60000000
## 387                    Bishop          1.59677419
## 388            Doctor Octopus          1.59090909
## 389             Absorbing Man          1.58196721
## 390                 Hydro-Man          1.57983193
## 391                Apocalypse          1.57777778
## 392                     Atlas          1.57142857
## 393           Granny Goodness          1.54782609
## 394                     Steel          1.53435115
## 395           Mister Sinister          1.53125000
## 396            Doctor Doom II          1.52272727
## 397                     Namor          1.50400000
## 398            Winter Soldier          1.49572650
## 399         Martian Manhunter          1.48888889
## 400                Battlestar          1.48872180
## 401                     Amazo          1.48554913
## 402                  Brainiac          1.46666667
## 403               Darth Vader          1.46666667
## 404                Paul Blart          1.45299145
## 405                     Alien          1.44378698
## 406                    Vision          1.41481481
## 407                 Vision II          1.41481481
## 408                  Mephisto          1.41428571
## 409                 Big Barda          1.39259259
## 410                   Thundra          1.37974684
## 411                   Warpath          1.37974684
## 412                  Hercules          1.34246575
## 413                     Cable          1.28481013
## 414             Mr Incredible          1.27215190
## 415                   Aquaman          1.26712329
## 416               Red Tornado          1.26712329
## 417                    T-1000          1.25342466
## 418               Shadow King          1.24161074
## 419                   Bizarro          1.23225806
## 420                 Thor Girl          1.22377622
## 421                 She-Thing          1.19607843
## 422                 Wolverine          1.18518519
## 423                Doc Samson          1.15789474
## 424                Sabretooth          1.15789474
## 425                    Cyborg          1.14450867
## 426                    Walrus          1.12962963
## 427                      Bane          1.12777778
## 428                 Omega Red          1.10471204
## 429                     Skaar          1.10000000
## 430                Wonder Man          1.09941520
## 431                  Deathlok          1.08426966
## 432                     MODOK          1.08284024
## 433                    Frigga          1.07784431
## 434             Citizen Steel          1.07647059
## 435               Doctor Doom          1.07486631
## 436                  Iron Man          1.03664921
## 437                 Luke Cage          1.03664921
## 438                  Colossus          1.00444444
## 439                     Beast          0.99447514
## 440                   Kingpin          0.99014778
## 441                       Sif          0.98429319
## 442                   Etrigan          0.95073892
## 443                      Hela          0.94666667
## 444                 Man-Thing          0.94666667
## 445             Beta Ray Bill          0.93055556
## 446                   Sandman          0.91133005
## 447               Tiger Shark          0.91133005
## 448                  Predator          0.91025641
## 449                  Valkyrie          0.89252336
## 450                  Hyperion          0.88405797
## 451                    Lizard          0.88260870
## 452                      Loki          0.81779661
## 453                     Thing          0.81333333
## 454                      Lobo          0.79513889
## 455                Molten Man          0.79032258
## 456                      Blob          0.77391304
## 457                 Wolfsbane          0.77378436
## 458                 Onslaught          0.75308642
## 459                 Gladiator          0.73880597
## 460             Gorilla Grodd          0.73333333
## 461                   Kilowog          0.72222222
## 462                      Odin          0.70307167
## 463                      Thor          0.68750000
## 464             Thunderstrike          0.68750000
## 465                 Venom III          0.68562874
## 466               Killer Croc          0.68539326
## 467                      Ares          0.68518519
## 468                  Scorpion          0.68064516
## 469                Anti-Venom          0.63966480
## 470            Solomon Grundy          0.63844394
## 471                  She-Hulk          0.63809524
## 472        Drax the Destroyer          0.63071895
## 473                    Ultron          0.62235650
## 474                     Rhino          0.61250000
## 475                  Doomsday          0.59223301
## 476              Living Brain          0.55000000
## 477                     Spawn          0.52098765
## 478                 Destroyer          0.49086162
## 479               Machine Man          0.47780679
## 480                    A-Bomb          0.46031746
## 481               Abomination          0.46031746
## 482                    Thanos          0.45372460
## 483                  Bloodaxe          0.44040404
## 484                      Hulk          0.38730159
## 485                 Sasquatch          0.33888889
## 486                  Red Hulk          0.33809524
## 487                Juggernaut          0.33567251
## 488                  Darkseid          0.32680539
## 489               Utgard-Loki          0.26206897
## 490                   Giganta          0.09920635
## 491                   Abraxas                  NA
## 492               Adam Monroe                  NA
## 493               Alex Mercer                  NA
## 494              Alex Woolsly                  NA
## 495          Allan Quatermain                  NA
## 496            Ando Masahashi                  NA
## 497                     Angel                  NA
## 498                    Angela                  NA
## 499              Anti-Monitor                  NA
## 500                Anti-Spawn                  NA
## 501                  Aquababy                  NA
## 502                   Arsenal                  NA
## 503                 Astro Boy                  NA
## 504                      Atom                  NA
## 505                  Atom III                  NA
## 506                   Atom IV                  NA
## 507                    Azrael                  NA
## 508                     Aztar                  NA
## 509                   Batgirl                  NA
## 510               Batgirl III                  NA
## 511                 Batgirl V                  NA
## 512                Batwoman V                  NA
## 513                    Beetle                  NA
## 514                    Ben 10                  NA
## 515                  Beyonder                  NA
## 516                 Big Daddy                  NA
## 517               Bill Harken                  NA
## 518             Billy Kincaid                  NA
## 519              Bionic Woman                  NA
## 520                Bird-Brain                  NA
## 521                  Bird-Man                  NA
## 522               Bird-Man II                  NA
## 523                   Birdman                  NA
## 524              Black Abbott                  NA
## 525               Black Flash                  NA
## 526             Black Goliath                  NA
## 527               Blaquesmith                  NA
## 528                  Blizzard                  NA
## 529                  Blizzard                  NA
## 530                 Bloodhawk                  NA
## 531               Bloodwraith                  NA
## 532               Blue Beetle                  NA
## 533               Blue Beetle                  NA
## 534           Blue Beetle III                  NA
## 535                 Boba Fett                  NA
## 536                      Bolt                  NA
## 537                Bomb Queen                  NA
## 538                    Boomer                  NA
## 539                       Box                  NA
## 540                    Box IV                  NA
## 541                Brundlefly                  NA
## 542                 Bumbleboy                  NA
## 543                   Bushido                  NA
## 544             Cameron Hicks                  NA
## 545              Captain Cold                  NA
## 546              Captain Epic                  NA
## 547         Captain Hindsight                  NA
## 548          Captain Midnight                  NA
## 549            Captain Planet                  NA
## 550          Captain Universe                  NA
## 551                    Cat II                  NA
## 552                   Cerebra                  NA
## 553                 Chameleon                  NA
## 554              Chuck Norris                  NA
## 555             Claire Bennet                  NA
## 556                      Clea                  NA
## 557                Cogliostro                  NA
## 558              Colin Wagner                  NA
## 559              Colossal Boy                  NA
## 560          Crimson Crusader                  NA
## 561                     Curse                  NA
## 562                    Cy-Gor                  NA
## 563           Cyborg Superman                  NA
## 564              Danny Cooper                  NA
## 565             Daphne Powell                  NA
## 566                   Darkman                  NA
## 567                  Darkside                  NA
## 568                Darth Maul                  NA
## 569                      Data                  NA
## 570                DL Hawkins                  NA
## 571                 Donatello                  NA
## 572                  Dormammu                  NA
## 573              Dr Manhattan                  NA
## 574                       Ego                  NA
## 575               Elle Bishop                  NA
## 576                    Energy                  NA
## 577                     ERG-1                  NA
## 578                Ethan Hunt                  NA
## 579             Fallen One II                  NA
## 580                     Faora                  NA
## 581           Fighting Spirit                  NA
## 582                 Firestorm                  NA
## 583                     Fixer                  NA
## 584              Flash Gordon                  NA
## 585               Garbage Man                  NA
## 586                 Gary Bell                  NA
## 587               General Zod                  NA
## 588            Ghost Rider II                  NA
## 589                 Giant-Man                  NA
## 590              Giant-Man II                  NA
## 591                  Godzilla                  NA
## 592                       Gog                  NA
## 593                   Goliath                  NA
## 594                   Goliath                  NA
## 595                   Goliath                  NA
## 596                    Greedo                  NA
## 597                  Guardian                  NA
## 598                   Hancock                  NA
## 599              Harry Potter                  NA
## 600              Hawkwoman II                  NA
## 601             Hiro Nakamura                  NA
## 602                  Hit-Girl                  NA
## 603                    Hollow                  NA
## 604                    Indigo                  NA
## 605               Iron Monger                  NA
## 606                      Isis                  NA
## 607                Jack Bauer                  NA
## 608                James Bond                  NA
## 609             Jar Jar Binks                  NA
## 610              Jason Bourne                  NA
## 611           Jean-Luc Picard                  NA
## 612               Jesse Quick                  NA
## 613              Jessica Cruz                  NA
## 614           Jessica Sanders                  NA
## 615                Jim Powell                  NA
## 616                 JJ Powell                  NA
## 617             Johann Krauss                  NA
## 618          John Constantine                  NA
## 619               Judge Dredd                  NA
## 620                  Junkpile                  NA
## 621                  Jyn Erso                  NA
## 622                     K-2SO                  NA
## 623           Kathryn Janeway                  NA
## 624          Katniss Everdeen                  NA
## 625                  Kevin 11                  NA
## 626                  Kick-Ass                  NA
## 627                 Kid Flash                  NA
## 628              Kid Flash II                  NA
## 629              Killer Frost                  NA
## 630                 King Kong                  NA
## 631                King Shark                  NA
## 632              Kool-Aid Man                  NA
## 633                  Kylo Ren                  NA
## 634             Lady Bullseye                  NA
## 635                     Leech                  NA
## 636                  Leonardo                  NA
## 637           Living Tribunal                  NA
## 638               Liz Sherman                  NA
## 639             Luke Campbell                  NA
## 640                      Luna                  NA
## 641                      Lyja                  NA
## 642                     Magog                  NA
## 643                     Magus                  NA
## 644           Man of Miracles                  NA
## 645                   Man-Bat                  NA
## 646              Master Chief                  NA
## 647                     Match                  NA
## 648              Matt Parkman                  NA
## 649              Maya Herrera                  NA
## 650                 Meteorite                  NA
## 651             Micah Sanders                  NA
## 652              Michelangelo                  NA
## 653              Minna Murray                  NA
## 654                    Misfit                  NA
## 655              Mister Knife                  NA
## 656           Mister Mxyzptlk                  NA
## 657              Mister Zsasz                  NA
## 658                      Mogo                  NA
## 659           Mohinder Suresh                  NA
## 660                    Moloch                  NA
## 661             Monica Dawson                  NA
## 662                     Namor                  NA
## 663           Nathan Petrelli                  NA
## 664 Negasonic Teenage Warhead                  NA
## 665              Niki Sanders                  NA
## 666              Nina Theroux                  NA
## 667               Nite Owl II                  NA
## 668                 Offspring                  NA
## 669             One-Above-All                  NA
## 670                    Osiris                  NA
## 671                 Overtkill                  NA
## 672                Ozymandias                  NA
## 673                 Parademon                  NA
## 674                   Penance                  NA
## 675                 Penance I                  NA
## 676            Peter Petrelli                  NA
## 677                   Phantom                  NA
## 678               Plastic Lad                  NA
## 679                 Power Man                  NA
## 680              Proto-Goblin                  NA
## 681                         Q                  NA
## 682                   Quantum                  NA
## 683             Rachel Pirzad                  NA
## 684                   Raphael                  NA
## 685                  Red Mist                  NA
## 686               Redeemer II                  NA
## 687              Redeemer III                  NA
## 688              Renata Soliz                  NA
## 689                       Rey                  NA
## 690                   Riddler                  NA
## 691                Rip Hunter                  NA
## 692                  Robin VI                  NA
## 693                    Sauron                  NA
## 694             Savage Dragon                  NA
## 695                   Scorpia                  NA
## 696            Sebastian Shaw                  NA
## 697          Shrinking Violet                  NA
## 698                      Silk                  NA
## 699              Silk Spectre                  NA
## 700           Silk Spectre II                  NA
## 701                 Simon Baz                  NA
## 702                  Siren II                  NA
## 703                Snake-Eyes                  NA
## 704                     Sobek                  NA
## 705                   Spectre                  NA
## 706                 Speedball                  NA
## 707                    Speedy                  NA
## 708                    Speedy                  NA
## 709            Spider-Carnage                  NA
## 710           Spider-Woman II                  NA
## 711                   Stacy X                  NA
## 712                  Stardust                  NA
## 713          Stephanie Powell                  NA
## 714              Stormtrooper                  NA
## 715               Swamp Thing                  NA
## 716                     Sylar                  NA
## 717                     T-800                  NA
## 718                     T-850                  NA
## 719                       T-X                  NA
## 720                  The Cape                  NA
## 721            Thunderbird II                  NA
## 722                     Titan                  NA
## 723             Tracy Strauss                  NA
## 724                    Trigon                  NA
## 725                 Venompool                  NA
## 726                Vindicator                  NA
## 727                  Violator                  NA
## 728                    Vulcan                  NA
## 729                   Watcher                  NA
## 730                 Weapon XI                  NA
## 731              White Canary                  NA
## 732                  Wildfire                  NA
## 733                    Wondra                  NA
## 734                      Ymir                  NA
```

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

13. How many superheros have a combination of agility, stealth, super_strength, stamina?


```r
superhero_powers%>%
  filter(Agility == "True" & Stealth == "True" & Super.Strength == "True" & Stamina == "True")
```

```
##           hero_names Agility Accelerated.Healing Lantern.Power.Ring
## 1        Alex Mercer    True                True              False
## 2              Angel    True                True              False
## 3         Ant-Man II    True               False              False
## 4            Aquaman    True                True              False
## 5             Batman    True               False              False
## 6        Black Flash    True               False              False
## 7        Black Manta    True               False              False
## 8         Brundlefly    True               False              False
## 9              Buffy    True                True              False
## 10             Cable    True                True              False
## 11   Captain Britain    True               False              False
## 12       Cheetah III    True               False              False
## 13        Darth Maul    True               False              False
## 14            Gamora    True                True              False
## 15      Harley Quinn    True                True              False
## 16            Hybrid    True                True              False
## 17         King Kong    True               False              False
## 18  Lady Deathstrike    True                True              False
## 19            Legion    True                True              False
## 20 Martian Manhunter    True               False              False
## 21          Mystique    True                True              False
## 22    Naruto Uzumaki    True                True              False
## 23              Odin    True                True              False
## 24             Orion    True                True              False
## 25        Sabretooth    True                True              False
## 26              Silk    True                True              False
## 27             Spawn    True               False              False
## 28           Spectre    True                True              False
## 29       Spider-Girl    True                True              False
## 30       Spider-Gwen    True                True              False
## 31        Spider-Man    True                True              False
## 32       Steppenwolf    True                True              False
## 33            T-1000    True                True              False
## 34               T-X    True                True              False
## 35             Toxin    True                True              False
## 36            Ultron    True               False              False
## 37         Venompool    True                True              False
## 38             Vixen    True                True              False
## 39         Wolverine    True                True              False
## 40              X-23    True                True              False
##    Dimensional.Awareness Cold.Resistance Durability Stealth Energy.Absorption
## 1                  False           False      False    True              True
## 2                  False           False       True    True             False
## 3                  False           False      False    True             False
## 4                  False            True       True    True             False
## 5                  False           False       True    True             False
## 6                  False           False      False    True             False
## 7                  False            True       True    True             False
## 8                  False           False      False    True             False
## 9                  False           False       True    True             False
## 10                 False           False       True    True             False
## 11                 False           False       True    True             False
## 12                 False           False       True    True             False
## 13                 False           False       True    True             False
## 14                 False           False       True    True             False
## 15                 False           False      False    True             False
## 16                 False           False       True    True             False
## 17                 False           False       True    True             False
## 18                 False           False       True    True             False
## 19                 False           False      False    True             False
## 20                 False           False       True    True             False
## 21                 False           False      False    True             False
## 22                 False           False      False    True             False
## 23                 False           False       True    True              True
## 24                 False           False       True    True             False
## 25                 False            True       True    True             False
## 26                 False           False       True    True             False
## 27                 False           False       True    True             False
## 28                  True           False      False    True              True
## 29                 False           False      False    True             False
## 30                 False           False       True    True             False
## 31                 False           False       True    True             False
## 32                 False           False      False    True             False
## 33                 False            True       True    True             False
## 34                 False            True       True    True             False
## 35                 False           False       True    True             False
## 36                 False           False       True    True              True
## 37                  True           False       True    True             False
## 38                 False           False      False    True             False
## 39                 False            True       True    True             False
## 40                 False           False       True    True             False
##    Flight Danger.Sense Underwater.breathing Marksmanship Weapons.Master
## 1   False         True                False         True           True
## 2    True        False                False        False          False
## 3   False        False                False        False          False
## 4   False        False                 True        False          False
## 5   False        False                 True         True           True
## 6    True        False                False        False          False
## 7   False        False                 True         True           True
## 8   False        False                False        False          False
## 9   False         True                False        False          False
## 10   True        False                False         True           True
## 11   True        False                False        False          False
## 12  False        False                False        False          False
## 13  False         True                False         True           True
## 14  False        False                False         True           True
## 15  False        False                False         True          False
## 16   True         True                False         True          False
## 17  False        False                False        False          False
## 18  False        False                False        False          False
## 19   True        False                False        False          False
## 20   True        False                False         True          False
## 21  False        False                False         True           True
## 22  False        False                False         True           True
## 23  False        False                False        False          False
## 24   True        False                False         True           True
## 25  False        False                False         True           True
## 26  False         True                False         True          False
## 27   True        False                False        False           True
## 28   True        False                False         True          False
## 29  False         True                False        False          False
## 30  False         True                False         True          False
## 31  False         True                False         True          False
## 32  False        False                False        False           True
## 33  False        False                False         True           True
## 34  False        False                False         True           True
## 35  False         True                False        False          False
## 36   True        False                False        False          False
## 37  False         True                False         True           True
## 38   True         True                False        False          False
## 39  False        False                False        False          False
## 40  False        False                False         True          False
##    Power.Augmentation Animal.Attributes Longevity Intelligence Super.Strength
## 1               False             False      True        False           True
## 2               False             False      True        False           True
## 3               False             False     False         True           True
## 4               False             False      True         True           True
## 5               False             False     False         True           True
## 6               False             False     False        False           True
## 7               False             False     False        False           True
## 8               False             False     False        False           True
## 9               False             False     False        False           True
## 10              False             False     False         True           True
## 11              False             False     False        False           True
## 12              False              True     False         True           True
## 13              False             False     False        False           True
## 14              False             False     False         True           True
## 15              False             False     False         True           True
## 16              False             False     False        False           True
## 17              False             False      True        False           True
## 18              False             False     False         True           True
## 19              False             False     False        False           True
## 20              False             False     False         True           True
## 21              False             False      True         True           True
## 22              False             False      True        False           True
## 23              False             False      True         True           True
## 24              False             False     False         True           True
## 25              False             False      True        False           True
## 26              False              True     False        False           True
## 27              False             False     False        False           True
## 28              False             False      True         True           True
## 29              False             False     False         True           True
## 30              False              True     False        False           True
## 31              False              True     False        False           True
## 32              False             False      True         True           True
## 33              False             False      True         True           True
## 34              False             False      True         True           True
## 35              False             False     False        False           True
## 36              False             False     False        False           True
## 37              False             False      True        False           True
## 38              False              True     False        False           True
## 39              False             False      True        False           True
## 40              False             False      True        False           True
##    Cryokinesis Telepathy Energy.Armor Energy.Blasts Duplication Size.Changing
## 1        False     False        False          True       False         False
## 2        False     False        False         False       False         False
## 3        False      True        False         False       False          True
## 4        False      True        False         False       False         False
## 5        False     False        False         False       False         False
## 6        False     False        False         False       False         False
## 7        False     False        False          True       False         False
## 8        False     False        False         False       False         False
## 9        False     False        False         False       False         False
## 10       False      True        False          True       False         False
## 11       False     False        False         False       False         False
## 12       False     False        False         False       False         False
## 13       False      True        False         False       False         False
## 14       False     False        False         False       False         False
## 15       False     False        False         False       False         False
## 16       False     False        False         False       False         False
## 17       False     False        False         False       False         False
## 18       False     False        False         False       False         False
## 19       False      True        False         False        True         False
## 20       False      True        False         False       False          True
## 21       False     False        False         False       False         False
## 22       False     False        False         False        True         False
## 23        True      True        False          True       False          True
## 24       False     False        False          True       False         False
## 25       False     False        False         False       False         False
## 26       False     False        False         False       False         False
## 27       False      True        False         False       False         False
## 28       False      True        False          True        True          True
## 29       False     False        False         False       False         False
## 30       False     False        False         False       False         False
## 31       False     False        False         False       False         False
## 32       False     False        False         False       False         False
## 33       False     False        False         False       False          True
## 34       False     False        False          True       False         False
## 35       False     False        False         False       False          True
## 36       False     False         True          True       False         False
## 37       False     False        False         False       False         False
## 38       False     False        False         False       False         False
## 39       False     False        False         False       False         False
## 40       False     False        False         False       False         False
##    Density.Control Stamina Astral.Travel Audio.Control Dexterity Omnitrix
## 1            False    True         False         False     False    False
## 2            False    True         False         False     False    False
## 3            False    True         False         False     False    False
## 4            False    True         False         False     False    False
## 5            False    True         False         False     False    False
## 6            False    True         False         False     False    False
## 7            False    True         False         False     False    False
## 8            False    True         False         False     False    False
## 9            False    True         False         False     False    False
## 10           False    True         False         False      True    False
## 11           False    True         False         False     False    False
## 12           False    True         False         False     False    False
## 13           False    True         False         False     False    False
## 14           False    True         False         False     False    False
## 15           False    True         False         False     False    False
## 16           False    True         False         False     False    False
## 17           False    True         False         False     False    False
## 18           False    True         False         False     False    False
## 19           False    True         False         False     False    False
## 20           False    True         False         False     False    False
## 21           False    True         False         False     False    False
## 22           False    True         False         False     False    False
## 23           False    True         False         False     False    False
## 24           False    True         False         False     False    False
## 25           False    True         False         False     False    False
## 26           False    True         False         False     False    False
## 27           False    True         False         False     False    False
## 28            True    True         False         False     False    False
## 29           False    True         False         False      True    False
## 30           False    True         False         False     False    False
## 31           False    True         False         False     False    False
## 32           False    True         False         False     False    False
## 33           False    True         False         False     False    False
## 34           False    True         False         False     False    False
## 35           False    True         False         False     False    False
## 36           False    True         False         False     False    False
## 37           False    True         False         False     False    False
## 38           False    True         False         False     False    False
## 39           False    True         False         False     False    False
## 40           False    True         False         False     False    False
##    Super.Speed Possession Animal.Oriented.Powers Weapon.based.Powers
## 1         True      False                  False               False
## 2         True      False                  False               False
## 3        False      False                   True               False
## 4         True      False                  False               False
## 5         True      False                  False                True
## 6         True      False                  False               False
## 7         True      False                  False               False
## 8        False      False                  False               False
## 9         True      False                  False               False
## 10        True      False                  False               False
## 11        True      False                  False               False
## 12        True      False                   True               False
## 13       False      False                  False               False
## 14        True      False                  False               False
## 15       False      False                  False               False
## 16        True      False                  False               False
## 17        True      False                  False               False
## 18        True      False                  False               False
## 19        True      False                   True               False
## 20        True      False                  False               False
## 21       False      False                  False               False
## 22        True      False                  False               False
## 23        True      False                  False               False
## 24       False      False                  False               False
## 25        True      False                   True               False
## 26        True      False                   True               False
## 27        True      False                  False               False
## 28        True       True                  False               False
## 29        True      False                  False               False
## 30        True      False                   True               False
## 31        True      False                   True               False
## 32        True      False                  False               False
## 33        True      False                  False               False
## 34        True      False                  False               False
## 35        True      False                  False                True
## 36        True      False                  False               False
## 37        True      False                  False                True
## 38        True      False                   True               False
## 39       False      False                   True               False
## 40       False      False                  False               False
##    Electrokinesis Darkforce.Manipulation Death.Touch Teleportation
## 1           False                  False       False         False
## 2           False                  False       False         False
## 3           False                  False       False         False
## 4           False                  False       False         False
## 5           False                  False       False         False
## 6           False                  False        True         False
## 7           False                  False       False         False
## 8           False                  False       False         False
## 9           False                  False       False         False
## 10          False                  False       False          True
## 11          False                  False       False         False
## 12          False                  False       False         False
## 13           True                   True       False         False
## 14          False                  False       False         False
## 15          False                  False       False         False
## 16          False                  False       False         False
## 17          False                  False       False         False
## 18          False                  False       False         False
## 19          False                  False       False          True
## 20          False                  False       False         False
## 21          False                  False       False         False
## 22          False                  False       False          True
## 23          False                  False       False          True
## 24          False                  False       False         False
## 25          False                  False       False         False
## 26          False                  False       False         False
## 27          False                  False       False          True
## 28           True                   True        True          True
## 29          False                  False       False         False
## 30          False                  False       False         False
## 31          False                  False       False         False
## 32          False                  False       False         False
## 33          False                  False       False         False
## 34          False                  False       False         False
## 35          False                  False       False         False
## 36          False                  False       False         False
## 37          False                  False       False          True
## 38          False                  False       False         False
## 39          False                  False       False         False
## 40          False                  False       False         False
##    Enhanced.Senses Telekinesis Energy.Beams Magic Hyperkinesis  Jump
## 1             True       False        False False        False  True
## 2             True       False        False  True        False False
## 3            False       False        False False        False False
## 4             True       False        False False        False False
## 5            False       False        False False        False False
## 6             True       False        False False        False False
## 7            False       False        False False        False  True
## 8            False       False        False False        False False
## 9            False       False        False False        False False
## 10           False        True        False False        False False
## 11            True       False        False False        False False
## 12            True       False        False False        False  True
## 13           False        True        False False        False False
## 14            True       False        False False        False False
## 15           False       False        False False        False False
## 16           False       False        False False        False  True
## 17           False       False        False False        False  True
## 18           False       False        False False        False False
## 19           False        True        False False        False False
## 20            True        True        False False        False False
## 21           False       False        False False        False False
## 22           False       False        False  True        False False
## 23           False        True        False  True        False False
## 24            True       False        False  True        False False
## 25           False       False        False False        False False
## 26           False       False        False False        False  True
## 27           False       False        False  True        False False
## 28           False       False        False  True        False False
## 29           False       False        False False        False  True
## 30           False       False        False False        False  True
## 31            True       False        False False        False  True
## 32            True       False        False False        False False
## 33            True       False        False False        False False
## 34            True       False         True False        False False
## 35           False       False        False False        False  True
## 36            True       False         True False        False False
## 37           False       False        False False        False  True
## 38           False       False        False False        False False
## 39            True       False        False False        False  True
## 40           False       False        False False        False  True
##    Clairvoyance Dimensional.Travel Power.Sense Shapeshifting
## 1         False              False       False          True
## 2         False              False       False         False
## 3         False              False       False         False
## 4         False              False       False         False
## 5         False              False       False         False
## 6         False               True       False         False
## 7         False              False       False         False
## 8         False              False       False         False
## 9         False              False       False         False
## 10        False              False       False         False
## 11        False              False       False         False
## 12        False              False       False          True
## 13        False              False       False         False
## 14        False              False       False         False
## 15        False              False       False         False
## 16        False              False       False          True
## 17        False              False       False         False
## 18        False              False       False         False
## 19        False              False       False          True
## 20        False              False       False          True
## 21        False              False       False          True
## 22        False              False       False          True
## 23        False              False       False          True
## 24        False              False       False         False
## 25        False              False       False         False
## 26        False              False       False         False
## 27        False              False       False          True
## 28        False              False       False         False
## 29        False              False       False         False
## 30        False              False       False         False
## 31        False              False       False         False
## 32        False              False       False         False
## 33        False              False       False          True
## 34        False              False       False          True
## 35        False              False       False          True
## 36        False              False       False         False
## 37        False              False       False         False
## 38        False              False       False          True
## 39        False              False       False         False
## 40        False              False       False         False
##    Peak.Human.Condition Immortality Camouflage Element.Control Phasing
## 1                 False        True      False           False   False
## 2                 False        True      False           False   False
## 3                 False       False      False           False   False
## 4                 False       False      False           False   False
## 5                  True       False      False           False   False
## 6                 False        True      False           False   False
## 7                  True       False       True           False   False
## 8                 False       False      False           False   False
## 9                 False       False      False           False   False
## 10                False       False      False           False   False
## 11                False       False      False           False   False
## 12                False       False      False           False   False
## 13                False       False      False           False   False
## 14                False       False      False           False   False
## 15                 True       False      False           False   False
## 16                False       False       True           False   False
## 17                False       False      False           False   False
## 18                False       False      False           False   False
## 19                False       False      False           False   False
## 20                False       False      False           False    True
## 21                False       False      False           False   False
## 22                False       False       True           False   False
## 23                False       False      False           False   False
## 24                False        True      False           False   False
## 25                False       False      False           False   False
## 26                False       False      False           False   False
## 27                False        True      False           False   False
## 28                False        True      False           False    True
## 29                False       False      False           False   False
## 30                False       False      False           False   False
## 31                False       False      False           False   False
## 32                False        True      False           False   False
## 33                False       False       True           False   False
## 34                False       False       True           False   False
## 35                False       False       True           False   False
## 36                False        True      False           False   False
## 37                False        True      False           False   False
## 38                False       False      False           False   False
## 39                False       False      False           False   False
## 40                False       False      False           False   False
##    Astral.Projection Electrical.Transport Fire.Control Projection Summoning
## 1              False                False        False      False     False
## 2              False                False        False      False     False
## 3              False                False        False      False     False
## 4              False                False        False      False     False
## 5              False                False        False      False     False
## 6              False                False        False      False     False
## 7              False                False        False      False     False
## 8              False                False        False      False     False
## 9              False                False        False      False     False
## 10              True                False        False      False     False
## 11             False                False        False      False     False
## 12             False                False        False      False     False
## 13             False                False        False      False     False
## 14             False                False        False      False     False
## 15             False                False        False      False     False
## 16             False                False        False      False     False
## 17             False                False        False      False     False
## 18             False                False        False      False     False
## 19              True                False         True      False     False
## 20              True                False        False      False     False
## 21             False                False        False      False     False
## 22             False                False         True      False     False
## 23             False                False        False      False     False
## 24             False                False        False      False     False
## 25             False                False        False      False     False
## 26             False                False        False      False     False
## 27             False                False        False      False     False
## 28              True                False         True      False     False
## 29             False                False        False      False     False
## 30             False                False        False      False     False
## 31             False                False        False      False     False
## 32             False                False        False      False     False
## 33             False                False        False      False     False
## 34             False                False        False      False     False
## 35             False                False        False      False     False
## 36             False                False        False      False     False
## 37             False                False        False      False     False
## 38             False                False        False      False     False
## 39             False                False        False      False     False
## 40             False                False        False      False     False
##    Enhanced.Memory Reflexes Invulnerability Energy.Constructs Force.Fields
## 1            False    False            True             False        False
## 2             True    False           False             False        False
## 3            False    False           False             False        False
## 4            False     True            True             False        False
## 5            False     True           False             False        False
## 6            False    False           False             False        False
## 7            False    False           False             False        False
## 8            False    False           False             False        False
## 9             True     True           False             False        False
## 10           False    False           False             False         True
## 11           False     True           False             False         True
## 12           False     True           False             False        False
## 13           False    False           False             False        False
## 14           False     True           False             False        False
## 15           False     True           False             False        False
## 16           False     True           False             False        False
## 17           False     True           False             False        False
## 18           False     True           False             False        False
## 19           False    False           False             False         True
## 20           False     True            True             False        False
## 21           False    False           False             False        False
## 22           False    False           False              True        False
## 23           False     True            True             False         True
## 24           False    False           False             False        False
## 25           False     True           False             False        False
## 26           False     True           False             False        False
## 27           False    False            True             False        False
## 28           False    False            True              True         True
## 29           False     True           False             False        False
## 30           False     True           False             False        False
## 31           False     True           False             False        False
## 32           False     True            True             False        False
## 33            True     True            True             False        False
## 34            True     True            True             False        False
## 35           False     True           False             False        False
## 36            True    False           False             False         True
## 37           False     True           False             False        False
## 38           False    False           False             False         True
## 39           False     True           False             False        False
## 40           False     True           False             False        False
##    Self.Sustenance Anti.Gravity Empathy Power.Nullifier Radiation.Control
## 1            False        False   False           False             False
## 2            False        False   False           False             False
## 3            False        False   False           False             False
## 4            False        False   False           False             False
## 5            False        False   False           False             False
## 6            False        False   False           False             False
## 7            False        False   False           False             False
## 8            False        False   False           False             False
## 9            False        False   False           False             False
## 10           False        False   False           False             False
## 11           False        False   False           False             False
## 12           False        False   False           False             False
## 13           False        False   False           False             False
## 14           False        False   False           False             False
## 15           False        False   False           False             False
## 16           False        False   False           False             False
## 17           False        False   False           False             False
## 18           False        False   False           False             False
## 19           False        False   False            True             False
## 20            True        False   False           False             False
## 21           False        False   False           False             False
## 22           False        False    True           False             False
## 23           False        False   False           False             False
## 24           False        False   False           False             False
## 25           False        False   False           False             False
## 26           False        False   False           False             False
## 27           False        False    True           False             False
## 28           False        False   False           False             False
## 29           False        False   False           False             False
## 30           False        False   False           False             False
## 31           False        False   False           False             False
## 32           False        False   False           False             False
## 33           False        False   False           False             False
## 34           False        False   False           False             False
## 35           False        False   False           False             False
## 36           False        False   False           False              True
## 37           False        False   False           False             False
## 38           False        False   False           False             False
## 39           False        False    True           False             False
## 40           False        False   False           False             False
##    Psionic.Powers Elasticity Substance.Secretion Elemental.Transmogrification
## 1           False       True                True                        False
## 2            True      False               False                        False
## 3           False      False               False                        False
## 4           False      False               False                        False
## 5           False      False               False                        False
## 6           False      False               False                        False
## 7           False      False               False                        False
## 8           False      False               False                        False
## 9            True      False               False                        False
## 10           True      False               False                        False
## 11          False      False               False                        False
## 12          False      False               False                        False
## 13          False      False               False                        False
## 14          False      False               False                        False
## 15          False      False               False                        False
## 16          False      False                True                        False
## 17          False      False               False                        False
## 18          False      False               False                        False
## 19           True      False               False                        False
## 20          False      False               False                        False
## 21          False      False               False                        False
## 22          False      False               False                        False
## 23          False      False               False                        False
## 24          False      False               False                        False
## 25          False      False               False                        False
## 26          False      False                True                        False
## 27          False      False               False                        False
## 28           True       True               False                        False
## 29          False      False               False                        False
## 30          False      False                True                        False
## 31          False      False                True                        False
## 32          False      False               False                        False
## 33          False      False               False                        False
## 34          False      False               False                        False
## 35          False      False                True                        False
## 36          False      False               False                        False
## 37          False      False               False                        False
## 38          False      False               False                        False
## 39          False      False               False                        False
## 40          False      False               False                        False
##    Technopath.Cyberpath Photographic.Reflexes Seismic.Power Animation
## 1                 False                 False         False     False
## 2                 False                 False         False     False
## 3                 False                 False         False     False
## 4                 False                 False         False     False
## 5                 False                 False         False     False
## 6                 False                 False         False     False
## 7                 False                 False         False     False
## 8                 False                 False         False     False
## 9                 False                 False         False     False
## 10                 True                 False         False     False
## 11                False                 False         False     False
## 12                False                 False         False     False
## 13                False                 False         False     False
## 14                False                 False         False     False
## 15                False                 False         False     False
## 16                False                 False         False     False
## 17                False                 False         False     False
## 18                 True                 False         False     False
## 19                False                 False         False     False
## 20                False                 False         False     False
## 21                False                 False         False     False
## 22                False                 False         False     False
## 23                False                 False         False     False
## 24                False                 False         False     False
## 25                False                 False         False     False
## 26                False                 False         False     False
## 27                False                 False         False     False
## 28                False                 False         False      True
## 29                False                 False         False     False
## 30                False                 False         False     False
## 31                False                 False         False     False
## 32                False                 False         False     False
## 33                 True                 False         False     False
## 34                 True                 False         False     False
## 35                False                 False         False     False
## 36                 True                 False         False     False
## 37                False                 False         False     False
## 38                False                 False         False     False
## 39                False                 False         False     False
## 40                False                 False         False     False
##    Precognition Mind.Control Fire.Resistance Power.Absorption Enhanced.Hearing
## 1         False        False           False            False             True
## 2         False        False           False            False             True
## 3         False        False           False            False            False
## 4         False        False           False            False             True
## 5         False        False           False            False            False
## 6         False        False           False            False            False
## 7         False        False           False            False            False
## 8         False        False           False            False            False
## 9         False        False           False            False            False
## 10         True         True           False            False            False
## 11        False        False           False            False            False
## 12        False        False           False            False             True
## 13        False        False           False            False            False
## 14        False        False           False            False            False
## 15        False        False           False            False            False
## 16        False        False           False            False            False
## 17        False        False           False            False             True
## 18        False        False           False            False            False
## 19        False        False           False             True            False
## 20        False         True           False            False             True
## 21        False        False           False            False            False
## 22        False        False           False            False            False
## 23         True        False           False            False            False
## 24        False        False           False            False             True
## 25        False        False           False            False             True
## 26        False        False           False            False            False
## 27        False        False           False            False            False
## 28         True        False           False            False            False
## 29        False        False           False            False             True
## 30        False        False           False            False            False
## 31        False        False           False            False            False
## 32        False        False           False            False            False
## 33        False        False            True            False             True
## 34        False        False            True            False             True
## 35        False        False           False            False            False
## 36        False         True           False            False             True
## 37        False        False           False            False            False
## 38        False        False           False            False             True
## 39        False        False           False            False             True
## 40        False        False           False            False             True
##    Nova.Force Insanity Hypnokinesis Animal.Control Natural.Armor Intangibility
## 1       False    False        False          False          True         False
## 2       False    False        False          False         False         False
## 3       False    False        False           True         False         False
## 4       False    False        False           True         False         False
## 5       False    False        False          False         False         False
## 6       False    False        False          False         False          True
## 7       False    False        False          False         False         False
## 8       False    False        False          False         False         False
## 9       False    False        False          False         False         False
## 10      False    False        False          False         False         False
## 11      False    False        False          False         False         False
## 12      False    False        False           True         False         False
## 13      False    False        False          False         False         False
## 14      False    False        False          False         False         False
## 15      False    False        False          False         False         False
## 16      False    False        False          False          True         False
## 17      False    False        False          False         False         False
## 18      False    False        False          False         False         False
## 19      False    False        False          False         False         False
## 20      False    False        False          False         False          True
## 21      False    False        False          False         False         False
## 22      False    False        False          False         False         False
## 23      False    False        False          False         False         False
## 24      False    False        False          False         False         False
## 25      False    False        False          False         False         False
## 26      False    False        False          False         False         False
## 27      False    False        False          False         False         False
## 28      False    False         True           True         False         False
## 29      False    False        False          False         False         False
## 30      False    False        False          False         False         False
## 31      False    False        False          False         False         False
## 32      False    False        False          False         False         False
## 33      False    False        False          False         False         False
## 34      False    False        False          False         False         False
## 35      False    False        False          False          True         False
## 36      False    False        False          False         False         False
## 37      False    False        False          False          True         False
## 38      False    False        False          False         False         False
## 39      False    False        False          False         False         False
## 40      False    False        False          False         False         False
##    Enhanced.Sight Molecular.Manipulation Heat.Generation Adaptation Gliding
## 1           False                  False           False       True    True
## 2           False                  False           False      False    True
## 3           False                  False           False      False   False
## 4            True                  False           False      False   False
## 5           False                  False           False      False    True
## 6           False                  False           False      False   False
## 7           False                  False           False      False   False
## 8           False                  False           False      False   False
## 9           False                  False           False      False   False
## 10          False                   True           False      False   False
## 11          False                  False           False      False   False
## 12           True                  False           False      False   False
## 13          False                  False           False      False   False
## 14          False                  False           False      False   False
## 15          False                  False           False      False   False
## 16          False                  False           False      False    True
## 17          False                  False           False      False   False
## 18          False                  False           False      False   False
## 19          False                   True           False       True   False
## 20          False                  False           False      False   False
## 21          False                   True           False      False   False
## 22          False                  False            True      False   False
## 23          False                  False           False      False   False
## 24          False                  False           False      False   False
## 25           True                  False           False      False   False
## 26          False                  False           False      False   False
## 27          False                  False           False      False   False
## 28          False                  False            True      False   False
## 29          False                  False           False      False   False
## 30          False                  False           False      False   False
## 31          False                  False           False      False   False
## 32          False                  False           False      False   False
## 33          False                  False           False      False   False
## 34          False                  False           False      False   False
## 35          False                  False           False      False   False
## 36          False                  False           False      False   False
## 37          False                  False           False      False   False
## 38           True                  False           False      False   False
## 39           True                  False           False      False   False
## 40           True                  False           False      False   False
##    Power.Suit Mind.Blast Probability.Manipulation Gravity.Control Regeneration
## 1       False      False                    False           False        False
## 2       False      False                    False           False        False
## 3       False      False                    False           False        False
## 4       False      False                    False           False        False
## 5        True      False                    False           False        False
## 6       False      False                    False           False        False
## 7        True      False                    False           False        False
## 8       False      False                    False           False        False
## 9       False      False                    False           False        False
## 10      False       True                    False           False        False
## 11      False      False                    False           False        False
## 12      False      False                    False           False        False
## 13      False      False                    False           False        False
## 14      False      False                    False           False        False
## 15      False      False                    False           False        False
## 16      False      False                    False           False        False
## 17      False      False                    False           False        False
## 18      False      False                    False           False        False
## 19      False       True                    False           False        False
## 20      False      False                    False           False         True
## 21      False      False                    False           False        False
## 22      False      False                    False           False        False
## 23      False      False                    False           False        False
## 24      False      False                    False           False        False
## 25      False      False                    False           False        False
## 26      False      False                    False           False        False
## 27       True      False                    False           False        False
## 28      False      False                    False            True        False
## 29      False      False                    False           False        False
## 30      False      False                    False           False        False
## 31      False      False                    False           False        False
## 32      False      False                    False           False        False
## 33      False      False                    False           False         True
## 34      False      False                    False           False         True
## 35      False      False                    False           False        False
## 36      False      False                    False            True        False
## 37      False      False                    False           False         True
## 38      False      False                    False           False        False
## 39      False      False                    False           False         True
## 40      False      False                    False           False        False
##    Light.Control Echolocation Levitation Toxin.and.Disease.Control Banish
## 1          False        False      False                     False  False
## 2          False        False      False                     False  False
## 3          False        False      False                     False  False
## 4          False        False      False                     False  False
## 5          False        False      False                     False  False
## 6          False        False      False                     False  False
## 7          False        False      False                     False  False
## 8          False        False      False                      True  False
## 9          False        False      False                     False  False
## 10         False        False      False                     False  False
## 11         False        False      False                     False  False
## 12         False        False      False                     False  False
## 13         False        False      False                     False  False
## 14         False        False      False                     False  False
## 15         False        False      False                     False  False
## 16         False        False      False                     False  False
## 17         False        False      False                     False  False
## 18         False        False      False                     False  False
## 19         False        False       True                     False  False
## 20         False        False      False                     False  False
## 21         False        False      False                     False  False
## 22         False        False      False                     False  False
## 23         False        False      False                     False  False
## 24         False        False      False                     False  False
## 25         False        False      False                     False  False
## 26         False        False      False                     False  False
## 27         False        False      False                     False  False
## 28          True        False       True                     False  False
## 29         False        False      False                     False  False
## 30         False        False      False                     False  False
## 31         False        False      False                     False  False
## 32         False        False      False                     False  False
## 33         False        False      False                     False  False
## 34         False        False      False                     False  False
## 35         False        False      False                     False  False
## 36         False        False      False                     False  False
## 37         False        False      False                     False  False
## 38         False        False      False                     False  False
## 39         False        False      False                     False  False
## 40         False        False      False                     False  False
##    Energy.Manipulation Heat.Resistance Natural.Weapons Time.Travel
## 1                False           False            True       False
## 2                False           False           False       False
## 3                False           False           False       False
## 4                False            True           False       False
## 5                False           False           False       False
## 6                False           False            True       False
## 7                False           False           False       False
## 8                False           False           False       False
## 9                False           False           False       False
## 10               False           False           False        True
## 11               False           False           False       False
## 12               False           False           False       False
## 13               False           False           False       False
## 14               False           False           False       False
## 15               False           False           False       False
## 16               False           False            True       False
## 17               False           False            True       False
## 18               False           False            True       False
## 19               False           False           False        True
## 20               False           False           False       False
## 21               False           False           False       False
## 22                True           False           False       False
## 23                True           False           False        True
## 24                True           False           False       False
## 25               False           False            True       False
## 26               False           False            True       False
## 27               False           False           False       False
## 28               False           False           False        True
## 29               False           False            True       False
## 30               False           False           False       False
## 31               False           False            True       False
## 32               False           False           False       False
## 33               False            True            True       False
## 34               False            True            True       False
## 35               False           False            True       False
## 36               False           False           False       False
## 37               False           False            True       False
## 38               False           False            True       False
## 39               False           False            True       False
## 40               False           False            True       False
##    Enhanced.Smell Illusions Thirstokinesis Hair.Manipulation Illumination
## 1           False     False          False             False        False
## 2           False     False          False             False        False
## 3           False     False          False             False        False
## 4            True     False          False             False        False
## 5           False     False          False             False        False
## 6           False     False          False             False        False
## 7           False     False          False             False        False
## 8           False     False          False             False        False
## 9           False     False          False             False        False
## 10          False      True          False             False        False
## 11          False     False          False             False        False
## 12           True     False          False             False        False
## 13          False      True          False             False        False
## 14          False     False          False             False        False
## 15          False     False          False             False        False
## 16          False     False          False             False        False
## 17           True     False          False             False        False
## 18          False     False          False             False        False
## 19          False     False          False             False        False
## 20          False      True          False             False        False
## 21          False     False          False             False        False
## 22           True     False          False             False        False
## 23          False      True          False             False        False
## 24           True     False          False             False        False
## 25           True     False          False             False        False
## 26          False     False          False             False        False
## 27          False     False          False             False        False
## 28          False      True          False             False        False
## 29          False     False          False             False        False
## 30          False     False          False             False        False
## 31          False     False          False             False        False
## 32          False     False          False             False        False
## 33           True     False          False             False        False
## 34           True     False          False             False        False
## 35          False     False          False             False        False
## 36          False     False          False             False        False
## 37          False     False          False             False        False
## 38           True     False          False             False        False
## 39           True     False          False             False        False
## 40           True     False          False             False        False
##    Omnipotent Cloaking Changing.Armor Power.Cosmic Biokinesis Water.Control
## 1       False    False          False        False      False         False
## 2       False    False          False        False      False         False
## 3       False    False          False        False      False         False
## 4       False    False          False        False      False          True
## 5       False    False          False        False      False         False
## 6       False    False          False        False      False         False
## 7       False    False          False        False      False         False
## 8       False    False          False        False      False         False
## 9       False    False          False        False      False         False
## 10      False     True          False        False      False         False
## 11      False    False          False        False      False         False
## 12      False    False          False        False      False         False
## 13      False    False          False        False      False         False
## 14      False    False          False        False      False         False
## 15      False    False          False        False      False         False
## 16      False    False          False        False      False         False
## 17      False    False          False        False      False         False
## 18      False    False          False        False      False         False
## 19      False    False          False        False      False         False
## 20      False    False          False        False      False         False
## 21      False    False          False        False      False         False
## 22      False    False          False        False      False         False
## 23      False    False          False        False      False         False
## 24      False    False          False        False      False         False
## 25      False    False          False        False      False         False
## 26      False    False          False        False      False         False
## 27      False    False          False        False      False         False
## 28      False    False          False        False      False          True
## 29      False    False          False        False      False         False
## 30      False    False          False        False      False         False
## 31      False    False          False        False      False         False
## 32      False    False          False        False      False         False
## 33      False    False          False        False      False         False
## 34      False    False          False        False      False         False
## 35      False    False          False        False      False         False
## 36      False    False          False        False      False         False
## 37      False    False          False        False      False         False
## 38      False    False          False        False      False         False
## 39      False    False          False        False      False         False
## 40      False    False          False        False      False         False
##    Radiation.Immunity Vision...Telescopic Toxin.and.Disease.Resistance
## 1               False               False                        False
## 2               False                True                         True
## 3               False               False                        False
## 4               False               False                        False
## 5               False               False                        False
## 6               False               False                        False
## 7               False                True                        False
## 8               False               False                        False
## 9               False                True                        False
## 10              False               False                        False
## 11              False               False                        False
## 12              False               False                        False
## 13              False               False                        False
## 14              False               False                        False
## 15              False               False                         True
## 16              False               False                        False
## 17              False               False                        False
## 18              False               False                        False
## 19              False               False                        False
## 20              False                True                         True
## 21              False               False                         True
## 22              False               False                        False
## 23              False               False                        False
## 24              False               False                        False
## 25              False               False                         True
## 26              False               False                        False
## 27              False               False                        False
## 28              False               False                        False
## 29              False               False                        False
## 30              False               False                        False
## 31              False               False                         True
## 32              False               False                         True
## 33              False                True                        False
## 34              False                True                        False
## 35              False               False                        False
## 36              False                True                        False
## 37              False               False                         True
## 38              False               False                        False
## 39              False                True                         True
## 40              False                True                         True
##    Spatial.Awareness Energy.Resistance Telepathy.Resistance
## 1              False             False                False
## 2              False             False                False
## 3              False             False                False
## 4              False             False                False
## 5              False             False                False
## 6              False             False                False
## 7              False             False                False
## 8              False             False                False
## 9              False             False                False
## 10             False             False                 True
## 11             False             False                False
## 12             False             False                False
## 13             False             False                False
## 14             False             False                 True
## 15             False             False                False
## 16             False             False                False
## 17             False             False                False
## 18             False             False                False
## 19             False             False                False
## 20             False             False                 True
## 21             False             False                 True
## 22             False             False                False
## 23             False             False                False
## 24             False             False                False
## 25             False             False                False
## 26             False             False                False
## 27             False             False                False
## 28             False             False                False
## 29             False             False                False
## 30             False             False                False
## 31             False             False                False
## 32             False             False                False
## 33             False             False                False
## 34             False             False                False
## 35             False             False                False
## 36             False             False                False
## 37             False             False                 True
## 38             False             False                False
## 39             False             False                False
## 40             False             False                False
##    Molecular.Combustion Omnilingualism Portal.Creation Magnetism
## 1                 False          False           False     False
## 2                 False          False           False     False
## 3                 False          False           False     False
## 4                 False          False           False     False
## 5                 False          False           False     False
## 6                 False          False           False     False
## 7                 False          False           False     False
## 8                 False          False           False     False
## 9                 False          False           False     False
## 10                False          False           False     False
## 11                False          False           False     False
## 12                False          False           False     False
## 13                False          False           False     False
## 14                False          False           False     False
## 15                False          False           False     False
## 16                False          False           False     False
## 17                False          False           False     False
## 18                False          False           False     False
## 19                False          False           False     False
## 20                False          False           False     False
## 21                False          False           False     False
## 22                False          False           False     False
## 23                False           True           False     False
## 24                False          False           False     False
## 25                False          False           False     False
## 26                False          False           False     False
## 27                False          False           False     False
## 28                False           True           False     False
## 29                False          False           False     False
## 30                False          False           False     False
## 31                False          False           False     False
## 32                False          False           False     False
## 33                False          False           False     False
## 34                False          False           False     False
## 35                False          False           False     False
## 36                False          False           False     False
## 37                False          False           False     False
## 38                False          False           False     False
## 39                False          False           False     False
## 40                False          False           False     False
##    Mind.Control.Resistance Plant.Control Sonar Sonic.Scream Time.Manipulation
## 1                    False         False False        False             False
## 2                    False         False False        False             False
## 3                    False         False False        False             False
## 4                    False         False  True        False             False
## 5                    False         False False        False             False
## 6                    False         False False        False              True
## 7                    False         False False        False             False
## 8                    False         False False        False             False
## 9                    False         False False        False             False
## 10                    True         False False        False             False
## 11                   False         False False        False             False
## 12                   False         False False        False             False
## 13                   False         False False        False             False
## 14                   False         False False        False             False
## 15                   False         False False        False             False
## 16                   False         False False        False             False
## 17                   False         False False        False             False
## 18                   False         False False        False             False
## 19                   False         False False        False              True
## 20                   False         False False        False             False
## 21                   False         False False        False             False
## 22                   False         False False        False             False
## 23                   False         False False        False              True
## 24                   False         False False        False             False
## 25                   False         False False        False             False
## 26                   False         False False        False             False
## 27                   False         False False        False             False
## 28                   False         False False        False              True
## 29                   False         False False        False             False
## 30                   False         False False        False             False
## 31                   False         False False        False             False
## 32                   False         False False        False             False
## 33                   False         False False        False             False
## 34                   False         False False        False             False
## 35                   False         False False        False             False
## 36                   False         False False        False             False
## 37                    True         False False        False             False
## 38                   False         False False        False             False
## 39                   False         False False        False             False
## 40                   False         False False        False             False
##    Enhanced.Touch Magic.Resistance Invisibility Sub.Mariner
## 1           False            False        False       False
## 2           False            False        False       False
## 3           False            False        False       False
## 4           False            False        False        True
## 5           False            False        False       False
## 6           False            False        False       False
## 7           False            False        False        True
## 8           False            False        False       False
## 9           False            False        False       False
## 10          False            False        False       False
## 11          False            False        False       False
## 12          False            False        False       False
## 13          False            False        False       False
## 14          False            False        False       False
## 15          False            False        False       False
## 16          False            False        False       False
## 17          False            False        False       False
## 18          False            False        False       False
## 19          False            False        False       False
## 20          False            False         True       False
## 21          False            False        False       False
## 22          False            False        False       False
## 23          False            False         True       False
## 24          False            False        False       False
## 25          False            False        False       False
## 26          False            False        False       False
## 27          False            False        False       False
## 28          False            False         True        True
## 29          False            False        False       False
## 30          False            False        False       False
## 31          False            False        False       False
## 32          False            False        False       False
## 33           True            False        False       False
## 34           True            False        False       False
## 35          False            False        False       False
## 36          False            False        False       False
## 37          False            False        False       False
## 38          False            False        False       False
## 39          False            False        False       False
## 40          False            False        False       False
##    Radiation.Absorption Intuitive.aptitude Vision...Microscopic Melting
## 1                 False              False                False   False
## 2                 False              False                False   False
## 3                 False              False                False   False
## 4                 False              False                False   False
## 5                 False              False                False   False
## 6                 False              False                False   False
## 7                 False              False                False   False
## 8                 False              False                False   False
## 9                 False              False                False   False
## 10                False              False                False   False
## 11                False              False                False   False
## 12                False              False                False   False
## 13                False              False                False   False
## 14                False              False                False   False
## 15                False              False                False   False
## 16                False              False                False   False
## 17                False              False                False   False
## 18                False              False                False   False
## 19                False              False                False   False
## 20                False              False                 True   False
## 21                False              False                False   False
## 22                False              False                False   False
## 23                False              False                False   False
## 24                False              False                False   False
## 25                False              False                False   False
## 26                False              False                False   False
## 27                False              False                False   False
## 28                False              False                False   False
## 29                False              False                False   False
## 30                False              False                False   False
## 31                False              False                False   False
## 32                False              False                False   False
## 33                False              False                False    True
## 34                False              False                 True   False
## 35                False              False                False   False
## 36                 True              False                False   False
## 37                False              False                False   False
## 38                False              False                False   False
## 39                False              False                False   False
## 40                False              False                False   False
##    Wind.Control Super.Breath Wallcrawling Vision...Night Vision...Infrared
## 1         False        False         True          False             False
## 2         False        False        False          False             False
## 3         False        False        False          False             False
## 4         False        False        False           True             False
## 5         False        False        False           True              True
## 6         False        False        False          False             False
## 7         False        False        False          False              True
## 8         False        False         True          False             False
## 9         False        False        False          False             False
## 10        False        False        False          False             False
## 11        False        False        False          False             False
## 12        False        False        False           True             False
## 13        False        False        False          False             False
## 14        False        False        False          False             False
## 15        False        False        False          False             False
## 16        False        False         True          False             False
## 17        False        False        False          False             False
## 18        False        False        False          False             False
## 19        False        False        False          False             False
## 20        False         True        False           True              True
## 21        False        False        False          False             False
## 22        False        False         True          False             False
## 23        False        False        False          False             False
## 24        False        False        False          False             False
## 25        False        False        False          False             False
## 26        False        False         True          False             False
## 27        False        False        False          False             False
## 28        False        False        False          False             False
## 29        False        False         True          False             False
## 30        False        False         True          False             False
## 31        False        False         True           True             False
## 32        False        False        False          False             False
## 33        False        False        False           True              True
## 34        False        False        False           True              True
## 35        False        False         True          False             False
## 36        False        False        False           True             False
## 37        False        False         True          False             False
## 38        False        False         True          False             False
## 39        False        False        False           True             False
## 40        False        False        False           True             False
##    Grim.Reaping Matter.Absorption The.Force Resurrection Terrakinesis
## 1         False              True     False        False        False
## 2         False             False     False        False        False
## 3         False             False     False        False        False
## 4         False             False     False        False        False
## 5         False             False     False        False        False
## 6         False             False     False        False        False
## 7         False             False     False        False        False
## 8         False             False     False        False        False
## 9         False             False     False        False        False
## 10        False             False     False        False        False
## 11        False             False     False        False        False
## 12        False             False     False        False        False
## 13        False             False      True        False        False
## 14        False             False     False        False        False
## 15        False             False     False        False        False
## 16        False              True     False        False        False
## 17        False             False     False        False        False
## 18        False             False     False        False        False
## 19        False             False     False        False        False
## 20        False             False     False        False        False
## 21        False             False     False        False        False
## 22        False             False     False        False        False
## 23        False              True     False        False        False
## 24        False             False     False        False        False
## 25        False             False     False        False        False
## 26        False             False     False        False        False
## 27        False             False     False         True        False
## 28        False             False     False        False        False
## 29        False             False     False        False        False
## 30        False             False     False        False        False
## 31        False             False     False        False        False
## 32        False             False     False        False        False
## 33        False             False     False        False        False
## 34        False             False     False        False        False
## 35        False             False     False        False        False
## 36        False             False     False        False        False
## 37        False             False     False        False        False
## 38        False             False     False        False        False
## 39        False             False     False        False        False
## 40        False             False     False        False        False
##    Vision...Heat Vitakinesis Radar.Sense Qwardian.Power.Ring Weather.Control
## 1          False       False       False               False           False
## 2          False       False       False               False           False
## 3          False       False       False               False           False
## 4          False       False       False               False           False
## 5          False       False       False               False           False
## 6          False       False       False               False           False
## 7          False       False       False               False           False
## 8          False       False       False               False           False
## 9          False       False       False               False           False
## 10         False       False       False               False           False
## 11         False       False       False               False           False
## 12         False       False       False               False           False
## 13         False       False       False               False           False
## 14         False       False       False               False           False
## 15         False       False       False               False           False
## 16         False       False       False               False           False
## 17         False       False       False               False           False
## 18         False       False       False               False           False
## 19         False       False       False               False           False
## 20          True       False       False               False           False
## 21         False       False       False               False           False
## 22         False       False       False               False            True
## 23         False       False       False               False            True
## 24         False       False       False               False           False
## 25         False       False       False               False           False
## 26         False       False       False               False           False
## 27         False       False       False               False           False
## 28          True       False       False               False           False
## 29         False       False       False               False           False
## 30         False       False       False               False           False
## 31         False       False       False               False           False
## 32         False       False       False               False           False
## 33         False       False       False               False           False
## 34         False       False       False               False           False
## 35         False       False       False               False           False
## 36         False       False       False               False           False
## 37         False       False       False               False           False
## 38         False       False       False               False           False
## 39         False       False       False               False           False
## 40         False       False       False               False           False
##    Vision...X.Ray Vision...Thermal Web.Creation Reality.Warping Odin.Force
## 1           False             True        False           False      False
## 2           False            False        False           False      False
## 3           False            False        False           False      False
## 4           False            False        False           False      False
## 5           False            False        False           False      False
## 6           False            False        False           False      False
## 7           False            False        False           False      False
## 8           False            False        False           False      False
## 9           False            False        False           False      False
## 10          False             True        False           False      False
## 11          False            False        False           False      False
## 12          False            False        False           False      False
## 13          False            False        False           False      False
## 14          False            False        False           False      False
## 15          False            False        False           False      False
## 16          False            False         True           False      False
## 17          False            False        False           False      False
## 18          False            False        False           False      False
## 19          False            False        False            True      False
## 20           True             True        False           False      False
## 21          False            False        False           False      False
## 22          False            False        False           False      False
## 23          False            False        False           False       True
## 24          False            False        False           False      False
## 25          False            False        False           False      False
## 26          False            False         True           False      False
## 27          False            False        False           False      False
## 28          False            False        False            True      False
## 29          False            False         True           False      False
## 30          False            False         True           False      False
## 31          False            False         True           False      False
## 32          False            False        False           False      False
## 33          False             True        False           False      False
## 34          False             True        False           False      False
## 35          False            False         True           False      False
## 36           True             True        False           False      False
## 37          False            False         True           False      False
## 38          False            False        False           False      False
## 39          False            False        False           False      False
## 40          False            False        False           False      False
##    Symbiote.Costume Speed.Force Phoenix.Force Molecular.Dissipation
## 1             False       False         False                 False
## 2             False       False         False                 False
## 3             False       False         False                 False
## 4             False       False         False                 False
## 5             False       False         False                 False
## 6             False       False         False                 False
## 7             False       False         False                 False
## 8             False       False         False                 False
## 9             False       False         False                 False
## 10            False       False         False                 False
## 11            False       False         False                 False
## 12            False       False         False                 False
## 13            False       False         False                 False
## 14            False       False         False                 False
## 15            False       False         False                 False
## 16             True       False         False                 False
## 17            False       False         False                 False
## 18            False       False         False                 False
## 19            False       False         False                 False
## 20            False       False         False                 False
## 21            False       False         False                 False
## 22            False       False         False                 False
## 23            False       False         False                 False
## 24            False       False         False                 False
## 25            False       False         False                 False
## 26            False       False         False                 False
## 27             True       False         False                 False
## 28            False       False         False                 False
## 29            False       False         False                 False
## 30            False       False         False                 False
## 31            False       False         False                 False
## 32            False       False         False                 False
## 33            False       False         False                 False
## 34            False       False         False                 False
## 35             True       False         False                 False
## 36            False       False         False                 False
## 37             True       False         False                 False
## 38            False       False         False                 False
## 39            False       False         False                 False
## 40            False       False         False                 False
##    Vision...Cryo Omnipresent Omniscient
## 1          False       False      False
## 2          False       False      False
## 3          False       False      False
## 4          False       False      False
## 5          False       False      False
## 6          False       False      False
## 7          False       False      False
## 8          False       False      False
## 9          False       False      False
## 10         False       False      False
## 11         False       False      False
## 12         False       False      False
## 13         False       False      False
## 14         False       False      False
## 15         False       False      False
## 16         False       False      False
## 17         False       False      False
## 18         False       False      False
## 19         False       False      False
## 20         False       False      False
## 21         False       False      False
## 22         False       False      False
## 23         False       False      False
## 24         False       False      False
## 25         False       False      False
## 26         False       False      False
## 27         False       False      False
## 28         False       False      False
## 29         False       False      False
## 30         False       False      False
## 31         False       False      False
## 32         False       False      False
## 33         False       False      False
## 34         False       False      False
## 35         False       False      False
## 36         False       False      False
## 37         False       False      False
## 38         False       False      False
## 39         False       False      False
## 40         False       False      False
```


## Your Favorite
14. Pick your favorite superhero and let's see their powers!  


```r
superhero_powers%>%
  filter(hero_names == "Iron Man")%>%
  select_if(all)
```

```
## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical

## Warning in .p(column, ...): coercing argument of type 'character' to logical
```

```
##   Accelerated.Healing Durability Energy.Absorption Flight Underwater.breathing
## 1                True       True              True   True                 True
##   Marksmanship Super.Strength Energy.Blasts Stamina Super.Speed
## 1         True           True          True    True        True
##   Weapon.based.Powers Energy.Beams Reflexes Force.Fields Power.Suit
## 1                True         True     True         True       True
##   Radiation.Immunity Vision...Telescopic Magnetism Invisibility Vision...Night
## 1               True                True      True         True           True
##   Vision...Thermal
## 1             True
```


15. Can you find your hero in the superhero_info data? Show their info!  


```r
superhero_info%>%
  filter(name == "Iron Man")
```

```
##       name Gender Eye.color  Race Hair.color Height     Publisher Skin.color
## 1 Iron Man   Male      blue Human      Black    198 Marvel Comics       <NA>
##   Alignment Weight
## 1      good    191
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
