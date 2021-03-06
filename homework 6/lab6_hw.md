---
title: "Lab 6 Homework"
author: "Valerie Chau"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code, keep track of your versions using git, and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the libraries

```r
#install.packages("tidyverse")
#install.packages("skimr")
#install.packages("RColorBrewer")
library(tidyverse)
library(skimr)
library("RColorBrewer")
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get lost go have a look. Please do not copy and paste her code!  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use.

```r
#install.packages("gapminder")
```


```r
library("gapminder")
```

Please load the data into a new object called gapminder.

```r
gapminder <- 
  gapminder::gapminder
```

1. Explore the data using the various function you have learned. Is it tidy, are there any NA's, what are its dimensions, what are the column names, etc.

```r
#no NAs
glimpse(gapminder)
```

```
## Observations: 1,704
## Variables: 6
## $ country   <fct> Afghanistan, Afghanistan, Afghanistan, Afghanistan, Af…
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, …
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, …
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854…
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 148803…
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.…
```

```r
names(gapminder)
```

```
## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
```

```r
gapminder %>% 
  summarize(number_nas= sum(is.na(gapminder)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1          0
```


2. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer on average. Make a quick plot below to visualize this relationship.

```r
gapminder %>% 
  ggplot(data=gapminder, mapping=aes(x=gdpPercap, y=lifeExp)) +
  geom_point()+
  geom_smooth(method=lm, se=FALSE)+
  labs(title = "GDP per Capita vs Life Expectancy",
       x = "GDP per Capita",
       y = "Life Expectancy (Years)")+ 
  theme(plot.title = element_text(size = rel(1.5)))
```

![](lab6_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
#positive relationship

3. There is extreme disparity in per capita GDP. Rescale the x axis to make this easier to interpret. How would you characterize the relationship?

```r
gapminder %>% 
  ggplot(data=gapminder, mapping=aes(x=gdpPercap, y=lifeExp)) +
  geom_point()+
  geom_smooth(method=lm, se=FALSE)+
  labs(title = "GDP per Capita vs Life Expectancy",
       x = "GDP per Capita",
       y = "Life Expectancy (Years)")+ 
  theme(plot.title = element_text(size = rel(1.5)))+
  scale_x_log10()
```

![](lab6_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

4. This should look pretty dense to you with significant overplotting. Try using a faceting approach to break this relationship down by year.

```r
gapminder %>% 
  ggplot(data=gapminder, mapping=aes(x=gdpPercap, y=lifeExp)) +
  geom_point()+
  geom_smooth(method=lm, se=FALSE)+
  labs(title = "GDP per Capita vs Life Expectancy",
       x = "GDP per Capita",
       y = "Life Expectancy (Years)")+ 
  theme(plot.title = element_text(size = rel(1.5)))+
  scale_x_log10()+
  facet_grid(~year)
```

![](lab6_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


5. Simplify the comparison by comparing only 1952 and 2007. Can you come to any conclusions?

```r
gapminder %>% 
  filter(year==1952 |year==2007) %>% 
  ggplot(mapping=aes(x=gdpPercap, y=lifeExp)) +
  geom_point()+
  geom_smooth(method=lm, se=FALSE)+
  labs(title = "GDP per Capita vs Life Expectancy",
       x = "GDP per Capita",
       y = "Life Expectancy (Years)")+ 
  theme(plot.title = element_text(size = rel(1.5)))+
  scale_x_log10()+
  facet_grid(~year)
```

![](lab6_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


6. Let's stick with the 1952 and 2007 comparison but make some aesthetic adjustments. First try to color by continent and adjust the size of the points by population. Add `+ scale_size(range = c(0.1, 10), guide = "none")` as a layer to clean things up a bit.

```r
gapminder %>% 
  filter(year==1952 | year==2007) %>% 
  ggplot(mapping=aes(x=gdpPercap, y=lifeExp, color=continent, size=pop)) +
  geom_point()+
  #geom_smooth(method=lm, se=FALSE)+
  labs(title = "GDP per Capita vs Life Expectancy",
       x = "GDP per capita",
       y = "Life Expectancy (Years)")+ 
  theme(plot.title = element_text(size = rel(0.9)),
        axis.text=element_text(size=8),
        axis.title=element_text(size=8))+
  scale_x_log10()+
  facet_grid(~year)+
  scale_size(range = c(0.1, 10), guide = "none")
```

![](lab6_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->



 did not introduce them in lab, ggplot has a number of built-in themes that make things easier. I like the light theme for these data, but you can see lots of options. Apply one of these to your plot above.

```r
?theme_light
```

```r
gapminder %>% 
  filter(year==1952 | year==2007) %>% 
  ggplot(mapping=aes(x=gdpPercap, y=lifeExp, color=continent, size=pop)) +
  geom_point()+
  #geom_smooth(method=lm, se=FALSE)+
  labs(title = "GDP per Capita vs Life Expectancy",
       x = "GDP per capita",
       y = "Life Expectancy (Years)")+ 
  theme(plot.title = element_text(size = rel(0.9)),
        axis.text=element_text(size=8),
        axis.title=element_text(size=8))+
  scale_x_log10()+
  facet_grid(~year)+
  scale_size(range = c(0.1, 10), guide = "none")+
  theme_light()
```

![](lab6_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

8. What is the population for all countries on the Asian continent in 2007? Show this as a barplot.

```r
gapminder %>% 
  filter(continent=="Asia") %>% 
  filter(year==2007) %>% 
  ggplot(aes(x=country, y=pop))+
  geom_col()+
  labs(title = "Population of Asian Countries",
       x = "Countries in Asia",
       y = "Population")+ 
  theme(plot.title = element_text(size = rel(1.25)),
        axis.text=element_text(size=2))
```

![](lab6_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


9. You should see that China's population is the largest with India a close second. Let's focus on China only. Make a plot that shows how population has changed over the years.

```r
gapminder %>% 
  filter(country=="China") %>% 
  ggplot(aes(x=year, y=pop))+
  geom_point()+
  labs(title = "Population of China Over the Years",
       x = "Year",
       y = "Population")+ 
  theme(plot.title = element_text(size = rel(1.25)))
```

![](lab6_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

10. Let's compare China and India. Make a barplot that shows population growth by year using `position=dodge`. Apply a custom color theme using RColorBrewer.

```r
gapminder %>% 
  filter(country =="China" | country =="India") %>% 
  ggplot(aes(x=year, y=pop, fill=country))+
  geom_col(alpha=0.9, position="dodge")+
  scale_fill_brewer(palette = "Dark2")
```

![](lab6_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

```r
  labs(title = "Population of China vs India Over the Years",
       x = "Year",
       y = "Population")+ 
  theme(plot.title = element_text(size = rel(1.25)))
```

```
## NULL
```



## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
