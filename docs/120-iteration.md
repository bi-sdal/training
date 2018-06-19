# Iteration

## Broadcasting


```r
f_values <- c(0, 32, 212, -40)
```


```r
f_values * 10
```

```
## [1]    0  320 2120 -400
```


```r
f_values * c(10, 100)
```

```
## [1]     0  3200  2120 -4000
```

## For loops

Temp conversion functions


```r
f_k <- function(f_temp) {
    ((f_temp - 32) * (5 / 9)) + 273.15
}


k_c <- function(temp_k) {
    if (is.na(temp_k)) {
        return(NA)
    } else if (temp_k < 0) {
        warning('you passed in a negative Kelvin number')
        # stop()
        return(NA)
    } else {
        temp_c <- temp_k - 273.15
        return(temp_c)
    }
}

f_c <- function(temp_f) {
    temp_k <- f_k(temp_f)
    temp_c <- k_c(temp_k)
    return(temp_c)
}
```




```r
for (pizza in f_values) {
    print(pizza)
    converted <- f_c(pizza)
    print(converted)
}
```

```
## [1] 0
## [1] -17.77778
## [1] 32
## [1] 0
## [1] 212
## [1] 100
## [1] -40
## [1] -40
```


```r
# 1:length(f_values)
# seq_along(f_values)
for (i in seq_along(f_values)) {
    print(i)
    val <- f_values[i]
    print(val)
    
    converted <- f_c(val)
    print(converted)
}
```

```
## [1] 1
## [1] 0
## [1] -17.77778
## [1] 2
## [1] 32
## [1] 0
## [1] 3
## [1] 212
## [1] 100
## [1] 4
## [1] -40
## [1] -40
```

### Pre allocating in a loop


```r
# prepopulate an empty vector
converted_values <- vector("double", length(f_values))
for (to_be_converted_position in seq_along(f_values)) {
    converted <- f_c(to_be_converted_position)
    converted_values[to_be_converted_position] <- converted
}
```



```r
converted_values
```

```
## [1] -17.22222 -16.66667 -16.11111 -15.55556
```

## purrr (map)


```r
library(purrr)
```


```r
map(f_values, f_c)
```

```
## [[1]]
## [1] -17.77778
## 
## [[2]]
## [1] 0
## 
## [[3]]
## [1] 100
## 
## [[4]]
## [1] -40
```


```r
map_dbl(f_values, f_c)
```

```
## [1] -17.77778   0.00000 100.00000 -40.00000
```


```r
mtcars
```

```
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```


```r
map(mtcars, class)
```

```
## $mpg
## [1] "numeric"
## 
## $cyl
## [1] "numeric"
## 
## $disp
## [1] "numeric"
## 
## $hp
## [1] "numeric"
## 
## $drat
## [1] "numeric"
## 
## $wt
## [1] "numeric"
## 
## $qsec
## [1] "numeric"
## 
## $vs
## [1] "numeric"
## 
## $am
## [1] "numeric"
## 
## $gear
## [1] "numeric"
## 
## $carb
## [1] "numeric"
```


```r
map_chr(mtcars, class)
```

```
##       mpg       cyl      disp        hp      drat        wt      qsec 
## "numeric" "numeric" "numeric" "numeric" "numeric" "numeric" "numeric" 
##        vs        am      gear      carb 
## "numeric" "numeric" "numeric" "numeric"
```



```r
map_dbl(mtcars, mean)
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500   0.406250   3.687500   2.812500
```


```r
map(mtcars, summary)
```

```
## $mpg
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   10.40   15.43   19.20   20.09   22.80   33.90 
## 
## $cyl
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   4.000   4.000   6.000   6.188   8.000   8.000 
## 
## $disp
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    71.1   120.8   196.3   230.7   326.0   472.0 
## 
## $hp
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    52.0    96.5   123.0   146.7   180.0   335.0 
## 
## $drat
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   2.760   3.080   3.695   3.597   3.920   4.930 
## 
## $wt
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.513   2.581   3.325   3.217   3.610   5.424 
## 
## $qsec
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   14.50   16.89   17.71   17.85   18.90   22.90 
## 
## $vs
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.0000  0.0000  0.0000  0.4375  1.0000  1.0000 
## 
## $am
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.0000  0.0000  0.0000  0.4062  1.0000  1.0000 
## 
## $gear
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   3.000   3.000   4.000   3.688   4.000   5.000 
## 
## $carb
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.000   2.000   2.000   2.812   4.000   8.000
```

## Apply (in base R)

apply family of functions


### lapply


```r
lapply(f_values, f_c)
```

```
## [[1]]
## [1] -17.77778
## 
## [[2]]
## [1] 0
## 
## [[3]]
## [1] 100
## 
## [[4]]
## [1] -40
```

### sapply


```r
sapply(f_values, f_c)
```

```
## [1] -17.77778   0.00000 100.00000 -40.00000
```


```r
v1 <- c(1, 2, 3, 4)
v2 <- c(100, 200, 300, 400)
```

### mapply


```r
my_mean <- function(x, y){
    return((x + y) / 2)
}
```


```r
# sapply(v1, v2, my_mean)
```


```r
mapply(my_mean, v1, v2)
```

```
## [1]  50.5 101.0 151.5 202.0
```

```r
# this is the same as purrr::map2
```

### apply (2-dimensions)


```r
apply(mtcars, MARGIN = 1, mean)
```

```
##           Mazda RX4       Mazda RX4 Wag          Datsun 710 
##            29.90727            29.98136            23.59818 
##      Hornet 4 Drive   Hornet Sportabout             Valiant 
##            38.73955            53.66455            35.04909 
##          Duster 360           Merc 240D            Merc 230 
##            59.72000            24.63455            27.23364 
##            Merc 280           Merc 280C          Merc 450SE 
##            31.86000            31.78727            46.43091 
##          Merc 450SL         Merc 450SLC  Cadillac Fleetwood 
##            46.50000            46.35000            66.23273 
## Lincoln Continental   Chrysler Imperial            Fiat 128 
##            66.05855            65.97227            19.44091 
##         Honda Civic      Toyota Corolla       Toyota Corona 
##            17.74227            18.81409            24.88864 
##    Dodge Challenger         AMC Javelin          Camaro Z28 
##            47.24091            46.00773            58.75273 
##    Pontiac Firebird           Fiat X1-9       Porsche 914-2 
##            57.37955            18.92864            24.77909 
##        Lotus Europa      Ford Pantera L        Ferrari Dino 
##            24.88027            60.97182            34.50818 
##       Maserati Bora          Volvo 142E 
##            63.15545            26.26273
```


```r
apply(mtcars, MARGIN = 2, mean)
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500   0.406250   3.687500   2.812500
```




