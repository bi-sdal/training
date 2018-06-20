
# Conditionals

## if statements


```r
# make a modification to this function
k_c <- function(temp_k) {
    if (temp_k < 0) {
        warning('you passed in a negative Kelvin number')
        # stop()
        return(NA)
    }
    temp_c <- temp_k - 273.15
    return(temp_c)
}
```


```r
k_c(-9)
## Warning in k_c(-9): you passed in a negative Kelvin number
## [1] NA
```

Our current function does not deal with missing numbers

```r
k_c(NA)
```

```
Error in if (temp_k < 0) { : missing value where TRUE/FALSE needed
```


```r
k_c(0)
## [1] -273.15
```


## If else statements


```r

k_c <- function(temp_k) {
    if (temp_k < 0) {
        warning('you passed in a negative Kelvin number')
        # stop()
        return(NA)
    } else {
        temp_c <- temp_k - 273.15
        return(temp_c)
    }
}
```


```r
k_c(-9)
## Warning in k_c(-9): you passed in a negative Kelvin number
## [1] NA
```

Our current function does not deal with missing numbers

```r
k_c(NA)
```


```r
k_c(0)
## [1] -273.15
```

## Dealing with NA

Re-write our function to work with missing values.

Note you need to make the `NA` check first.


```r
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
```


```r
k_c(-9)
## Warning in k_c(-9): you passed in a negative Kelvin number
## [1] NA
```


```r
k_c(NA)
## [1] NA
```


```r
k_c(0)
## [1] -273.15
```
