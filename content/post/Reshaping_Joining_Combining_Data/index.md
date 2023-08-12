---
title: "Reshaping, Joining, Combine data"
date: 2023-07-31T06:13:28+07:00
author: admin
description: fungsi reshaping, joining, combine data dengan r
---

## Reshaping and Combining data

### Transpose Matrix

``` r
first <- matrix(c(1:12), nrow=4, byrow=TRUE)
print("Original Matrix")
```

    ## [1] "Original Matrix"

``` r
first
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    3
    ## [2,]    4    5    6
    ## [3,]    7    8    9
    ## [4,]   10   11   12

``` r
first <- t(first)
print("Transpose of the Matrix")
```

    ## [1] "Transpose of the Matrix"

``` r
first
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    4    7   10
    ## [2,]    2    5    8   11
    ## [3,]    3    6    9   12

### Joining baris dan kolom

``` r
name <- c("Shaoni", "esha", "soumitra", "soumi")
age <- c(24, 53, 62, 29)
address <- c("puducherry", "kolkata", "delhi", "bangalore")
 
# Cbind function
info <- cbind(name, age, address)
print("Combining vectors into data frame using cbind ")
```

``` r
print(info)
```

    ##      name       age  address     
    ## [1,] "Shaoni"   "24" "puducherry"
    ## [2,] "esha"     "53" "kolkata"   
    ## [3,] "soumitra" "62" "delhi"     
    ## [4,] "soumi"    "29" "bangalore"

``` r
newd <- data.frame(name=c("sounak", "bhabani"),
                   age=c("28", "87"),
                   address=c("bangalore", "kolkata"))
 
# Rbind function
new.info <- rbind(info, newd)
print("Combining data frames using rbind ")
```

    ## [1] "Combining data frames using rbind "

``` r
print(new.info)
```

    ##       name age    address
    ## 1   Shaoni  24 puducherry
    ## 2     esha  53    kolkata
    ## 3 soumitra  62      delhi
    ## 4    soumi  29  bangalore
    ## 5   sounak  28  bangalore
    ## 6  bhabani  87    kolkata

### Melting Casting

``` r
# Mengimpor paket reshape2
library(reshape2)

# Membuat dataframe contoh
data <- data.frame(ID = c(1, 2, 3),
                   Name = c("Alice", "Bob", "Charlie"),
                   Math = c(85, 90, 78),
                   Science = c(75, 88, 92))

# Melting data
melted_data <- melt(data, id.vars = c("ID", "Name"), 
                    measure.vars = c("Math", "Science"))

# Menampilkan hasil melting
print(melted_data)
```

    ##   ID    Name variable value
    ## 1  1   Alice     Math    85
    ## 2  2     Bob     Math    90
    ## 3  3 Charlie     Math    78
    ## 4  1   Alice  Science    75
    ## 5  2     Bob  Science    88
    ## 6  3 Charlie  Science    92

``` r
# Membuat dataframe contoh
melted_data <- data.frame(ID = c(1, 1, 2, 2, 3, 3),
                          Variable = c("Math", "Science", "Math", "Science", "Math", "Science"),
                          Value = c(85, 75, 90, 88, 78, 92))

# Casting data
casted_data <- dcast(melted_data, ID ~ Variable, value.var = "Value")

# Menampilkan hasil casting
print(casted_data)
```

    ##   ID Math Science
    ## 1  1   85      75
    ## 2  2   90      88
    ## 3  3   78      92
