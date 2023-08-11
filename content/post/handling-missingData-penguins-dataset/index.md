---
title: "Handling_MissingData_Penguins_Dataset"
date: 2023-08-02T03:58:35+07:00
author: admin
description: Handling missing data value in penguins dataset
tags: 
  - Wrangling
---

## mengatasi nilai yang hilang dalam dataset penguins

### Import Data and call package

``` r
library(tidyverse)
library(dplyr)
library(palmerpenguins)
data("penguins")
dim(penguins)
```

    ## [1] 344   8

``` r
glimpse(penguins)
```

    ## Rows: 344
    ## Columns: 8
    ## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
    ## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
    ## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
    ## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
    ## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
    ## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
    ## $ sex               <fct> male, female, female, NA, female, male, female, male…
    ## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…

### cek missing value

``` r
which(is.na(penguins))
```

    ##  [1]  692  960 1036 1304 1380 1648 1724 1992 2068 2073 2074 2075 2076 2112 2243
    ## [16] 2283 2321 2333 2336

``` r
sum(is.na(penguins))
```

    ## [1] 19

terdapat 19 nilai NA

``` r
sum(is.nan(penguins$bill_length_mm))
```

    ## [1] 0

``` r
sum(is.nan(penguins$bill_depth_mm))
```

    ## [1] 0

``` r
sum(is.nan(penguins$flipper_length_mm))
```

    ## [1] 0

``` r
sum(is.nan(penguins$body_mass_g))
```

    ## [1] 0

``` r
sum(is.nan(penguins$year))
```

    ## [1] 0

### Handling With remove missing data

``` r
penguins_data1 <- penguins
penguins_data1 <- na.omit(penguins_data1)
```

[***Dataset***](https://github.com/muhammadrendysa/handling_missing_value/blob/main/penguins_data1.xlsx)

### Filling Missing Values with Mean or Median

``` r
penguins_data2 <- penguins

listMissingColumns <- colnames(penguins_data2)[apply(penguins_data2, 2, anyNA)]
listMissingColumns
```

    ## [1] "bill_length_mm"    "bill_depth_mm"     "flipper_length_mm"
    ## [4] "body_mass_g"       "sex"

``` r
penguins_data2$bill_length_mm <- as.numeric(penguins_data2$bill_length_mm)
penguins_data2$bill_depth_mm <- as.numeric(penguins_data2$bill_depth_mm)
penguins_data2$flipper_length_mm <- as.numeric(penguins_data2$flipper_length_mm)
penguins_data2$body_mass_g <- as.numeric(penguins_data2$body_mass_g)
penguins_data2$sex <- as.numeric(penguins_data2$sex)
```

``` r
glimpse(penguins_data2)
```

    ## Rows: 344
    ## Columns: 8
    ## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
    ## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
    ## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
    ## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
    ## $ flipper_length_mm <dbl> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
    ## $ body_mass_g       <dbl> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
    ## $ sex               <dbl> 2, 1, 1, NA, 1, 2, 1, 2, NA, NA, NA, NA, 1, 2, 2, 1,…
    ## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…

``` r
meanMissing <- apply(penguins_data2[, colnames(penguins_data2) %in% listMissingColumns], 2, mean, na.rm = TRUE)
meanMissing
```

    ##    bill_length_mm     bill_depth_mm flipper_length_mm       body_mass_g 
    ##         43.921930         17.151170        200.915205       4201.754386 
    ##               sex 
    ##          1.504505

``` r
penguins_data2 <- penguins_data2 %>%
  mutate(
    bill_length_mm = ifelse(is.na(bill_length_mm), meanMissing[1], bill_length_mm),
    bill_depth_mm = ifelse(is.na(bill_depth_mm), meanMissing[2], bill_depth_mm),
    flipper_length_mm = ifelse(is.na(flipper_length_mm), meanMissing[3], flipper_length_mm),
    body_mass_g = ifelse(is.na(body_mass_g), meanMissing[4], body_mass_g),
    sex = ifelse(is.na(sex), meanMissing[5], sex)
  )

colnames(penguins_data2)[apply(penguins_data2, 2, anyNA)]
```

    ## character(0)

[***dataset***](https://github.com/muhammadrendysa/handling_missing_value/blob/main/penguins_data2.xlsx)
