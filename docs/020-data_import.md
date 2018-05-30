# Importing Data

DataCamp Courses:

- https://www.datacamp.com/courses/importing-data-in-r-part-1
- https://www.datacamp.com/courses/importing-data-in-r-part-2

## Loading datasets


```r
library(readr)
```

look at `read_` set of functions



```r
heights = read_csv('data/heights.csv')
```

```
## Parsed with column specification:
## cols(
##   earn = col_double(),
##   height = col_double(),
##   sex = col_character(),
##   ed = col_integer(),
##   age = col_integer(),
##   race = col_character()
## )
```

- skip number of rows
- comments can be skipped
- col_names for no column names
    - manually pass in column names
- na specifies what is "missing"


in base R, `read_csv`, `data.table::fread()`

string as factors! useing `readr` compared to base R

## Writing to a files

readr has a `write_csv` and `write_tsv` function

Use write_rds and read_rds (similar to readRDS and saveRDS)

`feather` can be used to share data between languages (e.g., Python)

- haven: SPSS, Stata, and SAS files
- readxl: excel files (xls, xlsx)
- DBI + backend (RMySQL, RSQLite, RPostgreSQL, etc) to connect to databases
- jsonlite
- xml2
- rio
