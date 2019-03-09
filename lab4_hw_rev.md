---
title: "Lab 4 Homework"
author: "Valerie Chau"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code, keep track of your versions using git, and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Life History
Aren't mammals fun? Let's load up some more mammal data. This will be life history data for mammals. The [data](http://esapubs.org/archive/ecol/E084/093/) are from: *S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.*  


```r
life_history <- readr::read_csv("/Users/clmuser/Desktop/val98-master/mammal_lifehistories_v2.csv")
```

```
## Parsed with column specification:
## cols(
##   order = col_character(),
##   family = col_character(),
##   Genus = col_character(),
##   species = col_character(),
##   mass = col_double(),
##   gestation = col_double(),
##   newborn = col_double(),
##   weaning = col_double(),
##   `wean mass` = col_double(),
##   AFR = col_double(),
##   `max. life` = col_double(),
##   `litter size` = col_double(),
##   `litters/year` = col_double()
## )
```

Rename some of the variables. Notice that I am replacing the old `life_history` data.

```r
life_history <- 
  life_history %>% 
  dplyr::rename(
          genus        = Genus,
          wean_mass    = `wean mass`,
          max_life     = `max. life`,
          litter_size  = `litter size`,
          litters_yr   = `litters/year`
          )
```

1. Explore the data using the method that you prefer. Below, I am using a new package called `skimr`. If you want to use this, make sure that it is installed.

```r
#install.packages("skimr")
```


```r
library("skimr")
```

```
## Warning: package 'skimr' was built under R version 3.5.2
```

```
## 
## Attaching package: 'skimr'
```

```
## The following object is masked from 'package:stats':
## 
##     filter
```


```r
life_history %>% 
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## ── Variable type:character ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## ── Variable type:numeric ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
##     variable missing complete    n      mean         sd   p0  p25     p50
##          AFR       0     1440 1440   -408.12     504.97 -999 -999    2.5 
##    gestation       0     1440 1440   -287.25     455.36 -999 -999    1.05
##  litter_size       0     1440 1440    -55.63     234.88 -999    1    2.27
##   litters_yr       0     1440 1440   -477.14     500.03 -999 -999    0.38
##         mass       0     1440 1440 383576.72 5055162.92 -999   50  403.02
##     max_life       0     1440 1440   -490.26     615.3  -999 -999 -999   
##      newborn       0     1440 1440   6703.15   90912.52 -999 -999    2.65
##    wean_mass       0     1440 1440  16048.93   5e+05    -999 -999 -999   
##      weaning       0     1440 1440   -427.17     496.71 -999 -999    0.73
##      p75          p100     hist
##    15.61     210       ▆▁▁▁▁▁▇▁
##     4.5       21.46    ▃▁▁▁▁▁▁▇
##     3.83      14.18    ▁▁▁▁▁▁▁▇
##     1.15       7.5     ▇▁▁▁▁▁▁▇
##  7009.17       1.5e+08 ▇▁▁▁▁▁▁▁
##   147.25    1368       ▇▁▁▃▂▁▁▁
##    98    2250000       ▇▁▁▁▁▁▁▁
##    10          1.9e+07 ▇▁▁▁▁▁▁▁
##     2         48       ▆▁▁▁▁▁▁▇
```

2. Run the code below. Are there any NA's in the data? Does this seem likely?

```r
msleep %>% 
  summarize(number_nas= sum(is.na(life_history)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```

3. Are there any missing data (NA's) represented by different values? How much and where? In which variables do we have the most missing data? Can you think of a reason why so much data are missing in this variable?
## represented as -990

```r
lifeHistories_na <- 
  life_history %>%
  na_if("-999")%>%
  purrr::map_df(~ sum(is.na(.)))
lifeHistories_na
```

```
## # A tibble: 1 x 13
##   order family genus species  mass gestation newborn weaning wean_mass
##   <int>  <int> <int>   <int> <int>     <int>   <int>   <int>     <int>
## 1     0      0     0       0    85       418     595     619      1039
## # … with 4 more variables: AFR <int>, max_life <int>, litter_size <int>,
## #   litters_yr <int>
```

4. Compared to the `msleep` data, we have better representation among taxa. Produce a summary that shows the number of observations by taxonomic order.

```r
life_history %>%
   count(order, sort=TRUE)
```

```
## # A tibble: 17 x 2
##    order              n
##    <chr>          <int>
##  1 Rodentia         665
##  2 Carnivora        197
##  3 Artiodactyla     161
##  4 Primates         156
##  5 Insectivora       91
##  6 Cetacea           55
##  7 Lagomorpha        42
##  8 Xenarthra         20
##  9 Perissodactyla    15
## 10 Macroscelidea     10
## 11 Pholidota          7
## 12 Scandentia         7
## 13 Sirenia            5
## 14 Hyracoidea         4
## 15 Dermoptera         2
## 16 Proboscidea        2
## 17 Tubulidentata      1
```


5. Mammals have a range of life histories, including lifespan. Produce a summary of lifespan in years by order. Be sure to include the minimum, maximum, mean, standard deviation, and total n.

```r
life_history %>%
  group_by(order) %>% #we are grouping by feeding ecology
  summarize(min_lifespan=min(max_life),
            max_lifespan=max(max_life),
            mean_lifespan=mean(max_life),
            std_lifespan=sd(max_lifespan),
            total=n())
```

```
## # A tibble: 17 x 6
##    order         min_lifespan max_lifespan mean_lifespan std_lifespan total
##    <chr>                <dbl>        <dbl>         <dbl>        <dbl> <int>
##  1 Artiodactyla          -999          732        -232.            NA   161
##  2 Carnivora             -999          672        -122.            NA   197
##  3 Cetacea               -999         1368         125.            NA    55
##  4 Dermoptera            -999          210        -394.            NA     2
##  5 Hyracoidea            -999          147        -430.            NA     4
##  6 Insectivora           -999          156        -540.            NA    91
##  7 Lagomorpha            -999          216        -788.            NA    42
##  8 Macroscelidea         -999          105        -572.            NA    10
##  9 Perissodacty…         -999          600         141.            NA    15
## 10 Pholidota             -999          240        -822             NA     7
## 11 Primates              -999          726        -379.            NA   156
## 12 Proboscidea            840          960         900             NA     2
## 13 Rodentia              -999          420        -737.            NA   665
## 14 Scandentia            -999          149        -525.            NA     7
## 15 Sirenia               -999          876         -88.8           NA     5
## 16 Tubulidentata          288          288         288             NA     1
## 17 Xenarthra             -999          480        -309.            NA    20
```



6. Let's look closely at gestation and newborns. Summarize the mean gestation, newborn mass, and weaning mass by order. Add a new column that shows mean gestation in years and sort in descending order. Which group has the longest mean gestation? What is the common name for these mammals?

```r
life_history %>%
  group_by(order) %>% #we are grouping by feeding ecology
  summarize(mean_gestation=mean(gestation),
            mean_newBornMass=mean(newborn),
            mean_weaningMass=mean(wean_mass),
            total=n()) %>% 
  arrange(desc(mean_gestation))
```

```
## # A tibble: 17 x 5
##    order          mean_gestation mean_newBornMass mean_weaningMass total
##    <chr>                   <dbl>            <dbl>            <dbl> <int>
##  1 Proboscidea             21.3           99523.          299500.      2
##  2 Perissodactyla          13.0           21412.           75639.     15
##  3 Hyracoidea               7.4             -76.5           -624.      4
##  4 Tubulidentata            7.08           1734             6250       1
##  5 Dermoptera               2.75           -482.            -999       2
##  6 Artiodactyla           -61.5            5125.            7725.    161
##  7 Carnivora             -124.             2262.            7607.    197
##  8 Cetacea               -135.           149143.          347883.     55
##  9 Xenarthra             -196.             -342.            -928.     20
## 10 Lagomorpha            -237.             -345.            -428.     42
## 11 Primates              -259.              -83.8            -40.9   156
## 12 Macroscelidea         -298.             -385.            -778.     10
## 13 Rodentia              -391.             -484.            -673.    665
## 14 Sirenia               -393.            13327.           12701.      5
## 15 Scandentia            -427.             -276.            -684.      7
## 16 Insectivora           -504.             -502.            -727.     91
## 17 Pholidota             -569.              -88.0            718.      7
```

#proboscidea (elephants) have the longest mean gestation period 

## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
