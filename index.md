## Manage Data Frames
Data frames in R are used to store data tables. A data frame is a list of vectors of the same length, each vector can have a different type, i.e. integer, factor, character, etc. 

We use R package 'dplyr' and built-in dataset 'mtcars' to illustrate how to read, manipulate and store data frames. In practice, one can perform all the operations below using built-in functions in R. The reason for chosing 'dplyr' is discussed at the end of this page.

### Import libraries

```
> library("dplyr")
```

### Print first few rows

```
> head(mtcars)
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```

### See dimension of dataset

```
> dim(mtcars)
[1] 32 11
```

### See basic characteristics (including dimension)

```
> str(mtcars)
'data.frame':	32 obs. of  11 variables:
 $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
 $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
 $ disp: num  160 160 108 258 360 ...
 $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
 $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
 $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
 $ qsec: num  16.5 17 18.6 19.4 17 ...
 $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
 $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
 $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
 $ carb: num  4 4 1 1 2 1 4 2 2 4 ...
```
### See basic statistics of a variable

```
> summary(mtcars$mpg)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  10.40   15.42   19.20   20.09   22.80   33.90 
```

### Select columns

```
> subset <- select(mtcars, cyl:drat)
> head(subset)
                  cyl disp  hp drat
Mazda RX4           6  160 110 3.90
Mazda RX4 Wag       6  160 110 3.90
Datsun 710          4  108  93 3.85
Hornet 4 Drive      6  258 110 3.08
Hornet Sportabout   8  360 175 3.15
Valiant             6  225 105 2.76

> subset <- select(mtcars, ends_with("p"))
> head(subset)
                  disp  hp
Mazda RX4          160 110
Mazda RX4 Wag      160 110
Datsun 710         108  93
Hornet 4 Drive     258 110
Hornet Sportabout  360 175
Valiant            225 105
```

### Select rows

```
> subset <- filter(mtcars, hp > 100 & mpg > 20)
> str(subset)
'data.frame':	5 obs. of  11 variables:
 $ mpg : num  21 21 21.4 30.4 21.4
 $ cyl : num  6 6 6 4 4
 $ disp: num  160 160 258 95.1 121
 $ hp  : num  110 110 110 113 109
 $ drat: num  3.9 3.9 3.08 3.77 4.11
 $ wt  : num  2.62 2.88 3.21 1.51 2.78
 $ qsec: num  16.5 17 19.4 16.9 18.6
 $ vs  : num  0 0 1 1 1
 $ am  : num  1 1 0 1 1
 $ gear: num  4 4 3 5 4
 $ carb: num  4 4 1 2 2
```

### Reorder rows by condition

```
> mtcars.order <- arrange(mtcars, desc(mpg))
> tail(mtcars.order)
    mpg cyl disp  hp drat    wt  qsec vs am gear carb
27 15.0   8  301 335 3.54 3.570 14.60  0  1    5    8
28 14.7   8  440 230 3.23 5.345 17.42  0  0    3    4
29 14.3   8  360 245 3.21 3.570 15.84  0  0    3    4
30 13.3   8  350 245 3.73 3.840 15.41  0  0    3    4
31 10.4   8  472 205 2.93 5.250 17.98  0  0    3    4
32 10.4   8  460 215 3.00 5.424 17.82  0  0    3    4
> min(mtcars$mpg)
[1] 10.4
```

### Rename columns

```
> mtcars.rename <- rename(mtcars, HP = hp, WT = wt)
> head(mtcars.rename)
                   mpg cyl disp  HP drat    WT  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```

### Transform selected variables in a data frame
```
> mtcars.mutate <- mutate(mtcars, qsec = qsec - mean(qsec) / sd(qsec))
> head(mtcars.mutate)
   mpg cyl disp  hp drat    wt      qsec vs am gear carb
1 21.0   6  160 110 3.90 2.620  6.471574  0  1    4    4
2 21.0   6  160 110 3.90 2.875  7.031574  0  1    4    4
3 22.8   4  108  93 3.85 2.320  8.621574  1  1    4    1
4 21.4   6  258 110 3.08 3.215  9.451574  1  0    3    1
5 18.7   8  360 175 3.15 3.440  7.031574  0  0    3    2
6 18.1   6  225 105 2.76 3.460 10.231574  1  0    3    1
```

### Group data

```
> by_cyl <- group_by(mtcars, cyl)
> summarise(by_cyl, mean(disp), mean(hp))
# A tibble: 3 x 3
    cyl mean(disp)  mean(hp)
  <dbl>      <dbl>     <dbl>
1     4   105.1364  82.63636
2     6   183.3143 122.28571
3     8   353.1000 209.21429
```
### Pipe operator %>%

Pipe operators are used to perform composition of functions. For example, x %>% g %>% f is equivalent to f(g(x)). Equivalent to the previous example, we can do:

```
> by_cyl <- group_by(mtcars, cyl) %>% summarize(mean(disp), mean(hp))
> by_cyl
# A tibble: 3 x 3
    cyl mean(disp)  mean(hp)
  <dbl>      <dbl>     <dbl>
1     4   105.1364  82.63636
2     6   183.3143 122.28571
3     8   353.1000 209.21429
```

## Why dplyr?
