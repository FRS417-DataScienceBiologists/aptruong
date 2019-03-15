---
title: "Lab5: Tidy Data 2, Summarize, Dealing with NA's"
author: "April Truong"
date: "Winter 2019, February 8"
output: 
  html_document: 
    keep_md: yes
---



# Lab 4-1: Tidy Data 2 & Summarize

## Lab intro for the day
- group project: think about kind of data want to use; will have to do short presentation & analysis
- create new repository and share things through that repository

### HW Review
> looked at devan's hw
7. year and catch are numerics, so make R recognize as numbers; also need R to recognize "..." as missing data and treat NAs as blanks

## Resources
- [dplyr-tidyr tutorial](http://tclavelle.github.io/dplyr-tidyr-tutorial/)
- [Data wrangling cheatsheet (`dplyr`,`tidyr`)](http://ucsb-bren.github.io/refs/cheatsheets/data-wrangling-cheatsheet.pdf)
- [Tidyr, R for Data Science](https://r4ds.had.co.nz/tidy-data.html#unite)

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Explain the difference between tidy and messy data.  
> (1) *each variable has its own column*    
> (2) *each observation has its own row*  
> (3) *each value has its own cell*      
2. Evaluate a dataset as tidy or untidy.  
3. Use the `spread()` function of tidyr to transform messy data to tidy data. 
4. Use `summarize()` and `group_by()` to produce statistical summaries of data.

# Tidy Data 2 & Summarize: Review

Load tidyverse

```r
library(tidyverse)
```

```
## -- Attaching packages -------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.0     v purrr   0.2.5
## v tibble  1.4.2     v dplyr   0.7.8
## v tidyr   0.8.2     v stringr 1.3.1
## v readr   1.3.1     v forcats 0.3.0
```

```
## -- Conflicts ----------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

## Tidyr
*gather() and spread() convert data between wide and long format*
*separate() and unite() separates or unites information in columns*
> remember that gather() and spread() work in key and values; in gather() use == , but spread() don't need it

## gather()
Recall that we use `gather()` when our column names actually represent variables. A classic example would be that the column names represent observations of a variable.

```r
?USArrests
```

```
## starting httpd help server ... done
```


```r
arrests <- 
  USArrests %>% 
  mutate(State=rownames(USArrests)) %>%  #mutate to make new row name
  select(State, Murder, Assault, Rape)
arrests #arrest per 100,000 individuals
```

```
##             State Murder Assault Rape
## 1         Alabama   13.2     236 21.2
## 2          Alaska   10.0     263 44.5
## 3         Arizona    8.1     294 31.0
## 4        Arkansas    8.8     190 19.5
## 5      California    9.0     276 40.6
## 6        Colorado    7.9     204 38.7
## 7     Connecticut    3.3     110 11.1
## 8        Delaware    5.9     238 15.8
## 9         Florida   15.4     335 31.9
## 10        Georgia   17.4     211 25.8
## 11         Hawaii    5.3      46 20.2
## 12          Idaho    2.6     120 14.2
## 13       Illinois   10.4     249 24.0
## 14        Indiana    7.2     113 21.0
## 15           Iowa    2.2      56 11.3
## 16         Kansas    6.0     115 18.0
## 17       Kentucky    9.7     109 16.3
## 18      Louisiana   15.4     249 22.2
## 19          Maine    2.1      83  7.8
## 20       Maryland   11.3     300 27.8
## 21  Massachusetts    4.4     149 16.3
## 22       Michigan   12.1     255 35.1
## 23      Minnesota    2.7      72 14.9
## 24    Mississippi   16.1     259 17.1
## 25       Missouri    9.0     178 28.2
## 26        Montana    6.0     109 16.4
## 27       Nebraska    4.3     102 16.5
## 28         Nevada   12.2     252 46.0
## 29  New Hampshire    2.1      57  9.5
## 30     New Jersey    7.4     159 18.8
## 31     New Mexico   11.4     285 32.1
## 32       New York   11.1     254 26.1
## 33 North Carolina   13.0     337 16.1
## 34   North Dakota    0.8      45  7.3
## 35           Ohio    7.3     120 21.4
## 36       Oklahoma    6.6     151 20.0
## 37         Oregon    4.9     159 29.3
## 38   Pennsylvania    6.3     106 14.9
## 39   Rhode Island    3.4     174  8.3
## 40 South Carolina   14.4     279 22.5
## 41   South Dakota    3.8      86 12.8
## 42      Tennessee   13.2     188 26.9
## 43          Texas   12.7     201 25.5
## 44           Utah    3.2     120 22.9
## 45        Vermont    2.2      48 11.2
## 46       Virginia    8.5     156 20.7
## 47     Washington    4.0     145 26.2
## 48  West Virginia    5.7      81  9.3
## 49      Wisconsin    2.6      53 10.8
## 50        Wyoming    6.8     161 15.6
```


### Practice: gather()
1. Are these data tidy? Please use `gather()` to tidy the data.
> no, observations are grouped in a single row by stae


```r
tidy_arrests <- arrests %>% 
  gather(Murder, Assault, Rape, key="Crime", value="per100k")
tidy_arrests
```

```
##              State   Crime per100k
## 1          Alabama  Murder    13.2
## 2           Alaska  Murder    10.0
## 3          Arizona  Murder     8.1
## 4         Arkansas  Murder     8.8
## 5       California  Murder     9.0
## 6         Colorado  Murder     7.9
## 7      Connecticut  Murder     3.3
## 8         Delaware  Murder     5.9
## 9          Florida  Murder    15.4
## 10         Georgia  Murder    17.4
## 11          Hawaii  Murder     5.3
## 12           Idaho  Murder     2.6
## 13        Illinois  Murder    10.4
## 14         Indiana  Murder     7.2
## 15            Iowa  Murder     2.2
## 16          Kansas  Murder     6.0
## 17        Kentucky  Murder     9.7
## 18       Louisiana  Murder    15.4
## 19           Maine  Murder     2.1
## 20        Maryland  Murder    11.3
## 21   Massachusetts  Murder     4.4
## 22        Michigan  Murder    12.1
## 23       Minnesota  Murder     2.7
## 24     Mississippi  Murder    16.1
## 25        Missouri  Murder     9.0
## 26         Montana  Murder     6.0
## 27        Nebraska  Murder     4.3
## 28          Nevada  Murder    12.2
## 29   New Hampshire  Murder     2.1
## 30      New Jersey  Murder     7.4
## 31      New Mexico  Murder    11.4
## 32        New York  Murder    11.1
## 33  North Carolina  Murder    13.0
## 34    North Dakota  Murder     0.8
## 35            Ohio  Murder     7.3
## 36        Oklahoma  Murder     6.6
## 37          Oregon  Murder     4.9
## 38    Pennsylvania  Murder     6.3
## 39    Rhode Island  Murder     3.4
## 40  South Carolina  Murder    14.4
## 41    South Dakota  Murder     3.8
## 42       Tennessee  Murder    13.2
## 43           Texas  Murder    12.7
## 44            Utah  Murder     3.2
## 45         Vermont  Murder     2.2
## 46        Virginia  Murder     8.5
## 47      Washington  Murder     4.0
## 48   West Virginia  Murder     5.7
## 49       Wisconsin  Murder     2.6
## 50         Wyoming  Murder     6.8
## 51         Alabama Assault   236.0
## 52          Alaska Assault   263.0
## 53         Arizona Assault   294.0
## 54        Arkansas Assault   190.0
## 55      California Assault   276.0
## 56        Colorado Assault   204.0
## 57     Connecticut Assault   110.0
## 58        Delaware Assault   238.0
## 59         Florida Assault   335.0
## 60         Georgia Assault   211.0
## 61          Hawaii Assault    46.0
## 62           Idaho Assault   120.0
## 63        Illinois Assault   249.0
## 64         Indiana Assault   113.0
## 65            Iowa Assault    56.0
## 66          Kansas Assault   115.0
## 67        Kentucky Assault   109.0
## 68       Louisiana Assault   249.0
## 69           Maine Assault    83.0
## 70        Maryland Assault   300.0
## 71   Massachusetts Assault   149.0
## 72        Michigan Assault   255.0
## 73       Minnesota Assault    72.0
## 74     Mississippi Assault   259.0
## 75        Missouri Assault   178.0
## 76         Montana Assault   109.0
## 77        Nebraska Assault   102.0
## 78          Nevada Assault   252.0
## 79   New Hampshire Assault    57.0
## 80      New Jersey Assault   159.0
## 81      New Mexico Assault   285.0
## 82        New York Assault   254.0
## 83  North Carolina Assault   337.0
## 84    North Dakota Assault    45.0
## 85            Ohio Assault   120.0
## 86        Oklahoma Assault   151.0
## 87          Oregon Assault   159.0
## 88    Pennsylvania Assault   106.0
## 89    Rhode Island Assault   174.0
## 90  South Carolina Assault   279.0
## 91    South Dakota Assault    86.0
## 92       Tennessee Assault   188.0
## 93           Texas Assault   201.0
## 94            Utah Assault   120.0
## 95         Vermont Assault    48.0
## 96        Virginia Assault   156.0
## 97      Washington Assault   145.0
## 98   West Virginia Assault    81.0
## 99       Wisconsin Assault    53.0
## 100        Wyoming Assault   161.0
## 101        Alabama    Rape    21.2
## 102         Alaska    Rape    44.5
## 103        Arizona    Rape    31.0
## 104       Arkansas    Rape    19.5
## 105     California    Rape    40.6
## 106       Colorado    Rape    38.7
## 107    Connecticut    Rape    11.1
## 108       Delaware    Rape    15.8
## 109        Florida    Rape    31.9
## 110        Georgia    Rape    25.8
## 111         Hawaii    Rape    20.2
## 112          Idaho    Rape    14.2
## 113       Illinois    Rape    24.0
## 114        Indiana    Rape    21.0
## 115           Iowa    Rape    11.3
## 116         Kansas    Rape    18.0
## 117       Kentucky    Rape    16.3
## 118      Louisiana    Rape    22.2
## 119          Maine    Rape     7.8
## 120       Maryland    Rape    27.8
## 121  Massachusetts    Rape    16.3
## 122       Michigan    Rape    35.1
## 123      Minnesota    Rape    14.9
## 124    Mississippi    Rape    17.1
## 125       Missouri    Rape    28.2
## 126        Montana    Rape    16.4
## 127       Nebraska    Rape    16.5
## 128         Nevada    Rape    46.0
## 129  New Hampshire    Rape     9.5
## 130     New Jersey    Rape    18.8
## 131     New Mexico    Rape    32.1
## 132       New York    Rape    26.1
## 133 North Carolina    Rape    16.1
## 134   North Dakota    Rape     7.3
## 135           Ohio    Rape    21.4
## 136       Oklahoma    Rape    20.0
## 137         Oregon    Rape    29.3
## 138   Pennsylvania    Rape    14.9
## 139   Rhode Island    Rape     8.3
## 140 South Carolina    Rape    22.5
## 141   South Dakota    Rape    12.8
## 142      Tennessee    Rape    26.9
## 143          Texas    Rape    25.5
## 144           Utah    Rape    22.9
## 145        Vermont    Rape    11.2
## 146       Virginia    Rape    20.7
## 147     Washington    Rape    26.2
## 148  West Virginia    Rape     9.3
## 149      Wisconsin    Rape    10.8
## 150        Wyoming    Rape    15.6
```


2. Restrict the data to assault only. Sort in ascending order.
> use filter() & arrange()


```r
tidy_arrests %>% 
  filter(Crime=="Assault") %>% 
  arrange(desc(per100k))
```

```
##             State   Crime per100k
## 1  North Carolina Assault     337
## 2         Florida Assault     335
## 3        Maryland Assault     300
## 4         Arizona Assault     294
## 5      New Mexico Assault     285
## 6  South Carolina Assault     279
## 7      California Assault     276
## 8          Alaska Assault     263
## 9     Mississippi Assault     259
## 10       Michigan Assault     255
## 11       New York Assault     254
## 12         Nevada Assault     252
## 13       Illinois Assault     249
## 14      Louisiana Assault     249
## 15       Delaware Assault     238
## 16        Alabama Assault     236
## 17        Georgia Assault     211
## 18       Colorado Assault     204
## 19          Texas Assault     201
## 20       Arkansas Assault     190
## 21      Tennessee Assault     188
## 22       Missouri Assault     178
## 23   Rhode Island Assault     174
## 24        Wyoming Assault     161
## 25     New Jersey Assault     159
## 26         Oregon Assault     159
## 27       Virginia Assault     156
## 28       Oklahoma Assault     151
## 29  Massachusetts Assault     149
## 30     Washington Assault     145
## 31          Idaho Assault     120
## 32           Ohio Assault     120
## 33           Utah Assault     120
## 34         Kansas Assault     115
## 35        Indiana Assault     113
## 36    Connecticut Assault     110
## 37       Kentucky Assault     109
## 38        Montana Assault     109
## 39   Pennsylvania Assault     106
## 40       Nebraska Assault     102
## 41   South Dakota Assault      86
## 42          Maine Assault      83
## 43  West Virginia Assault      81
## 44      Minnesota Assault      72
## 45  New Hampshire Assault      57
## 46           Iowa Assault      56
## 47      Wisconsin Assault      53
## 48        Vermont Assault      48
## 49         Hawaii Assault      46
## 50   North Dakota Assault      45
```


## spread()
The opposite of `gather()`. You use `spread()` when you have an observation scattered across multiple rows. In the example below, `cases` and `population` represent variable names not observations.
> long format and short format


```r
country <- c("Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan", "Brazil", "Brazil", "Brazil", "Brazil", "China", "China", "China", "China")
year <- c("1999", "1999", "2000", "2000", "1999", "1999", "2000", "2000", "1999", "1999", "2000", "2000")
key <- c("cases", "population", "cases", "population", "cases", "population", "cases", "population", "cases", "population", "cases", "population")
value <- c(745, 19987071, 2666, 20595360, 37737, 172006362, 80488, 174504898, 212258, 1272915272, 213766, 1280428583)

tb_data <- data.frame(country=country, year=year, key=key, value=value)
tb_data
```

```
##        country year        key      value
## 1  Afghanistan 1999      cases        745
## 2  Afghanistan 1999 population   19987071
## 3  Afghanistan 2000      cases       2666
## 4  Afghanistan 2000 population   20595360
## 5       Brazil 1999      cases      37737
## 6       Brazil 1999 population  172006362
## 7       Brazil 2000      cases      80488
## 8       Brazil 2000 population  174504898
## 9        China 1999      cases     212258
## 10       China 1999 population 1272915272
## 11       China 2000      cases     213766
## 12       China 2000 population 1280428583
```
> run this data and see that afghanistan listed 4 times; each line it's own thing so essentially key and value col are NOT TIDY; need to spread data out; want cases and population to be its own col

When using `spread()` the `key` is the variable that you are spreading. 

```r
#spread into wide format
tb_data %>% 
  spread(key=key, value=value)
```

```
##       country year  cases population
## 1 Afghanistan 1999    745   19987071
## 2 Afghanistan 2000   2666   20595360
## 3      Brazil 1999  37737  172006362
## 4      Brazil 2000  80488  174504898
## 5       China 1999 212258 1272915272
## 6       China 2000 213766 1280428583
```

### Practice: spread()
1. Run the following to build the `gene_exp` data frame.

```r
id <- c("gene1", "gene1", "gene2", "gene2", "gene3", "gene3")
type <- c("treatment", "control", "treatment", "control","treatment", "control")
L4_values <- rnorm(6, mean = 20, sd = 3)
```


```r
gene_exp <- 
  data.frame(gene_id=id, type=type, L4_values=L4_values)
gene_exp
```

```
##   gene_id      type L4_values
## 1   gene1 treatment  19.85141
## 2   gene1   control  23.33931
## 3   gene2 treatment  18.05815
## 4   gene2   control  18.59404
## 5   gene3 treatment  19.45358
## 6   gene3   control  20.85343
```

2. Are these data tidy? Please use `spread()` to tidy the data.

```r
tidy_gene_exp <- gene_exp %>%
  spread(key = "type", value = "L4_values")
tidy_gene_exp
```

```
##   gene_id  control treatment
## 1   gene1 23.33931  19.85141
## 2   gene2 18.59404  18.05815
## 3   gene3 20.85343  19.45358
```

```r
#can also do: spread(type, L4_values)
```
> we learn this to learn how to make nice plots, which most people learn R for

## summarize()
summarize() will produce summary statistics for a given variable in a data frame. For example, in homework 2 you were asked to calculate the mean of the sleep total column for large and small mammals. We did this using a combination of tidyverse and base R commands, which isn't very efficient or clean. It also took two steps.

```r
?msleep
colnames(msleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"       
##  [5] "conservation" "sleep_total"  "sleep_rem"    "sleep_cycle" 
##  [9] "awake"        "brainwt"      "bodywt"
```

From homework 2.

```r
large <- msleep %>% 
  select(name, genus, bodywt, sleep_total) %>% 
  filter(bodywt>=200) %>% 
  arrange(desc(bodywt))
```


```r
mean(large$sleep_total)
```

```
## [1] 3.3
```

We can accomplish the same task using the `summarize()` function in the tidyverse and make things cleaner.
> in summarize(), first filter() to take bodywt greater than 200 --> now summarize() to make a new col


```r
msleep %>% 
  filter(bodywt>=200) %>%
  summarize(mean_sleep_lg=mean(sleep_total))
```

```
## # A tibble: 1 x 1
##   mean_sleep_lg
##           <dbl>
## 1           3.3
```

You can also combine functions to make useful summaries for multiple variables.

```r
msleep %>% 
    filter(bodywt>=200) %>% 
    summarize(mean_sleep_lg = mean(sleep_total), 
              min_sleep_lg = min(sleep_total),
              max_sleep_lg = max(sleep_total),
              sd_sleep_lg=sd(sleep_total), #std dev
              total = n())
```

```
## # A tibble: 1 x 5
##   mean_sleep_lg min_sleep_lg max_sleep_lg sd_sleep_lg total
##           <dbl>        <dbl>        <dbl>       <dbl> <int>
## 1           3.3          1.9          4.4       0.870     7
```
>convinient b/c can now get fast clean clear looking summary of variables

There are many other useful summary statistics, depending on your needs: sd(), min(), max(), median(), sum(), n() (returns the length of vector), first() (returns first value in vector), last() (returns last value in vector) and n_distinct() (number of distinct values in vector).

### Practice: summarize()
1. How many genera are represented in the msleep data frame?

```r
msleep %>% 
  summarize(ngenera=n_distinct(genus)) #decide what fxns to summarize; remember, can't count b/c not numerics and use n_distinct() b/c many duplicates
```

```
## # A tibble: 1 x 1
##   ngenera
##     <int>
## 1      77
```

2. What are the min, max, and mean body weight for all of the mammals? Be sure to include the total n.

```r
msleep %>% 
    summarize(min_bodywt = min(bodywt),
              max_bodywt = max(bodywt),
              mean_bodywt = mean(bodywt), 
              total = n())
```

```
## # A tibble: 1 x 4
##   min_bodywt max_bodywt mean_bodywt total
##        <dbl>      <dbl>       <dbl> <int>
## 1      0.005       6654        166.    83
```

## group_by()
The `summarize()` function is most useful when used in conjunction with `group_by()`. Although producing a summary of body weight for all of the mammals in the dataset is helpful, what if we were interested in body weight by feeding ecology?

```r
# counting can be made easier
msleep %>% 
  count(vore)
```

```
## # A tibble: 5 x 2
##   vore        n
##   <chr>   <int>
## 1 carni      19
## 2 herbi      32
## 3 insecti     5
## 4 omni       20
## 5 <NA>        7
```


```r
# can group by whatever you want and then do summary statistics
msleep %>%
  group_by(vore) %>% #we are grouping by feeding ecology; vore = trophic type(herbivore, carnivore)
  summarize(min_bodywt=min(bodywt),
            max_bodywt=max(bodywt),
            mean_bodywt=mean(bodywt),
            total=n())
```

```
## # A tibble: 5 x 5
##   vore    min_bodywt max_bodywt mean_bodywt total
##   <chr>        <dbl>      <dbl>       <dbl> <int>
## 1 carni        0.028      800        90.8      19
## 2 herbi        0.022     6654       367.       32
## 3 insecti      0.01        60        12.9       5
## 4 omni         0.005       86.2      12.7      20
## 5 <NA>         0.021        3.6       0.858     7
```
> Hint: in data sets you don't know well, this is quick easy way to see what data is like; group_by() and summarize() to get an idea of basic stuff!

### Practice: group_by()
1. Calculate mean brain weight by taxonomic order in the msleep data.

```r
msleep %>% 
  group_by(order) %>% 
  summarize(mean_brainwt=mean(brainwt))
```

```
## # A tibble: 19 x 2
##    order           mean_brainwt
##    <chr>                  <dbl>
##  1 Afrosoricida        0.0026  
##  2 Artiodactyla       NA       
##  3 Carnivora          NA       
##  4 Cetacea            NA       
##  5 Chiroptera          0.000275
##  6 Cingulata           0.0459  
##  7 Didelphimorphia    NA       
##  8 Diprotodontia      NA       
##  9 Erinaceomorpha      0.00295 
## 10 Hyracoidea          0.0152  
## 11 Lagomorpha          0.0121  
## 12 Monotremata         0.025   
## 13 Perissodactyla      0.414   
## 14 Pilosa             NA       
## 15 Primates           NA       
## 16 Proboscidea         5.16    
## 17 Rodentia           NA       
## 18 Scandentia          0.0025  
## 19 Soricomorpha        0.000592
```
> notice: for example, if even just one artiodactyla has one NA, then R is gonna thing the whole order has no brainwts

2. What does `NA` mean?
> 

3. Try running the code again, but this time add `na.rm=TRUE`. What is the problem with Cetacea?

```r
msleep %>% 
  group_by(order) %>% 
  summarize(mean_brainwt=mean(brainwt, na.rm=TRUE)) #remove rows where brainwt = NA
```

```
## # A tibble: 19 x 2
##    order           mean_brainwt
##    <chr>                  <dbl>
##  1 Afrosoricida        0.0026  
##  2 Artiodactyla        0.198   
##  3 Carnivora           0.0986  
##  4 Cetacea           NaN       
##  5 Chiroptera          0.000275
##  6 Cingulata           0.0459  
##  7 Didelphimorphia     0.0063  
##  8 Diprotodontia       0.0114  
##  9 Erinaceomorpha      0.00295 
## 10 Hyracoidea          0.0152  
## 11 Lagomorpha          0.0121  
## 12 Monotremata         0.025   
## 13 Perissodactyla      0.414   
## 14 Pilosa            NaN       
## 15 Primates            0.254   
## 16 Proboscidea         5.16    
## 17 Rodentia            0.00357 
## 18 Scandentia          0.0025  
## 19 Soricomorpha        0.000592
```
> Notice: Cetacea has NaN because no brainwt at all for it


```r
msleep %>% 
  filter(order=="Cetacea")
```

```
## # A tibble: 3 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl>
## 1 Pilo~ Glob~ carni Ceta~ cd                   2.7       0.1          NA
## 2 Comm~ Phoc~ carni Ceta~ vu                   5.6      NA            NA
## 3 Bott~ Turs~ carni Ceta~ <NA>                 5.2      NA            NA
## # ... with 3 more variables: awake <dbl>, brainwt <dbl>, bodywt <dbl>
```
> Notice: there is no brainwt for Cetacea, so can't make a mean of nothing



# Lab 4-2: Dealing with NA's

## Review
In the last section we practiced wrangling untidy data using `tidyr`. We also learned the `summarize()` function and `group_by()` to produce useful summaries of our data. But, we ended the last session with the discovery of NA's and how they can impact analyses. This is a big issue in data science and we will spend the remainder of this lab learning how to manage data with missing values.  

## Learning Goals
*At the end of this exercise, you will be able to:*    
1. Produce summaries of the number of NA's in a data set.  
2. Replace values with `NA` in a data set as appropriate.  

### Load the tidyverse

```r
library("tidyverse")
```

## Dealing with NA's
In almost all scientific data, there are missing observations. These can be tricky to deal with, partly because you first need to determine how missing values were treated in the original study. Scientists use different conventions in showing missing data; some use blank spaces, others use "-", etc. Worse yet, some scientists indicate **missing data with numerics like -999.0!**  
> Think: will always encounter NA; different ways to represent (..., -, NA)

### Practice
1. What are some possible problems if missing data are indicated by "-999.0"?
> Think again: what's the problem with representing NAs as -999.0? R will think it's a number!

### Load the `msleep` data into a new object

```r
msleep <- msleep
```

### Are there any NA's?
Let's first check to see if our data has any NA's. is.na() is a function that determines whether a value in a data frame is or is not an NA. This is evaluated logically as TRUE or FALSE.

```r
is.na(msleep)
```

```
##        name genus  vore order conservation sleep_total sleep_rem
##  [1,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
##  [2,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
##  [3,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [4,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [5,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [6,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
##  [7,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
##  [8,] FALSE FALSE  TRUE FALSE         TRUE       FALSE      TRUE
##  [9,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [10,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [11,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [12,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [13,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [14,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [15,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [16,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [17,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [18,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [19,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [20,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [21,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [22,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [23,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [24,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [25,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [26,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [27,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [28,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [29,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [30,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [31,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [32,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [33,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [34,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [35,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [36,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [37,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [38,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [39,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [40,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [41,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [42,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [43,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [44,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [45,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [46,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [47,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [48,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [49,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [50,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [51,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [52,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [53,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [54,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [55,] FALSE FALSE  TRUE FALSE        FALSE       FALSE     FALSE
## [56,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [57,] FALSE FALSE  TRUE FALSE         TRUE       FALSE      TRUE
## [58,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE
## [59,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [60,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [61,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [62,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [63,] FALSE FALSE  TRUE FALSE        FALSE       FALSE     FALSE
## [64,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [65,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [66,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [67,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [68,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [69,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE
## [70,] FALSE FALSE FALSE FALSE        FALSE       FALSE      TRUE
## [71,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [72,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [73,] FALSE FALSE  TRUE FALSE         TRUE       FALSE     FALSE
## [74,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [75,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [76,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [77,] FALSE FALSE FALSE FALSE        FALSE       FALSE     FALSE
## [78,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [79,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [80,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [81,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
## [82,] FALSE FALSE FALSE FALSE         TRUE       FALSE      TRUE
## [83,] FALSE FALSE FALSE FALSE         TRUE       FALSE     FALSE
##       sleep_cycle awake brainwt bodywt
##  [1,]        TRUE FALSE    TRUE  FALSE
##  [2,]        TRUE FALSE   FALSE  FALSE
##  [3,]        TRUE FALSE    TRUE  FALSE
##  [4,]       FALSE FALSE   FALSE  FALSE
##  [5,]       FALSE FALSE   FALSE  FALSE
##  [6,]       FALSE FALSE    TRUE  FALSE
##  [7,]       FALSE FALSE    TRUE  FALSE
##  [8,]        TRUE FALSE    TRUE  FALSE
##  [9,]       FALSE FALSE   FALSE  FALSE
## [10,]        TRUE FALSE   FALSE  FALSE
## [11,]        TRUE FALSE   FALSE  FALSE
## [12,]       FALSE FALSE   FALSE  FALSE
## [13,]        TRUE FALSE    TRUE  FALSE
## [14,]       FALSE FALSE   FALSE  FALSE
## [15,]        TRUE FALSE   FALSE  FALSE
## [16,]        TRUE FALSE   FALSE  FALSE
## [17,]       FALSE FALSE   FALSE  FALSE
## [18,]       FALSE FALSE   FALSE  FALSE
## [19,]        TRUE FALSE   FALSE  FALSE
## [20,]       FALSE FALSE   FALSE  FALSE
## [21,]        TRUE FALSE   FALSE  FALSE
## [22,]       FALSE FALSE   FALSE  FALSE
## [23,]       FALSE FALSE   FALSE  FALSE
## [24,]        TRUE FALSE   FALSE  FALSE
## [25,]       FALSE FALSE   FALSE  FALSE
## [26,]        TRUE FALSE   FALSE  FALSE
## [27,]        TRUE FALSE    TRUE  FALSE
## [28,]       FALSE FALSE   FALSE  FALSE
## [29,]       FALSE FALSE   FALSE  FALSE
## [30,]        TRUE FALSE    TRUE  FALSE
## [31,]        TRUE FALSE    TRUE  FALSE
## [32,]        TRUE FALSE   FALSE  FALSE
## [33,]        TRUE FALSE   FALSE  FALSE
## [34,]       FALSE FALSE   FALSE  FALSE
## [35,]        TRUE FALSE    TRUE  FALSE
## [36,]        TRUE FALSE   FALSE  FALSE
## [37,]        TRUE FALSE    TRUE  FALSE
## [38,]       FALSE FALSE   FALSE  FALSE
## [39,]        TRUE FALSE    TRUE  FALSE
## [40,]       FALSE FALSE   FALSE  FALSE
## [41,]        TRUE FALSE    TRUE  FALSE
## [42,]       FALSE FALSE   FALSE  FALSE
## [43,]       FALSE FALSE   FALSE  FALSE
## [44,]        TRUE FALSE    TRUE  FALSE
## [45,]        TRUE FALSE   FALSE  FALSE
## [46,]        TRUE FALSE    TRUE  FALSE
## [47,]        TRUE FALSE    TRUE  FALSE
## [48,]       FALSE FALSE   FALSE  FALSE
## [49,]        TRUE FALSE   FALSE  FALSE
## [50,]       FALSE FALSE   FALSE  FALSE
## [51,]        TRUE FALSE    TRUE  FALSE
## [52,]        TRUE FALSE   FALSE  FALSE
## [53,]        TRUE FALSE    TRUE  FALSE
## [54,]       FALSE FALSE   FALSE  FALSE
## [55,]        TRUE FALSE   FALSE  FALSE
## [56,]        TRUE FALSE    TRUE  FALSE
## [57,]        TRUE FALSE    TRUE  FALSE
## [58,]        TRUE FALSE   FALSE  FALSE
## [59,]        TRUE FALSE    TRUE  FALSE
## [60,]        TRUE FALSE    TRUE  FALSE
## [61,]        TRUE FALSE    TRUE  FALSE
## [62,]        TRUE FALSE   FALSE  FALSE
## [63,]        TRUE FALSE   FALSE  FALSE
## [64,]       FALSE FALSE   FALSE  FALSE
## [65,]        TRUE FALSE    TRUE  FALSE
## [66,]        TRUE FALSE   FALSE  FALSE
## [67,]       FALSE FALSE   FALSE  FALSE
## [68,]       FALSE FALSE   FALSE  FALSE
## [69,]        TRUE FALSE   FALSE  FALSE
## [70,]        TRUE FALSE   FALSE  FALSE
## [71,]       FALSE FALSE   FALSE  FALSE
## [72,]        TRUE FALSE    TRUE  FALSE
## [73,]       FALSE FALSE   FALSE  FALSE
## [74,]       FALSE FALSE   FALSE  FALSE
## [75,]        TRUE FALSE   FALSE  FALSE
## [76,]        TRUE FALSE    TRUE  FALSE
## [77,]       FALSE FALSE   FALSE  FALSE
## [78,]        TRUE FALSE   FALSE  FALSE
## [79,]       FALSE FALSE   FALSE  FALSE
## [80,]        TRUE FALSE    TRUE  FALSE
## [81,]        TRUE FALSE   FALSE  FALSE
## [82,]        TRUE FALSE   FALSE  FALSE
## [83,]       FALSE FALSE   FALSE  FALSE
```

OK, what are we supposed to do with that? Unless you have a small data frame, applying the is.na function universally is not helpful but we can use the code in another way. Let's incorporate it into the `summarize()` function.

```r
msleep %>% 
  summarize(number_nas= sum(is.na(msleep)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1        136
```
> keep in mind: may not actually rep # of NA's in df; doesn't tell you where they are either :(

This is better, but we still don't have any idea of where those NA's are in our data frame. If there were a systemic problem in the data it would be hard to determine. In order to do this, we need to apply `is.na` to each variable of interest.

```r
msleep %>% 
  summarize(number_nas= sum(is.na(conservation)))
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1         29
```
> so you could do this for every col, but not feasible with larger datasets

What if we are working with hundreds or thousands (or more!) variables?! In order to deal with this problem efficiently we can use another package in the tidyverse called `purrr`.

```r
msleep_na <- 
  msleep %>%
  purrr::map_df(~ sum(is.na(.))) #map to a new data frame the sum results of the is.na function for all columns
msleep_na
```

```
## # A tibble: 1 x 11
##    name genus  vore order conservation sleep_total sleep_rem sleep_cycle
##   <int> <int> <int> <int>        <int>       <int>     <int>       <int>
## 1     0     0     7     0           29           0        22          51
## # ... with 3 more variables: awake <int>, brainwt <int>, bodywt <int>
```
> what purrr does: take a command and repeat it accross for you (will look for NA's in every col for you)
> now, we can see where the NA's in sf are! super helpful!

Don't forget that we can use `gather()` to make viewing this output easier.

```r
msleep %>% 
  purrr::map_df(~ sum(is.na(.))) %>% 
  tidyr::gather(key="variables", value="num_nas") %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 11 x 2
##    variables    num_nas
##    <chr>          <int>
##  1 sleep_cycle       51
##  2 conservation      29
##  3 brainwt           27
##  4 sleep_rem         22
##  5 vore               7
##  6 name               0
##  7 genus              0
##  8 order              0
##  9 sleep_total        0
## 10 awake              0
## 11 bodywt             0
```
> nice clean clear output; every project should have section on NA's

This is much better, but we need to be careful. R can have difficulty interpreting missing data. This is especially true for categorical variables. Always do a reality check if the output doesn't make sense to you. A quick check never hurts.  

You can explore a specific variable more intently using `count()`. This operates similarly to `group_by()`.

```r
msleep %>% 
  count(conservation)
```

```
## # A tibble: 7 x 2
##   conservation     n
##   <chr>        <int>
## 1 cd               2
## 2 domesticated    10
## 3 en               4
## 4 lc              27
## 5 nt               4
## 6 vu               7
## 7 <NA>            29
```
> this is a reality check you can do. confirm that conservation col has 29 NA's (yes it does)

Adding the `sort=TRUE` option automatically makes a descending list.

```r
msleep %>% 
  count(conservation, sort=TRUE)
```

```
## # A tibble: 7 x 2
##   conservation     n
##   <chr>        <int>
## 1 <NA>            29
## 2 lc              27
## 3 domesticated    10
## 4 vu               7
## 5 en               4
## 6 nt               4
## 7 cd               2
```

It is true that all of this is redundant, but you want to be able to run multiple checks on the data. Remember, just because your code runs without errors doesn't mean it is doing what you intended.  

>so we found NA's. what do we do with them now?

## Replacing NA's
Once you have an idea of how NA's are represented in the data, you can replace them with `NA` so that R can better deal with them. The bit of code below is very handy, especially if the data has NA's represented as **actual values that you want replaced** or if you want to make sure any blanks are treated as NA.

```r
msleep_na2 <- 
  msleep %>% 
  na_if("") #replace x data with NA
msleep_na2
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
#so if the NA's are -999.0, just do 'na_if("-999.0")'
```

### Practice: replacing NA's
1. Did replacing blanks with NA have any effect on the msleep data? Demonstrate this using some code.
> look above

## Practice and Homework #4
We will work together on the next part (time permitting) and this will end up being your homework. Make sure that you save your work and copy and paste your responses into the RMarkdown homework file.

Aren't mammals fun? Let's load up some more mammal data. This will be life history data for mammals. The [data](http://esapubs.org/archive/ecol/E084/093/) are from: *S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.*  
> Note: in our projects, should reference where data is from like this 

See lab4_hw_aptruong in repository for HW

# Wrap-up
Please review the learning goals and be sure to use the code here as a reference when completing the homework.

See you next time!
