---
title: "Lab 4 Homework"
author: "April Truong"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: cerulean
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
life_history <- readr::read_csv("C:/Users/Apple/Desktop/aptruong/data/mammal_lifehistories_v2.csv")
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


```r
life_history %>% 
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 1440 
##  n variables: 13 
## 
## -- Variable type:character ---------------------
##  variable missing complete    n min max empty n_unique
##    family       0     1440 1440   6  15     0       96
##     genus       0     1440 1440   3  16     0      618
##     order       0     1440 1440   7  14     0       17
##   species       0     1440 1440   3  17     0     1191
## 
## -- Variable type:numeric -----------------------
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
##    15.61     210       <U+2586><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587><U+2581>
##     4.5       21.46    <U+2583><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
##     3.83      14.18    <U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
##     1.15       7.5     <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
##  7009.17       1.5e+08 <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##   147.25    1368       <U+2587><U+2581><U+2581><U+2583><U+2582><U+2581><U+2581><U+2581>
##    98    2250000       <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##    10          1.9e+07 <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##     2         48       <U+2586><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
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
> R says that there are 0 Na's, which does not seem likely since there are many -999 values.

3. Are there any missing data (NA's) represented by different values? How much and where? In which variables do we have the most missing data? Can you think of a reason why so much data are missing in this variable?


```r
life_history_replace <- 
  life_history %>% 
  na_if("-999") #replace x data with NA
life_history_replace
```

```
## # A tibble: 1,440 x 13
##    order family genus species   mass gestation newborn weaning wean_mass
##    <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>     <dbl>
##  1 Arti~ Antil~ Anti~ americ~ 4.54e4      8.13   3246.    3         8900
##  2 Arti~ Bovid~ Addax nasoma~ 1.82e5      9.39   5480     6.5         NA
##  3 Arti~ Bovid~ Aepy~ melamp~ 4.15e4      6.35   5093     5.63     15900
##  4 Arti~ Bovid~ Alce~ busela~ 1.50e5      7.9   10167.    6.5         NA
##  5 Arti~ Bovid~ Ammo~ clarkei 2.85e4      6.8      NA    NA           NA
##  6 Arti~ Bovid~ Ammo~ lervia  5.55e4      5.08   3810     4           NA
##  7 Arti~ Bovid~ Anti~ marsup~ 3.00e4      5.72   3910     4.04        NA
##  8 Arti~ Bovid~ Anti~ cervic~ 3.75e4      5.5    3846     2.13        NA
##  9 Arti~ Bovid~ Bison bison   4.98e5      8.93  20000    10.7     157500
## 10 Arti~ Bovid~ Bison bonasus 5.00e5      9.14  23000.    6.6         NA
## # ... with 1,430 more rows, and 4 more variables: AFR <dbl>,
## #   max_life <dbl>, litter_size <dbl>, litters_yr <dbl>
```

```r
msleep %>% 
  summarize(number_nas= sum(is.na(life_history_replace))) 
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1       4977
```

```r
#total of 4977 NAs

life_history_replace %>% 
  purrr::map_df(~ sum(is.na(.))) %>%
  gather(variable, value = "number_nas") %>%
  arrange(desc(number_nas))
```

```
## # A tibble: 13 x 2
##    variable    number_nas
##    <chr>            <int>
##  1 wean_mass         1039
##  2 max_life           841
##  3 litters_yr         689
##  4 weaning            619
##  5 AFR                607
##  6 newborn            595
##  7 gestation          418
##  8 mass                85
##  9 litter_size         84
## 10 order                0
## 11 family               0
## 12 genus                0
## 13 species              0
```

```r
#most NAs from wean_mass
```
> Most missing data from variable wean_mass. According to Wikipedia, weaning is the process of gradually switching mammals from their mother's milk to other food. It would be difficult to determine a wean mass since there are so many factors, such as timing and methods of weaning, that can influence a weaning weight. Standardizing an exact moment of when to calculate wean mass for each mammal would be even more difficult, especially considering that not all mammals listed have their newborns weanings extensively studied.


4. Compared to the `msleep` data, we have better representation among taxa. Produce a summary that shows the number of observations by taxonomic order.

```r
taxa_sum <- life_history_replace %>%
  group_by(order) %>% 
  summarize(total_n = n()) #n = # of obs
taxa_sum
```

```
## # A tibble: 17 x 2
##    order          total_n
##    <chr>            <int>
##  1 Artiodactyla       161
##  2 Carnivora          197
##  3 Cetacea             55
##  4 Dermoptera           2
##  5 Hyracoidea           4
##  6 Insectivora         91
##  7 Lagomorpha          42
##  8 Macroscelidea       10
##  9 Perissodactyla      15
## 10 Pholidota            7
## 11 Primates           156
## 12 Proboscidea          2
## 13 Rodentia           665
## 14 Scandentia           7
## 15 Sirenia              5
## 16 Tubulidentata        1
## 17 Xenarthra           20
```


5. Mammals have a range of life histories, including lifespan. Produce a summary of lifespan in years by order. Be sure to include the minimum, maximum, mean, standard deviation, and total n.

```r
life_history_replace %>%
  mutate(lifespan = max_life/12) %>% #create lifespan var, have to convert from months to years
  group_by(order) %>%
  summarize(min = min(lifespan, na.rm=TRUE), #ignore NA values
            max = max(lifespan, na.rm=TRUE),
            mean = mean(lifespan, na.rm=TRUE),
            sd = sd(lifespan, na.rm=TRUE),
            total = n())
```

```
## # A tibble: 17 x 6
##    order            min    max  mean      sd total
##    <chr>          <dbl>  <dbl> <dbl>   <dbl> <int>
##  1 Artiodactyla    6.75  61    20.7    7.70    161
##  2 Carnivora       5     56    21.1    9.42    197
##  3 Cetacea        13    114    48.8   27.7      55
##  4 Dermoptera     17.5   17.5  17.5  NaN         2
##  5 Hyracoidea     11     12.2  11.6    0.884     4
##  6 Insectivora     1.17  13     3.85   2.90     91
##  7 Lagomorpha      6     18     9.02   3.85     42
##  8 Macroscelidea   3      8.75  5.69   2.39     10
##  9 Perissodactyla 21     50    35.5   10.2      15
## 10 Pholidota      20     20    20    NaN         7
## 11 Primates        8.83  60.5  25.7   11.0     156
## 12 Proboscidea    70     80    75      7.07      2
## 13 Rodentia        1     35     6.99   5.30    665
## 14 Scandentia      2.67  12.4   8.86   5.38      7
## 15 Sirenia        12.5   73    43.2   30.3       5
## 16 Tubulidentata  24     24    24    NaN         1
## 17 Xenarthra       9     40    21.3    9.93     20
```


6. Let's look closely at gestation and newborns. Summarize the mean gestation, newborn mass, and weaning mass by order. Add a new column that shows mean gestation in years and sort in descending order. Which group has the longest mean gestation? What is the common name for these mammals?

```r
life_history_newborns <- life_history_replace %>% 
  group_by(order) %>% 
  summarize(gestation_yr_mean = mean(gestation/12, na.rm=TRUE),
            gestation_mo_mean = mean(gestation, na.rm=TRUE),
            newborn_mass_mean = mean(newborn, na.rm=TRUE),
            wean_mass_mean = mean(wean_mass, na.rm=TRUE)) %>% 
  arrange(desc(gestation_yr_mean))
life_history_newborns
```

```
## # A tibble: 17 x 5
##    order  gestation_yr_me~ gestation_mo_me~ newborn_mass_me~ wean_mass_mean
##    <chr>             <dbl>            <dbl>            <dbl>          <dbl>
##  1 Probo~           1.77              21.3          99523.         600000  
##  2 Peris~           1.09              13.0          27015.         382191. 
##  3 Cetac~           0.983             11.8         343077.        4796125  
##  4 Siren~           0.9               10.8          22878.          67500  
##  5 Hyrac~           0.617              7.4            231.            500  
##  6 Artio~           0.605              7.26          7082.          51025. 
##  7 Tubul~           0.59               7.08          1734            6250  
##  8 Prima~           0.456              5.47           287.           2115. 
##  9 Xenar~           0.412              4.95           314.            420  
## 10 Carni~           0.308              3.69          3657.          21020. 
## 11 Pholi~           0.302              3.63           276.           2006. 
## 12 Dermo~           0.229              2.75            35.9           NaN  
## 13 Macro~           0.159              1.91            24.5           104. 
## 14 Scand~           0.136              1.63            12.8           102. 
## 15 Roden~           0.109              1.31            35.5           135. 
## 16 Lagom~           0.0979             1.18            57.0           715. 
## 17 Insec~           0.0958             1.15             6.06           33.1
```
> order Proboscidea has the longest mean gestation (of around 1.77 years).
> common name = elephants

## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
