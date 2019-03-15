---
title: "Midterm Exam Revision"
author: "April Truong"
date: "Winter 2019"
output:
  html_document:
    keep_md: yes
    theme: spacelab
    toc_float: no
  pdf_document:
    toc: yes
---

## Instructions
This exam is designed to show me what you have learned and where there are problems. You may use your notes and anything from the `class_files` folder, but please no internet searches. You have 35 minutes to complete as many of these exercises as possible on your own, and 10 minutes to work with a partner.  

At the end of the exam, upload the complete .Rmd file to your GitHub repository.  

1. Load the tidyverse.

```r
library(tidyverse)
```


2. For these questions, we will use data about California colleges. Load the `ca_college_data.csv` as a new object called `colleges`.

```r
colleges <- 
  readr::read_csv("C:/Users/Apple/Desktop/aptruong/data/ca_college_data.csv")
```

```
## Parsed with column specification:
## cols(
##   INSTNM = col_character(),
##   CITY = col_character(),
##   STABBR = col_character(),
##   ZIP = col_character(),
##   ADM_RATE = col_double(),
##   SAT_AVG = col_double(),
##   PCIP26 = col_double(),
##   COSTT4_A = col_double(),
##   C150_4_POOLED = col_double(),
##   PFTFTUG1_EF = col_double()
## )
```


3. Use your preferred function to have a look at the data and get an idea of its structure.

```r
#install.packages("skimr")
library("skimr")
colleges %>% 
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 341 
##  n variables: 10 
## 
## -- Variable type:character ---------------------
##  variable missing complete   n min max empty n_unique
##      CITY       0      341 341   4  19     0      161
##    INSTNM       0      341 341  10  63     0      341
##    STABBR       0      341 341   2   2     0        3
##       ZIP       0      341 341   5  10     0      324
## 
## -- Variable type:numeric -----------------------
##       variable missing complete   n     mean        sd        p0      p25
##       ADM_RATE     240      101 341     0.59     0.23     0.081      0.46
##  C150_4_POOLED     221      120 341     0.57     0.21     0.062      0.43
##       COSTT4_A     124      217 341 26685.17 18122.7   7956      12578   
##         PCIP26      35      306 341     0.02     0.038    0          0   
##    PFTFTUG1_EF      53      288 341     0.56     0.29     0.0064     0.32
##        SAT_AVG     276       65 341  1112.31   170.8    870        985   
##       p50       p75     p100     hist
##      0.64     0.75      1    <U+2583><U+2582><U+2585><U+2587><U+2586><U+2587><U+2585><U+2583>
##      0.58     0.72      0.96 <U+2581><U+2583><U+2583><U+2586><U+2587><U+2587><U+2583><U+2585>
##  16591    39289     69355    <U+2587><U+2583><U+2581><U+2582><U+2581><U+2581><U+2581><U+2581>
##      0        0.025     0.22 <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##      0.5      0.81      1    <U+2581><U+2585><U+2587><U+2586><U+2583><U+2585><U+2583><U+2587>
##   1078     1237      1555    <U+2586><U+2587><U+2585><U+2583><U+2583><U+2582><U+2582><U+2581>
```

```r
summary(colleges)
```

```
##     INSTNM              CITY              STABBR         
##  Length:341         Length:341         Length:341        
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##      ZIP               ADM_RATE         SAT_AVG         PCIP26       
##  Length:341         Min.   :0.0807   Min.   : 870   Min.   :0.00000  
##  Class :character   1st Qu.:0.4581   1st Qu.: 985   1st Qu.:0.00000  
##  Mode  :character   Median :0.6370   Median :1078   Median :0.00000  
##                     Mean   :0.5901   Mean   :1112   Mean   :0.01981  
##                     3rd Qu.:0.7461   3rd Qu.:1237   3rd Qu.:0.02458  
##                     Max.   :1.0000   Max.   :1555   Max.   :0.21650  
##                     NA's   :240      NA's   :276    NA's   :35       
##     COSTT4_A     C150_4_POOLED     PFTFTUG1_EF    
##  Min.   : 7956   Min.   :0.0625   Min.   :0.0064  
##  1st Qu.:12578   1st Qu.:0.4265   1st Qu.:0.3212  
##  Median :16591   Median :0.5845   Median :0.5016  
##  Mean   :26685   Mean   :0.5705   Mean   :0.5577  
##  3rd Qu.:39289   3rd Qu.:0.7162   3rd Qu.:0.8117  
##  Max.   :69355   Max.   :0.9569   Max.   :1.0000  
##  NA's   :124     NA's   :221      NA's   :53
```


4. What are the column names?

```r
colnames(colleges)
```

```
##  [1] "INSTNM"        "CITY"          "STABBR"        "ZIP"          
##  [5] "ADM_RATE"      "SAT_AVG"       "PCIP26"        "COSTT4_A"     
##  [9] "C150_4_POOLED" "PFTFTUG1_EF"
```


5. Are there any NA's in the data? If so, how many are present and in which variables?

```r
colleges %>% 
  summarize(number_nas= sum(is.na(colleges))) 
```

```
## # A tibble: 1 x 1
##   number_nas
##        <int>
## 1        949
```
> Total of 949 NA's


```r
colleges %>% 
  purrr::map_df(~ sum(is.na(.))) %>%
  gather(variable, value = "number_nas") %>%
  arrange(desc(number_nas))
```

```
## # A tibble: 10 x 2
##    variable      number_nas
##    <chr>              <int>
##  1 SAT_AVG              276
##  2 ADM_RATE             240
##  3 C150_4_POOLED        221
##  4 COSTT4_A             124
##  5 PFTFTUG1_EF           53
##  6 PCIP26                35
##  7 INSTNM                 0
##  8 CITY                   0
##  9 STABBR                 0
## 10 ZIP                    0
```
> Variables w/ NA's: SAT_AVG, ADM_RATE, C150_4_POOLED, COSTT4_A, PFTFTUG1_EF, PCIP26


6. Which cities in California have the highest number of colleges?

```r
colleges %>% 
  count(CITY) %>% 
  arrange(desc(n))
```

```
## # A tibble: 161 x 2
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
## # ... with 151 more rows
```
> Los Angeles has highest number of colleges

7. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest cost?

```r
# My original code
# colleges %>%
#   select(CITY, COSTT4_A) %>% 
#   arrange(COSTT4_A)
#
# Solution
colleges %>% 
  group_by(CITY) %>% 
  summarize(mean_cost_yr=mean(COSTT4_A, na.rm=TRUE),
            total=n()) %>% 
  arrange(desc(mean_cost_yr))
```

```
## # A tibble: 161 x 3
##    CITY                mean_cost_yr total
##    <chr>                      <dbl> <int>
##  1 Claremont                  66498     7
##  2 Malibu                     66152     1
##  3 Valencia                   64686     1
##  4 Orange                     64501     3
##  5 Redlands                   61542     1
##  6 Moraga                     61095     1
##  7 Atherton                   56035     1
##  8 Thousand Oaks              54373     1
##  9 Rancho Palos Verdes        50758     1
## 10 La Verne                   50603     1
## # ... with 151 more rows
```
> Claremont has the highest annual cost ($66498).

8. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What does this mean?

```r
ggplot(data=colleges, mapping=aes(x=ADM_RATE, y=C150_4_POOLED)) +
  geom_point()+
  geom_smooth(method=lm, se=TRUE)+  #added code from midterm key
  labs(title = "Admission rate by college vs. 4-year completion rate",
       x = "Admission rate by college",
       y = "4-year completion rate") 
```

```
## Warning: Removed 251 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 251 rows containing missing values (geom_point).
```

![](midterm_exam_revised_aptruong_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
> This means that there is a correlation between high admission rates and low 4-year completion

9. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Run the code below and look at the output. Are all of the columns tidy? Why or why not?

```r
univ_calif<-
  colleges %>% 
  filter_all(any_vars(str_detect(., pattern = "University of California")))
univ_calif
```

```
## # A tibble: 10 x 10
##    INSTNM CITY  STABBR ZIP   ADM_RATE SAT_AVG PCIP26 COSTT4_A C150_4_POOLED
##    <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>         <dbl>
##  1 Unive~ La J~ CA     92093    0.357    1324  0.216    31043         0.872
##  2 Unive~ Irvi~ CA     92697    0.406    1206  0.107    31198         0.876
##  3 Unive~ Rive~ CA     92521    0.663    1078  0.149    31494         0.73 
##  4 Unive~ Los ~ CA     9009~    0.180    1334  0.155    33078         0.911
##  5 Unive~ Davis CA     9561~    0.423    1218  0.198    33904         0.850
##  6 Unive~ Sant~ CA     9506~    0.578    1201  0.193    34608         0.776
##  7 Unive~ Berk~ CA     94720    0.169    1422  0.105    34924         0.916
##  8 Unive~ Sant~ CA     93106    0.358    1281  0.108    34998         0.816
##  9 Unive~ San ~ CA     9410~   NA          NA NA           NA        NA    
## 10 Unive~ San ~ CA     9414~   NA          NA NA           NA        NA    
## # ... with 1 more variable: PFTFTUG1_EF <dbl>
```
> Not tidy; institution name combines university and campus

10. Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```r
# My original code
# separate(colleges, INSTNM, c("UNIV", "CAMPUS"), sep=",")

# Midterm Key code
univ_calif <- 
  univ_calif %>% 
  separate(INSTNM, c("UNIV", "CAMPUS"), sep="-")
univ_calif
```

```
## # A tibble: 10 x 11
##    UNIV  CAMPUS CITY  STABBR ZIP   ADM_RATE SAT_AVG PCIP26 COSTT4_A
##    <chr> <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>
##  1 Univ~ San D~ La J~ CA     92093    0.357    1324  0.216    31043
##  2 Univ~ Irvine Irvi~ CA     92697    0.406    1206  0.107    31198
##  3 Univ~ River~ Rive~ CA     92521    0.663    1078  0.149    31494
##  4 Univ~ Los A~ Los ~ CA     9009~    0.180    1334  0.155    33078
##  5 Univ~ Davis  Davis CA     9561~    0.423    1218  0.198    33904
##  6 Univ~ Santa~ Sant~ CA     9506~    0.578    1201  0.193    34608
##  7 Univ~ Berke~ Berk~ CA     94720    0.169    1422  0.105    34924
##  8 Univ~ Santa~ Sant~ CA     93106    0.358    1281  0.108    34998
##  9 Univ~ Hasti~ San ~ CA     9410~   NA          NA NA           NA
## 10 Univ~ San F~ San ~ CA     9414~   NA          NA NA           NA
## # ... with 2 more variables: C150_4_POOLED <dbl>, PFTFTUG1_EF <dbl>
```


11. As a final step, remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

```r
# My original code
# gather(univ_calif, "INSTNM", "Hastings COllege of Law, UC San Francisco", univ_calif_final)
#
# Midterm Key Code
univ_calif_final <- 
  univ_calif %>% 
  filter(CAMPUS !="Hastings College of Law",
         CAMPUS !="San Francisco")
univ_calif_final
```

```
## # A tibble: 8 x 11
##   UNIV  CAMPUS CITY  STABBR ZIP   ADM_RATE SAT_AVG PCIP26 COSTT4_A
##   <chr> <chr>  <chr> <chr>  <chr>    <dbl>   <dbl>  <dbl>    <dbl>
## 1 Univ~ San D~ La J~ CA     92093    0.357    1324  0.216    31043
## 2 Univ~ Irvine Irvi~ CA     92697    0.406    1206  0.107    31198
## 3 Univ~ River~ Rive~ CA     92521    0.663    1078  0.149    31494
## 4 Univ~ Los A~ Los ~ CA     9009~    0.180    1334  0.155    33078
## 5 Univ~ Davis  Davis CA     9561~    0.423    1218  0.198    33904
## 6 Univ~ Santa~ Sant~ CA     9506~    0.578    1201  0.193    34608
## 7 Univ~ Berke~ Berk~ CA     94720    0.169    1422  0.105    34924
## 8 Univ~ Santa~ Sant~ CA     93106    0.358    1281  0.108    34998
## # ... with 2 more variables: C150_4_POOLED <dbl>, PFTFTUG1_EF <dbl>
```


12. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Please use a barplot.

```r
# My original code
# ggplot(data=univ_calif, aes(x=reorder(INSTNM, -ADM_RATE), (y = ADM_RATE)))+ 
#   geom_bar(stat = "identity")+
#   labs(title = "Admission rate by campus",
#        x = "Admision rate",
#        y = "Campus")+ 
#   theme(plot.title = element_text(size = rel(2), hjust = 0.5))+
#    coord_flip() #just to flip

# Revision
univ_calif_final %>% 
  ggplot(aes(x=CAMPUS, y=ADM_RATE))+
  geom_bar(stat="identity")+
  labs(title = "Admission rate by campus",
       x = "Admision rate",
       y = "Campus")+
  theme(plot.title = element_text(size = rel(2), hjust = 0.5))+
   coord_flip() #just to flip
```

![](midterm_exam_revised_aptruong_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
