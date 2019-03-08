---
title: "Lab 2 Homework"
author: "Valerie Chau"
date: "Winter2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the tidyverse

```r
#install.packages("tidyverse")
library("tidyverse")
```

```
## ── Attaching packages ────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0       ✔ purrr   0.3.0  
## ✔ tibble  2.0.1       ✔ dplyr   0.8.0.1
## ✔ tidyr   0.8.2       ✔ stringr 1.4.0  
## ✔ readr   1.3.1       ✔ forcats 0.4.0
```

```
## Warning: package 'tibble' was built under R version 3.5.2
```

```
## Warning: package 'purrr' was built under R version 3.5.2
```

```
## Warning: package 'dplyr' was built under R version 3.5.2
```

```
## Warning: package 'stringr' was built under R version 3.5.2
```

```
## Warning: package 'forcats' was built under R version 3.5.2
```

```
## ── Conflicts ───────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

## Mammals Sleep
For this assignment, we are going to use built-in data on mammal sleep patterns.  

```r
msleep
```

```
## # A tibble: 83 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
##  1 Chee… Acin… carni Carn… lc                  12.1      NA        NA    
##  2 Owl … Aotus omni  Prim… <NA>                17         1.8      NA    
##  3 Moun… Aplo… herbi Rode… nt                  14.4       2.4      NA    
##  4 Grea… Blar… omni  Sori… lc                  14.9       2.3       0.133
##  5 Cow   Bos   herbi Arti… domesticated         4         0.7       0.667
##  6 Thre… Brad… herbi Pilo… <NA>                14.4       2.2       0.767
##  7 Nort… Call… carni Carn… vu                   8.7       1.4       0.383
##  8 Vesp… Calo… <NA>  Rode… <NA>                 7        NA        NA    
##  9 Dog   Canis carni Carn… domesticated        10.1       2.9       0.333
## 10 Roe … Capr… herbi Arti… lc                   3        NA        NA    
## # … with 73 more rows, and 3 more variables: awake <dbl>, brainwt <dbl>,
## #   bodywt <dbl>
```

1. From which publication are these data taken from? Don't do an internet search; show the code that you would use to find out in R.

```r
?msleep
```




2. Provide some summary information about the data to get you started; feel free to use the functions that you find most helpful.

```r
head(msleep)
```

```
## # A tibble: 6 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
## 1 Chee… Acin… carni Carn… lc                  12.1      NA        NA    
## 2 Owl … Aotus omni  Prim… <NA>                17         1.8      NA    
## 3 Moun… Aplo… herbi Rode… nt                  14.4       2.4      NA    
## 4 Grea… Blar… omni  Sori… lc                  14.9       2.3       0.133
## 5 Cow   Bos   herbi Arti… domesticated         4         0.7       0.667
## 6 Thre… Brad… herbi Pilo… <NA>                14.4       2.2       0.767
## # … with 3 more variables: awake <dbl>, brainwt <dbl>, bodywt <dbl>
```


```r
colnames(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"       
##  [5] "conservation" "sleep_total"  "sleep_rem"    "sleep_cycle" 
##  [9] "awake"        "brainwt"      "bodywt"
```

```r
rownames(msleep)
```

```
##  [1] "1"  "2"  "3"  "4"  "5"  "6"  "7"  "8"  "9"  "10" "11" "12" "13" "14"
## [15] "15" "16" "17" "18" "19" "20" "21" "22" "23" "24" "25" "26" "27" "28"
## [29] "29" "30" "31" "32" "33" "34" "35" "36" "37" "38" "39" "40" "41" "42"
## [43] "43" "44" "45" "46" "47" "48" "49" "50" "51" "52" "53" "54" "55" "56"
## [57] "57" "58" "59" "60" "61" "62" "63" "64" "65" "66" "67" "68" "69" "70"
## [71] "71" "72" "73" "74" "75" "76" "77" "78" "79" "80" "81" "82" "83"
```

```r
tail(msleep)
```

```
## # A tibble: 6 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
## 1 Tenr… Tenr… omni  Afro… <NA>                15.6       2.3      NA    
## 2 Tree… Tupa… omni  Scan… <NA>                 8.9       2.6       0.233
## 3 Bott… Turs… carni Ceta… <NA>                 5.2      NA        NA    
## 4 Genet Gene… carni Carn… <NA>                 6.3       1.3      NA    
## 5 Arct… Vulp… carni Carn… <NA>                12.5      NA        NA    
## 6 Red … Vulp… carni Carn… <NA>                 9.8       2.4       0.35 
## # … with 3 more variables: awake <dbl>, brainwt <dbl>, bodywt <dbl>
```

```r
summary(msleep)
```

```
##      name              genus               vore          
##  Length:83          Length:83          Length:83         
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##     order           conservation        sleep_total      sleep_rem    
##  Length:83          Length:83          Min.   : 1.90   Min.   :0.100  
##  Class :character   Class :character   1st Qu.: 7.85   1st Qu.:0.900  
##  Mode  :character   Mode  :character   Median :10.10   Median :1.500  
##                                        Mean   :10.43   Mean   :1.875  
##                                        3rd Qu.:13.75   3rd Qu.:2.400  
##                                        Max.   :19.90   Max.   :6.600  
##                                                        NA's   :22     
##   sleep_cycle         awake          brainwt            bodywt        
##  Min.   :0.1167   Min.   : 4.10   Min.   :0.00014   Min.   :   0.005  
##  1st Qu.:0.1833   1st Qu.:10.25   1st Qu.:0.00290   1st Qu.:   0.174  
##  Median :0.3333   Median :13.90   Median :0.01240   Median :   1.670  
##  Mean   :0.4396   Mean   :13.57   Mean   :0.28158   Mean   : 166.136  
##  3rd Qu.:0.5792   3rd Qu.:16.15   3rd Qu.:0.12550   3rd Qu.:  41.750  
##  Max.   :1.5000   Max.   :22.10   Max.   :5.71200   Max.   :6654.000  
##  NA's   :51                       NA's   :27
```


3. Make a new data frame focused on body weight, but be sure to indicate the common name and genus of each mammal.  


```r
#msleep.sub = tibble(msleep$name, msleep$genus, msleep$bodywt)
#msleep.sub

newmSleep <- subset(msleep,  select=c(name, genus, bodywt))
newmSleep
```

```
## # A tibble: 83 x 3
##    name                       genus        bodywt
##    <chr>                      <chr>         <dbl>
##  1 Cheetah                    Acinonyx     50    
##  2 Owl monkey                 Aotus         0.48 
##  3 Mountain beaver            Aplodontia    1.35 
##  4 Greater short-tailed shrew Blarina       0.019
##  5 Cow                        Bos         600    
##  6 Three-toed sloth           Bradypus      3.85 
##  7 Northern fur seal          Callorhinus  20.5  
##  8 Vesper mouse               Calomys       0.045
##  9 Dog                        Canis        14    
## 10 Roe deer                   Capreolus    14.8  
## # … with 73 more rows
```


4. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. For our study, we are interested in body weight and sleep total Make two new dataframes (large and small) based on these parameters.  


```r
#largemsleep.sub = tibble(msleep$name, msleep$genus, msleep$bodywt, msleep$sleep_total)
#largemsleep.sub
#filter(largemsleep.sub, msleep$bodywt>=200)

#smallmsleep.sub = tibble(msleep$name, msleep$genus, msleep$bodywt, msleep$sleep_total)
#smallmsleep.sub
#filter(smallmsleep.sub, msleep$bodywt <200)

smallmSleep <- subset(msleep, bodywt < 200, select=c(name, genus, bodywt, sleep_total))
smallmSleep
```

```
## # A tibble: 76 x 4
##    name                       genus       bodywt sleep_total
##    <chr>                      <chr>        <dbl>       <dbl>
##  1 Cheetah                    Acinonyx    50            12.1
##  2 Owl monkey                 Aotus        0.48         17  
##  3 Mountain beaver            Aplodontia   1.35         14.4
##  4 Greater short-tailed shrew Blarina      0.019        14.9
##  5 Three-toed sloth           Bradypus     3.85         14.4
##  6 Northern fur seal          Callorhinus 20.5           8.7
##  7 Vesper mouse               Calomys      0.045         7  
##  8 Dog                        Canis       14            10.1
##  9 Roe deer                   Capreolus   14.8           3  
## 10 Goat                       Capri       33.5           5.3
## # … with 66 more rows
```

```r
largemSleep <- subset(msleep, bodywt >= 200, select=c(name, genus, bodywt, sleep_total))
largemSleep
```

```
## # A tibble: 7 x 4
##   name             genus         bodywt sleep_total
##   <chr>            <chr>          <dbl>       <dbl>
## 1 Cow              Bos             600          4  
## 2 Asian elephant   Elephas        2547          3.9
## 3 Horse            Equus           521          2.9
## 4 Giraffe          Giraffa         900.         1.9
## 5 Pilot whale      Globicephalus   800          2.7
## 6 African elephant Loxodonta      6654          3.3
## 7 Brazilian tapir  Tapirus         208.         4.4
```




5. Let's try to figure out if large mammals sleep, on average, longer than small mammals. What is the average sleep duration for large mammals as we have defined them?

```r
#mean(large$sleep_total)
largeMean <- mean(largemSleep$sleep_total)
largeMean
```

```
## [1] 3.3
```

6. What is the average sleep duration for small mammals as we have defined them?

```r
smallMean <- mean(smallmSleep$sleep_total)
smallMean
```

```
## [1] 11.09079
```



7. Which animals sleep at least 18 hours per day? Be sure to show the name, genus, order, and sleep total.  


```r
newmSleep <- subset(msleep, sleep_total >= 18, select=c(name, genus, order, sleep_total))
newmSleep
```

```
## # A tibble: 5 x 4
##   name                   genus      order           sleep_total
##   <chr>                  <chr>      <chr>                 <dbl>
## 1 North American Opossum Didelphis  Didelphimorphia        18  
## 2 Big brown bat          Eptesicus  Chiroptera             19.7
## 3 Thick-tailed opposum   Lutreolina Didelphimorphia        19.4
## 4 Little brown bat       Myotis     Chiroptera             19.9
## 5 Giant armadillo        Priodontes Cingulata              18.1
```


## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
