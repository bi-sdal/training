
# Functions

## Writing Functions

###  Fahrenheit to Kelvin

$k = ((f - 32) * (5 / 9)) + 273.15$


```r
((32 - 32) * (5 / 9)) + 273.15
## [1] 273.15
((212 - 32) * (5 / 9)) + 273.15
## [1] 373.15
((-42 - 32) * (5 / 9)) + 273.15
## [1] 232.0389
```


```r
f_k <- function(f_temp) {
    ((f_temp - 32) * (5 / 9)) + 273.15
}
```


```r
f_k(32)
## [1] 273.15
f_k(212)
## [1] 373.15
f_k(-42)
## [1] 232.0389
```

### Kelvin to Celsius


```r
k_c <- function(temp_k) {
    temp_c <- temp_k - 273.15
    return(temp_c)
}
```


```r
k_c(0)
## [1] -273.15
```

### Fahrenheit to Celsius


```r
f_c <- function(temp_f) {
    temp_k <- f_k(temp_f)
    temp_c <- k_c(temp_k)
    return(temp_c)
}
```


```r
f_c(32)
## [1] 0
f_c(212)
## [1] 100
```
