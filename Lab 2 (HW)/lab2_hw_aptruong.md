---
title: "Lab 2 Homework"
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
library("tidyverse")
```

```
## -- Attaching packages --------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.0     v purrr   0.2.5
## v tibble  1.4.2     v dplyr   0.7.8
## v tidyr   0.8.2     v stringr 1.3.1
## v readr   1.3.1     v forcats 0.3.0
```

```
## -- Conflicts ------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

## Mammals Sleep
For this assignment, we are going to use built-in data on mammal sleep patterns.  

1. From which publication are these data taken from? Don't do an internet search; show the code that you would use to find out in R.


```r
# read the data first
msleep
```

```
## # A tibble: 83 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
##  1 Chee~ Acin~ carni Carn~ lc                  12.1      NA        NA    
##  2 Owl ~ Aotus omni  Prim~ <NA>                17         1.8      NA    
##  3 Moun~ Aplo~ herbi Rode~ nt                  14.4       2.4      NA    
##  4 Grea~ Blar~ omni  Sori~ lc                  14.9       2.3       0.133
##  5 Cow   Bos   herbi Arti~ domesticated         4         0.7       0.667
##  6 Thre~ Brad~ herbi Pilo~ <NA>                14.4       2.2       0.767
##  7 Nort~ Call~ carni Carn~ vu                   8.7       1.4       0.383
##  8 Vesp~ Calo~ <NA>  Rode~ <NA>                 7        NA        NA    
##  9 Dog   Canis carni Carn~ domesticated        10.1       2.9       0.333
## 10 Roe ~ Capr~ herbi Arti~ lc                   3        NA        NA    
## # ... with 73 more rows, and 3 more variables: awake <dbl>, brainwt <dbl>,
## #   bodywt <dbl>
```

```r
# information
?msleep
```

```
## starting httpd help server ... done
```
> Publication from National Academy of Sciences

2. Provide some summary information about the data to get you started; feel free to use the functions that you find most helpful.


```r
str(msleep)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	83 obs. of  11 variables:
##  $ name        : chr  "Cheetah" "Owl monkey" "Mountain beaver" "Greater short-tailed shrew" ...
##  $ genus       : chr  "Acinonyx" "Aotus" "Aplodontia" "Blarina" ...
##  $ vore        : chr  "carni" "omni" "herbi" "omni" ...
##  $ order       : chr  "Carnivora" "Primates" "Rodentia" "Soricomorpha" ...
##  $ conservation: chr  "lc" NA "nt" "lc" ...
##  $ sleep_total : num  12.1 17 14.4 14.9 4 14.4 8.7 7 10.1 3 ...
##  $ sleep_rem   : num  NA 1.8 2.4 2.3 0.7 2.2 1.4 NA 2.9 NA ...
##  $ sleep_cycle : num  NA NA NA 0.133 0.667 ...
##  $ awake       : num  11.9 7 9.6 9.1 20 9.6 15.3 17 13.9 21 ...
##  $ brainwt     : num  NA 0.0155 NA 0.00029 0.423 NA NA NA 0.07 0.0982 ...
##  $ bodywt      : num  50 0.48 1.35 0.019 600 ...
```
> Note: other functions include 'summary()' and 'glimpse()'(which is part of the tidyverse)


```r
## other functions

#names(msleep)

#nrow(msleep) #the number of rows

#ncol(msleep) #the number of columns

#dim(msleep) #total dimensions

#colnames(msleep) #column names

#head(msleep) #first 6 

#tail(msleep) #last 6
```


3. Make a new data frame focused on body weight, but be sure to indicate the common name and genus of each mammal. Sort the data in descending order by body weight.

```r
# practice with pipes, wish I was playing Smash Ultimate instead

IRL_yoshi_bodywt <-
  msleep %>%
  select (name, genus, bodywt)

IRL_yoshi_bodywt
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
## # ... with 73 more rows
```


```r
# sort in descending order

IRL_yoshi_bodywt[order(-IRL_yoshi_bodywt$bodywt),]
```

```
## # A tibble: 83 x 3
##    name                 genus         bodywt
##    <chr>                <chr>          <dbl>
##  1 African elephant     Loxodonta      6654 
##  2 Asian elephant       Elephas        2547 
##  3 Giraffe              Giraffa         900.
##  4 Pilot whale          Globicephalus   800 
##  5 Cow                  Bos             600 
##  6 Horse                Equus           521 
##  7 Brazilian tapir      Tapirus         208.
##  8 Donkey               Equus           187 
##  9 Bottle-nosed dolphin Tursiops        173.
## 10 Tiger                Panthera        163.
## # ... with 73 more rows
```


4. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. For our study, we are interested in body weight and sleep total.
Make two new objects (large and small) based on these parameters. Sort the data in descending order by body weight.


```r
# small group

small_group <- msleep %>%
  select(sleep_total, bodywt)%>%
  filter(bodywt <= 1) 
small_group
```

```
## # A tibble: 36 x 2
##    sleep_total bodywt
##          <dbl>  <dbl>
##  1        17    0.48 
##  2        14.9  0.019
##  3         7    0.045
##  4         9.4  0.728
##  5        12.5  0.42 
##  6        10.3  0.06 
##  7         8.3  1    
##  8         9.1  0.005
##  9        19.7  0.023
## 10        10.1  0.77 
## # ... with 26 more rows
```

```r
small_group[order(-small_group$bodywt),]
```

```
## # A tibble: 36 x 2
##    sleep_total bodywt
##          <dbl>  <dbl>
##  1         8.3  1    
##  2        16.6  0.92 
##  3        15.6  0.9  
##  4        10.1  0.77 
##  5         9.6  0.743
##  6         9.4  0.728
##  7        10.3  0.55 
##  8        17    0.48 
##  9        12.5  0.42 
## 10        19.4  0.37 
## # ... with 26 more rows
```


```r
# large group

large_group <- msleep %>%
  select(sleep_total, bodywt)%>%
  filter(bodywt >= 200) 
large_group
```

```
## # A tibble: 7 x 2
##   sleep_total bodywt
##         <dbl>  <dbl>
## 1         4     600 
## 2         3.9  2547 
## 3         2.9   521 
## 4         1.9   900.
## 5         2.7   800 
## 6         3.3  6654 
## 7         4.4   208.
```

```r
large_group[order(-large_group$bodywt),]
```

```
## # A tibble: 7 x 2
##   sleep_total bodywt
##         <dbl>  <dbl>
## 1         3.3  6654 
## 2         3.9  2547 
## 3         1.9   900.
## 4         2.7   800 
## 5         4     600 
## 6         2.9   521 
## 7         4.4   208.
```


5. Let's try to figure out if large mammals sleep, on average, longer than small mammals. What is the average sleep duration for large mammals as we have defined them?


```r
mean(large_group$sleep_total)
```

```
## [1] 3.3
```
> Average sleep duration for large mammals around 3 hours

6. What is the average sleep duration for small mammals as we have defined them?


```r
mean(small_group$sleep_total)
```

```
## [1] 12.65833
```
> Average sleep duration for small mammals almost 13 hours

7. Which animals sleep at least 18 hours per day? Be sure to show the name, genus, order, and sleep total. Sort by order and sleep total.


```r
msleep %>%
  select (name, genus, order, sleep_total)%>% 
  filter (sleep_total>=18) %>%
  arrange (order, sleep_total)
```

```
## # A tibble: 5 x 4
##   name                   genus      order           sleep_total
##   <chr>                  <chr>      <chr>                 <dbl>
## 1 Big brown bat          Eptesicus  Chiroptera             19.7
## 2 Little brown bat       Myotis     Chiroptera             19.9
## 3 Giant armadillo        Priodontes Cingulata              18.1
## 4 North American Opossum Didelphis  Didelphimorphia        18  
## 5 Thick-tailed opposum   Lutreolina Didelphimorphia        19.4
```
> five animals sleep at least 18 hours per day: big brown bat, little brown bat, giant armadillo, north american opossum, thick-tailed opposum

## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.
