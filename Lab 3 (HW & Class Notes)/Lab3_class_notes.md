---
title: "Lab 3 (Review of Lab 2-2): Data Frames and filter()"
author: "April Truong"
date: "Winter 2019, January 25"
output: 
  html_document: 
    keep_md: yes
---
 
 
## Beginning of Class

Always check directory

```r
getwd()
```

```
## [1] "C:/Users/Apple/Desktop/aptruong/Lab 3 (HW & Class Notes)"
```

Make sure to call/load tidyverse; ignore whatever is outputted

```r
library("tidyverse")
```

```
## -- Attaching packages ------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.0     v purrr   0.2.5
## v tibble  1.4.2     v dplyr   0.7.8
## v tidyr   0.8.2     v stringr 1.3.1
## v readr   1.3.1     v forcats 0.3.0
```

```
## -- Conflicts ---------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


## Reading the fish data

Begin by pulling up your data
  Copy and past working directory into the read_csv()
  Separated by /
Check Environment for the data

```r
fish <- readr::read_csv("C:/Users/Apple/Desktop/aptruong/data/Gaeta_etal_CLC_data.csv")
```

```
## Parsed with column specification:
## cols(
##   lakeid = col_character(),
##   fish_id = col_double(),
##   annnumber = col_character(),
##   length = col_double(),
##   radii_length_mm = col_double(),
##   scalelength = col_double()
## )
```


## Summary Functions

Output column names; remind self what you're working with

```r
names(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```


```r
nrow(fish) #the number of rows
```

```
## [1] 4033
```


```r
ncol(fish) #the number of columns
```

```
## [1] 6
```


```r
dim(fish) #total dimensions
```

```
## [1] 4033    6
```


```r
colnames(fish) #column names
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```

First 6 
Beneath each col names is class of data 
    dbl - numeric
    chr - character
    

```r
head(fish)
```

```
## # A tibble: 6 x 6
##   lakeid fish_id annnumber length radii_length_mm scalelength
##   <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
## 1 AL         299 EDGE         167            2.70        2.70
## 2 AL         299 2            167            2.04        2.70
## 3 AL         299 1            167            1.31        2.70
## 4 AL         300 EDGE         175            3.02        3.02
## 5 AL         300 3            175            2.67        3.02
## 6 AL         300 2            175            2.14        3.02
```

`summary()` and `str()` are classic functions used by many R programmers. `glimpse()` is part of the tidyverse.

- Summary (don't confuse with summarize)
  Give statistics to go along with each col

```r
summary(fish)
```

```
##     lakeid             fish_id       annnumber             length     
##  Length:4033        Min.   :  1.0   Length:4033        Min.   : 58.0  
##  Class :character   1st Qu.:156.0   Class :character   1st Qu.:253.0  
##  Mode  :character   Median :267.0   Mode  :character   Median :299.0  
##                     Mean   :258.3                      Mean   :293.3  
##                     3rd Qu.:376.0                      3rd Qu.:342.0  
##                     Max.   :478.0                      Max.   :420.0  
##  radii_length_mm    scalelength     
##  Min.   : 0.4569   Min.   : 0.6282  
##  1st Qu.: 2.3252   1st Qu.: 4.2596  
##  Median : 3.5380   Median : 5.4062  
##  Mean   : 3.6589   Mean   : 5.3821  
##  3rd Qu.: 4.8229   3rd Qu.: 6.4145  
##  Max.   :11.0258   Max.   :11.0258
```

- str

```r
str(fish)
```

```
## Classes 'spec_tbl_df', 'tbl_df', 'tbl' and 'data.frame':	4033 obs. of  6 variables:
##  $ lakeid         : chr  "AL" "AL" "AL" "AL" ...
##  $ fish_id        : num  299 299 299 300 300 300 300 301 301 301 ...
##  $ annnumber      : chr  "EDGE" "2" "1" "EDGE" ...
##  $ length         : num  167 167 167 175 175 175 175 194 194 194 ...
##  $ radii_length_mm: num  2.7 2.04 1.31 3.02 2.67 ...
##  $ scalelength    : num  2.7 2.7 2.7 3.02 3.02 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   lakeid = col_character(),
##   ..   fish_id = col_double(),
##   ..   annnumber = col_character(),
##   ..   length = col_double(),
##   ..   radii_length_mm = col_double(),
##   ..   scalelength = col_double()
##   .. )
```

- Glimpse

```r
glimpse(fish)
```

```
## Observations: 4,033
## Variables: 6
## $ lakeid          <chr> "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL"...
## $ fish_id         <dbl> 299, 299, 299, 300, 300, 300, 300, 301, 301, 3...
## $ annnumber       <chr> "EDGE", "2", "1", "EDGE", "3", "2", "1", "EDGE...
## $ length          <dbl> 167, 167, 167, 175, 175, 175, 175, 194, 194, 1...
## $ radii_length_mm <dbl> 2.697443, 2.037518, 1.311795, 3.015477, 2.6707...
## $ scalelength     <dbl> 2.697443, 2.697443, 2.697443, 3.015477, 3.0154...
```

## Practice: Data Frames (from Lab 2-2 Review)

1. Load the data `mammal_lifehistories_v2.csv` and place it into a new object called `mammals`.

```r
mammals <- readr::read_csv("C:/Users/Apple/Desktop/aptruong/data/mammal_lifehistories_v2.csv")
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

2. Provide the dimensions of the data frame.

```r
dim(mammals) #total dimensions
```

```
## [1] 1440   13
```

3. Display the column names in the data frame. 

```r
colnames(mammals)
```

```
##  [1] "order"        "family"       "Genus"        "species"     
##  [5] "mass"         "gestation"    "newborn"      "weaning"     
##  [9] "wean mass"    "AFR"          "max. life"    "litter size" 
## [13] "litters/year"
```

4. Use str() to show the structure of the data frame and its individual columns; compare this to glimpse(). 

```r
str(mammals)
```

```
## Classes 'spec_tbl_df', 'tbl_df', 'tbl' and 'data.frame':	1440 obs. of  13 variables:
##  $ order       : chr  "Artiodactyla" "Artiodactyla" "Artiodactyla" "Artiodactyla" ...
##  $ family      : chr  "Antilocapridae" "Bovidae" "Bovidae" "Bovidae" ...
##  $ Genus       : chr  "Antilocapra" "Addax" "Aepyceros" "Alcelaphus" ...
##  $ species     : chr  "americana" "nasomaculatus" "melampus" "buselaphus" ...
##  $ mass        : num  45375 182375 41480 150000 28500 ...
##  $ gestation   : num  8.13 9.39 6.35 7.9 6.8 5.08 5.72 5.5 8.93 9.14 ...
##  $ newborn     : num  3246 5480 5093 10167 -999 ...
##  $ weaning     : num  3 6.5 5.63 6.5 -999 ...
##  $ wean mass   : num  8900 -999 15900 -999 -999 ...
##  $ AFR         : num  13.5 27.3 16.7 23 -999 ...
##  $ max. life   : num  142 308 213 240 -999 251 228 255 300 324 ...
##  $ litter size : num  1.85 1 1 1 1 1.37 1 1 1 1 ...
##  $ litters/year: num  1 0.99 0.95 -999 -999 2 -999 1.89 1 1 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   order = col_character(),
##   ..   family = col_character(),
##   ..   Genus = col_character(),
##   ..   species = col_character(),
##   ..   mass = col_double(),
##   ..   gestation = col_double(),
##   ..   newborn = col_double(),
##   ..   weaning = col_double(),
##   ..   `wean mass` = col_double(),
##   ..   AFR = col_double(),
##   ..   `max. life` = col_double(),
##   ..   `litter size` = col_double(),
##   ..   `litters/year` = col_double()
##   .. )
```

5. Print out the first few rows of the data using the function head().  

```r
head(mammals)
```

```
## # A tibble: 6 x 13
##   order family Genus species   mass gestation newborn weaning `wean mass`
##   <chr> <chr>  <chr> <chr>    <dbl>     <dbl>   <dbl>   <dbl>       <dbl>
## 1 Arti~ Antil~ Anti~ americ~  45375      8.13   3246.    3           8900
## 2 Arti~ Bovid~ Addax nasoma~ 182375      9.39   5480     6.5         -999
## 3 Arti~ Bovid~ Aepy~ melamp~  41480      6.35   5093     5.63       15900
## 4 Arti~ Bovid~ Alce~ busela~ 150000      7.9   10167.    6.5         -999
## 5 Arti~ Bovid~ Ammo~ clarkei  28500      6.8    -999  -999           -999
## 6 Arti~ Bovid~ Ammo~ lervia   55500      5.08   3810     4           -999
## # ... with 4 more variables: AFR <dbl>, `max. life` <dbl>, `litter
## #   size` <dbl>, `litters/year` <dbl>
```


## Dplyr: filter()

    Interested in smaller subset of the data. In order to extract data from dataframe, use filter()

First thing: keep an idea of colnames

```r
colnames(fish)
```

```
## [1] "lakeid"          "fish_id"         "annnumber"       "length"         
## [5] "radii_length_mm" "scalelength"
```

What filters do you want to filter by? 
If only interested in L fish, use filter to only filter those
    Note: make sure to use ==, R distinguises b/t the two

```r
filter(fish, lakeid =="AL")
```

```
## # A tibble: 383 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # ... with 373 more rows
```
Now given new data frame restricted to AL lakes

Let's filter more, by numeric instead of character: pull rows where scale legnth are > or = to 350

```r
filter(fish, length >= 350 )
```

```
## # A tibble: 890 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         306 EDGE         350            6.94        6.94
##  2 AL         306 10           350            6.46        6.94
##  3 AL         306 9            350            6.16        6.94
##  4 AL         306 8            350            5.88        6.94
##  5 AL         306 7            350            5.42        6.94
##  6 AL         306 6            350            4.90        6.94
##  7 AL         306 5            350            4.46        6.94
##  8 AL         306 4            350            3.75        6.94
##  9 AL         306 3            350            2.93        6.94
## 10 AL         306 2            350            2.14        6.94
## # ... with 880 more rows
```
Reminder: not storing this data yet

Combine filters

```r
filter(fish, length == 350 & lakeid == "AL")
```

```
## # A tibble: 11 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         306 EDGE         350            6.94        6.94
##  2 AL         306 10           350            6.46        6.94
##  3 AL         306 9            350            6.16        6.94
##  4 AL         306 8            350            5.88        6.94
##  5 AL         306 7            350            5.42        6.94
##  6 AL         306 6            350            4.90        6.94
##  7 AL         306 5            350            4.46        6.94
##  8 AL         306 4            350            3.75        6.94
##  9 AL         306 3            350            2.93        6.94
## 10 AL         306 2            350            2.14        6.94
## 11 AL         306 1            350            1.19        6.94
```

Using AND and OR

```r
filter(fish, length == 167 & length == 175)
```

```
## # A tibble: 0 x 6
## # ... with 6 variables: lakeid <chr>, fish_id <dbl>, annnumber <chr>,
## #   length <dbl>, radii_length_mm <dbl>, scalelength <dbl>
```
Think: are there fish with 167 length? 175 length? Well yea, but this is asking for a fish that has BOTH 167 and 175 length. So use OR '|'

```r
filter(fish, length == 167 | length == 175)
```

```
## # A tibble: 18 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167           2.70         2.70
##  2 AL         299 2            167           2.04         2.70
##  3 AL         299 1            167           1.31         2.70
##  4 AL         300 EDGE         175           3.02         3.02
##  5 AL         300 3            175           2.67         3.02
##  6 AL         300 2            175           2.14         3.02
##  7 AL         300 1            175           1.23         3.02
##  8 BO         397 EDGE         175           2.67         2.67
##  9 BO         397 3            175           2.39         2.67
## 10 BO         397 2            175           1.59         2.67
## 11 BO         397 1            175           0.830        2.67
## 12 LSG         45 EDGE         175           3.21         3.21
## 13 LSG         45 3            175           2.92         3.21
## 14 LSG         45 2            175           2.44         3.21
## 15 LSG         45 1            175           1.60         3.21
## 16 RD         103 EDGE         167           2.80         2.80
## 17 RD         103 2            167           2.10         2.80
## 18 RD         103 1            167           1.31         2.80
```

Using '!'to filter for NOT

```r
filter(fish, !length == 175)
```

```
## # A tibble: 4,021 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         301 EDGE         194            3.34        3.34
##  5 AL         301 3            194            2.97        3.34
##  6 AL         301 2            194            2.29        3.34
##  7 AL         301 1            194            1.55        3.34
##  8 AL         302 EDGE         324            6.07        6.07
##  9 AL         302 9            324            5.73        6.07
## 10 AL         302 8            324            5.41        6.07
## # ... with 4,011 more rows
```


## Practice: filter() (from Lab 2-2 Review)

1. Filter the `fish` data to include the samples from lake `DY`.

```r
filter(fish, lakeid == "DY")
```

```
## # A tibble: 355 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 DY         359 EDGE         124           1.68         1.68
##  2 DY         359 2            124           1.35         1.68
##  3 DY         359 1            124           0.594        1.68
##  4 DY         360 EDGE         233           3.85         3.85
##  5 DY         360 6            233           3.34         3.85
##  6 DY         360 5            233           3.03         3.85
##  7 DY         360 4            233           2.60         3.85
##  8 DY         360 3            233           2.24         3.85
##  9 DY         360 2            233           1.71         3.85
## 10 DY         360 1            233           0.832        3.85
## # ... with 345 more rows
```

2. Filter the data to include all lakes except AL.

```r
filter(fish, !lakeid == "AL")
```

```
## # A tibble: 3,650 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AR         269 EDGE         140            2.01        2.01
##  2 AR         269 1            140            1.48        2.01
##  3 AR         270 EDGE         193            2.66        2.66
##  4 AR         270 3            193            2.39        2.66
##  5 AR         270 2            193            2.03        2.66
##  6 AR         270 1            193            1.42        2.66
##  7 AR         271 EDGE         220            3.50        3.50
##  8 AR         271 5            220            3.13        3.50
##  9 AR         271 4            220            2.86        3.50
## 10 AR         271 3            220            2.63        3.50
## # ... with 3,640 more rows
```

3. Filter the data to include all lakes except AL and DY.

```r
filter(fish, !lakeid == "AL" & !lakeid == "DY")
```

```
## # A tibble: 3,295 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AR         269 EDGE         140            2.01        2.01
##  2 AR         269 1            140            1.48        2.01
##  3 AR         270 EDGE         193            2.66        2.66
##  4 AR         270 3            193            2.39        2.66
##  5 AR         270 2            193            2.03        2.66
##  6 AR         270 1            193            1.42        2.66
##  7 AR         271 EDGE         220            3.50        3.50
##  8 AR         271 5            220            3.13        3.50
##  9 AR         271 4            220            2.86        3.50
## 10 AR         271 3            220            2.63        3.50
## # ... with 3,285 more rows
```

4. Filter the data to include all fish with a scale length greater than or equal to 11.

```r
filter(fish, scalelength >= 11)
```

```
## # A tibble: 17 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 WS         180 EDGE         403           11.0         11.0
##  2 WS         180 16           403           10.6         11.0
##  3 WS         180 15           403           10.3         11.0
##  4 WS         180 14           403            9.93        11.0
##  5 WS         180 13           403            9.56        11.0
##  6 WS         180 12           403            9.17        11.0
##  7 WS         180 11           403            8.62        11.0
##  8 WS         180 10           403            8.15        11.0
##  9 WS         180 9            403            7.49        11.0
## 10 WS         180 8            403            6.97        11.0
## 11 WS         180 7            403            6.24        11.0
## 12 WS         180 6            403            5.41        11.0
## 13 WS         180 5            403            4.98        11.0
## 14 WS         180 4            403            4.22        11.0
## 15 WS         180 3            403            3.04        11.0
## 16 WS         180 2            403            2.03        11.0
## 17 WS         180 1            403            1.19        11.0
```

5. Filter the data to include fish only from lake AL and with a scalelength greater than or equal to 2 and less than or equal to 4.

```r
fish%>%
  select(lakeid, scalelength)%>%
  filter(lakeid=="AL")%>%
  filter(scalelength >= 2 & scalelength <= 4)
```

```
## # A tibble: 11 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## 11 AL            3.34
```

Another way to do this is to separate by commas (create independent filter operations)

```r
filter(fish, lakeid =="AL",
       scalelength >= 2 & scalelength <= 4)
```

```
## # A tibble: 11 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## 11 AL         301 1            194            1.55        3.34
```
Note: the '&' restricts it to ONLY scalelengths of 2 and 4


## Pipes (%>%)
  Super helpful b/c makes codes cleaner
  Think about "flowing data into another command"     
    #supermariobros
  Shortcut: ctrl + shift + m


```r
fish %>% #take fish and pipe it to function filter()
  filter(lakeid == "AL")
```

```
## # A tibble: 383 x 6
##    lakeid fish_id annnumber length radii_length_mm scalelength
##    <chr>    <dbl> <chr>      <dbl>           <dbl>       <dbl>
##  1 AL         299 EDGE         167            2.70        2.70
##  2 AL         299 2            167            2.04        2.70
##  3 AL         299 1            167            1.31        2.70
##  4 AL         300 EDGE         175            3.02        3.02
##  5 AL         300 3            175            2.67        3.02
##  6 AL         300 2            175            2.14        3.02
##  7 AL         300 1            175            1.23        3.02
##  8 AL         301 EDGE         194            3.34        3.34
##  9 AL         301 3            194            2.97        3.34
## 10 AL         301 2            194            2.29        3.34
## # ... with 373 more rows
```

## Select (select())

```r
fish %>% 
  select(lakeid, scalelength)
```

```
## # A tibble: 4,033 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## # ... with 4,023 more rows
```
Now combine pipe & select()

```r
fish %>%
  select(lakeid, scalelength)%>%
  filter(lakeid=="AL")%>%
  filter(scalelength >= 2 & scalelength <= 4)
```

```
## # A tibble: 11 x 2
##    lakeid scalelength
##    <chr>        <dbl>
##  1 AL            2.70
##  2 AL            2.70
##  3 AL            2.70
##  4 AL            3.02
##  5 AL            3.02
##  6 AL            3.02
##  7 AL            3.02
##  8 AL            3.34
##  9 AL            3.34
## 10 AL            3.34
## 11 AL            3.34
```
Helpful for big data!

## How  to store as independent dataframe
  Assign to new object

```r
fish_new <- fish %>%
  select(lakeid, scalelength)%>%
  filter(lakeid=="AL")%>%
  filter(scalelength >= 2 & scalelength <= 4)
```



## Recap

Should feel comfortable with 
  - creating .md file
  - filtering
  
Make sure to clean up notes, can post to github so you can have access online (esp if can't have own laptop)
  
Can now do HW#2








