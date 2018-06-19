
<!--
Message to anyone editing this page
Note that most of the chunks are not being evaluated,
you need to pass in eval=TRUE into the chunk to evaluate the code chunk
-->
# SQL Databases

R4ds chapter (relational data): http://r4ds.had.co.nz/relational-data.html

Software-Carpentry SQL Lesson

- http://swcarpentry.github.io/sql-novice-survey/

DataCamp Courses

- https://www.datacamp.com/courses/intro-to-sql-for-data-science
- https://www.datacamp.com/courses/joining-data-in-postgresql

## Databases in R





```r
rap_chart_artists <- readxl::read_excel("data/Rap_Charts.xlsx", sheet = "Artists")
rap_chart_singles <- readxl::read_excel("data/Rap_Charts.xlsx", sheet = "Singles")
```


```r
head(rap_chart_artists)
## # A tibble: 4 x 4
##   Artist_ID First_Name Last_Name Psuedonym  
##       <dbl> <chr>      <chr>     <chr>      
## 1         4 O'Shea     Jackson   Ice Cube   
## 2         3 Marvin     Young     Young M.C. 
## 3         2 Stanley    Burrell   M.C. Hammer
## 4         1 James      Smith     LL Cool J
```


```r
head(rap_chart_singles)
## # A tibble: 6 x 5
##   Artist_ID  Year Single                     Top_US_Rap_Chart Album       
##       <dbl> <dbl> <chr>                                 <dbl> <chr>       
## 1         1  1984 "\"I Need a Beat\""                      NA Non-album s…
## 2         1  1985 "\"I Can't Live Without M…               NA Radio       
## 3         1  1986 "\"Rock the Bells\""                     NA Radio       
## 4         1  1986 "\"I Can Give You More\""                NA Radio       
## 5         1  1986 "\"You'll Rock\""                        NA Radio       
## 6         1  1987 "\"I'm Bad\""                            NA Bigger and …
```

### CREATE REUSABLE CONNECTION TO DATABASE


```r
# by default this will open up your own database (e.g., aschroed)
my_db_con <- sdalr::con_db(pass = sdalr::get_my_password())
```

### WRITE DATA TO DATABASE TABLES

```r
DBI::dbWriteTable(my_db_con, "rap_chart_artists", rap_chart_artists, row.names = FALSE)
DBI::dbWriteTable(my_db_con, "rap_chart_singles", rap_chart_singles, row.names = FALSE)
```

## Joins


### JOIN TABLES IN SQL WITH DBI

```r
DBI::dbGetQuery(my_db_con, 
                "SELECT * 
                 FROM rap_chart_artists a 
                 JOIN rap_chart_singles b ON a.\"Artist_ID\" = b.\"Artist_ID\"")
```

### JOIN TABLES IN SQL IN SQL CHUNK

<pre><code>```{sql connection=my_db_con}
SELECT *
FROM rap_chart_artists a
JOIN rap_chart_singles s ON a."Artist_ID" = s."Artist_ID"
```</code></pre>

### JOIN TABLES IN SQL IN SQL CHUNK WITH OUTPUT VARIABLE

<pre><code>```{sql connection=my_db_con, output.var="rap_artist_singles"}
SELECT *
FROM rap_chart_artists a
JOIN rap_chart_singles s ON a."Artist_ID" = s."Artist_ID"
```</code></pre>


```r
rap_artist_singles[, c("First_Name", "Last_Name")]
```

### JOIN TABLES IN SQL IN SQL CHUNK APPLY WHERE CLAUSE AND FUZZY SEARCH

<pre><code>```{sql connection=my_db_con}
SELECT *
FROM rap_chart_artists a
JOIN rap_chart_singles s ON a."Artist_ID" = s."Artist_ID"
WHERE "Psuedonym" LIKE '%LL%'
```</code></pre>

### JOIN TABLES IN SQL IN SQL CHUNK APPLY WHERE CLAUSE WITH 'AND' AND FUZZY SEARCH

<pre><code>```{sql connection=my_db_con}
SELECT *
FROM rap_chart_artists a
JOIN rap_chart_singles s ON a."Artist_ID" = s."Artist_ID"
WHERE "Psuedonym" LIKE '%LL%'
AND "Top_US_Rap_Chart" IS NOT NULL
```</code></pre>

### JOIN TABLES IN SQL IN SQL CHUNK APPLY SELECT AND WHERE CLAUSE WITH 'AND' AND FUZZY SEARCH AND ORDER

<pre><code>```{sql connection=my_db_con}
SELECT "Psuedonym", "Single", "Year", "Top_US_Rap_Chart"
FROM rap_chart_artists a
JOIN rap_chart_singles s ON a."Artist_ID" = s."Artist_ID"
WHERE "Psuedonym" LIKE '%LL%'
AND "Top_US_Rap_Chart" IS NOT NULL
ORDER BY "Top_US_Rap_Chart", "Year"
```</code></pre>

### JOIN TABLES IN SQL IN SQL CHUNK APPLY SELECT AND WHERE CLAUSE AND FUZZY SEARCH AND ORDER AND GROUP BY WITH AGGREGATE FUNCTION

<pre><code>```{sql connection=my_db_con, max.print = 15}
SELECT "Psuedonym", "Top_US_Rap_Chart", COUNT("Top_US_Rap_Chart") chart_position_cnt
FROM rap_chart_artists a
JOIN rap_chart_singles s ON a."Artist_ID" = s."Artist_ID"
WHERE "Psuedonym" LIKE '%M.C.%'
AND "Top_US_Rap_Chart" IS NOT NULL
GROUP BY "Psuedonym", "Top_US_Rap_Chart"
ORDER BY "Psuedonym", "Top_US_Rap_Chart"
```</code></pre>


## dplyr and data.table


```r
library(dplyr)
## 
## Attaching package: 'dplyr'
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
library(data.table)
## 
## Attaching package: 'data.table'
## The following objects are masked from 'package:dplyr':
## 
##     between, first, last

rap_chart_artists <- readxl::read_excel("data/Rap_Charts.xlsx", sheet = "Artists")
rap_chart_singles <- readxl::read_excel("data/Rap_Charts.xlsx", sheet = "Singles")

(rap_artist_singles <- rap_chart_artists %>%
    dplyr::full_join(rap_chart_singles, by = "Artist_ID"))
## # A tibble: 167 x 8
##    Artist_ID First_Name Last_Name Psuedonym  Year Single  Top_US_Rap_Chart
##        <dbl> <chr>      <chr>     <chr>     <dbl> <chr>              <dbl>
##  1         4 O'Shea     Jackson   Ice Cube   1990 "\"Who…               NA
##  2         4 O'Shea     Jackson   Ice Cube   1990 "\"Ame…                1
##  3         4 O'Shea     Jackson   Ice Cube   1990 "\"End…               NA
##  4         4 O'Shea     Jackson   Ice Cube   1990 "\"Dea…               NA
##  5         4 O'Shea     Jackson   Ice Cube   1990 "\"Jac…               NA
##  6         4 O'Shea     Jackson   Ice Cube   1991 "\"Ste…                3
##  7         4 O'Shea     Jackson   Ice Cube   1992 "\"Tru…               NA
##  8         4 O'Shea     Jackson   Ice Cube   1992 "\"Wic…               NA
##  9         4 O'Shea     Jackson   Ice Cube   1993 "\"It …                1
## 10         4 O'Shea     Jackson   Ice Cube   1993 "\"Che…                1
## # ... with 157 more rows, and 1 more variable: Album <chr>
```


```r
rap_artist_singles_dt <- setDT(rap_artist_singles)
```


```r
rap_artist_singles_dt[Psuedonym %like% "LL"]
##     Artist_ID First_Name Last_Name Psuedonym Year
##  1:         1      James     Smith LL Cool J 1984
##  2:         1      James     Smith LL Cool J 1985
##  3:         1      James     Smith LL Cool J 1986
##  4:         1      James     Smith LL Cool J 1986
##  5:         1      James     Smith LL Cool J 1986
##  6:         1      James     Smith LL Cool J 1987
##  7:         1      James     Smith LL Cool J 1987
##  8:         1      James     Smith LL Cool J 1987
##  9:         1      James     Smith LL Cool J 1988
## 10:         1      James     Smith LL Cool J 1989
## 11:         1      James     Smith LL Cool J 1989
## 12:         1      James     Smith LL Cool J 1989
## 13:         1      James     Smith LL Cool J 1990
## 14:         1      James     Smith LL Cool J 1990
## 15:         1      James     Smith LL Cool J 1990
## 16:         1      James     Smith LL Cool J 1990
## 17:         1      James     Smith LL Cool J 1991
## 18:         1      James     Smith LL Cool J 1991
## 19:         1      James     Smith LL Cool J 1991
## 20:         1      James     Smith LL Cool J 1991
## 21:         1      James     Smith LL Cool J 1991
## 22:         1      James     Smith LL Cool J 1993
## 23:         1      James     Smith LL Cool J 1993
## 24:         1      James     Smith LL Cool J 1993
## 25:         1      James     Smith LL Cool J 1993
## 26:         1      James     Smith LL Cool J 1995
## 27:         1      James     Smith LL Cool J 1996
## 28:         1      James     Smith LL Cool J 1996
## 29:         1      James     Smith LL Cool J 1996
## 30:         1      James     Smith LL Cool J 1997
## 31:         1      James     Smith LL Cool J 1997
## 32:         1      James     Smith LL Cool J 1997
## 33:         1      James     Smith LL Cool J 1998
## 34:         1      James     Smith LL Cool J 1998
## 35:         1      James     Smith LL Cool J 1998
## 36:         1      James     Smith LL Cool J 1999
## 37:         1      James     Smith LL Cool J 2000
## 38:         1      James     Smith LL Cool J 2000
## 39:         1      James     Smith LL Cool J 2000
## 40:         1      James     Smith LL Cool J 2000
## 41:         1      James     Smith LL Cool J 2002
## 42:         1      James     Smith LL Cool J 2003
## 43:         1      James     Smith LL Cool J 2003
## 44:         1      James     Smith LL Cool J 2004
## 45:         1      James     Smith LL Cool J 2005
## 46:         1      James     Smith LL Cool J 2006
## 47:         1      James     Smith LL Cool J 2006
## 48:         1      James     Smith LL Cool J 2006
## 49:         1      James     Smith LL Cool J 2008
## 50:         1      James     Smith LL Cool J 2008
## 51:         1      James     Smith LL Cool J 2009
## 52:         1      James     Smith LL Cool J 2011
## 53:         1      James     Smith LL Cool J 2012
## 54:         1      James     Smith LL Cool J 2012
## 55:         1      James     Smith LL Cool J 2013
## 56:         1      James     Smith LL Cool J 2013
## 57:         1      James     Smith LL Cool J 2013
## 58:         1      James     Smith LL Cool J 2013
## 59:         1      James     Smith LL Cool J 2014
## 60:         1      James     Smith LL Cool J 2016
##     Artist_ID First_Name Last_Name Psuedonym Year
##                                                                                       Single
##  1:                                                                          "I Need a Beat"
##  2:                                                          "I Can't Live Without My Radio"
##  3:                                                                         "Rock the Bells"
##  4:                                                                    "I Can Give You More"
##  5:                                                                            "You'll Rock"
##  6:                                                                                "I'm Bad"
##  7:                                                                            "I Need Love"
##  8:                                                                      "Go Cut Creator Go"
##  9:                                                                     "Going Back to Cali"
## 10:                                                                   "I'm That Type of Guy"
## 11:                                                                           "Big Ole Butt"
## 12:                                                                       "One Shot at Love"
## 13:                                                                          "Jingling Baby"
## 14:                                                                    "To da Break of Dawn"
## 15:                                                                     "The Boomin' System"
## 16:                                                                    "Around the Way Girl"
## 17:                                                                "Mama Said Knock You Out"
## 18:                                                            "Around the Way Girl" (remix)
## 19:                                                                  "6 Minutes of Pleasure"
## 20:                                                      "Who's Afraid of the Big Bad Wolf?"
## 21:                                                                      "Strictly Business"
## 22:                                                                         "How I'm Comin'"
## 23:                                                                 "Back Seat (Of My Jeep)"
## 24:                             "Pink Cookies In a Plastic Bag Getting Crushed by Buildings"
## 25:                                                                      "Stand By Your Man"
## 26:                                                      "Hey Lover" (featuring Boyz II Men)
## 27:                                                           "Doin' It" (featuring LeShaun)
## 28:                                                              "Loungin" (featuring Total)
## 29:                                                                           "Ain't Nobody"
## 30: "Hit 'Em High (The Monstars' Anthem)" (with B-Real, Busta Rhymes, Coolio and Method Man)
## 31:                                                                             "Phenomenon"
## 32:                              "4, 3, 2, 1" (featuring Method Man, Redman, Canibusand DMX)
## 33:                                                                                 "Father"
## 34:                                                                            "Hot Hot Hot"
## 35:                                                                    "Zoom" (with Dr. Dre)
## 36:                                                                         "Deepest Bluest"
## 37:                                                                           "Imagine That"
## 38:                                                                            "Take it Off"
## 39:                                                     "You and Me" (featuring Kelly Price)
## 40:                                                                          "Shut 'Em Down"
## 41:                                                                           "Luv U Better"
## 42:                                                           "Paradise" (featuring Ameriie)
## 43:                                                                                "Amazin'"
## 44:                                                                             "Headsprung"
## 45:                                                            "Hush" (featuring 7 Aurelius)
## 46:                                          "It's LL and Santana" (featuring Juelz Santana)
## 47:                                              "Control Myself" (featuring Jennifer Lopez)
## 48:                                                       "Freeze" (featuring Lyfe Jennings)
## 49:                                                                "Cry" (featuring Lil' Mo)
## 50:                                                             "Baby" (featuring The-Dream)
## 51:                                                              "NCIS: No Crew Is Superior"
## 52:                                                              "No More" (featuring Ne-Yo)
## 53:                                                                                "Ratchet"
## 54:                                                                "Take It" (featuring Joe)
## 55:                  "Whaddup" (featuring Chuck D, Travis Barker, Tom Morello and DJ Z-Trip)
## 56:                                "We Came to Party" (featuring Snoop Dogg andFatman Scoop)
## 57:                                                  "Live for You" (featuring Brad Paisley)
## 58:           "Not Leaving You Tonight" (featuring Fitz & The Tantrums with Eddie Van Halen)
## 59:                                                         "The Hustler" (featuring Mavado)
## 60:                                                       "You Already" (featuring Troy Ave)
##                                                                                       Single
##     Top_US_Rap_Chart                                      Album
##  1:               NA                           Non-album single
##  2:               NA                                      Radio
##  3:               NA                                      Radio
##  4:               NA                                      Radio
##  5:               NA                                      Radio
##  6:               NA                          Bigger and Deffer
##  7:               NA                          Bigger and Deffer
##  8:               NA                          Bigger and Deffer
##  9:               NA                     Walking with a Panther
## 10:                1                     Walking with a Panther
## 11:               13                     Walking with a Panther
## 12:               NA                     Walking with a Panther
## 13:                6                     Walking with a Panther
## 14:               17                    Mama Said Knock You Out
## 15:                1                    Mama Said Knock You Out
## 16:                1                    Mama Said Knock You Out
## 17:                1                    Mama Said Knock You Out
## 18:               NA                    Mama Said Knock You Out
## 19:                7                    Mama Said Knock You Out
## 20:               NA                 Simply Mad About the Mouse
## 21:               NA               Strictly Business soundtrack
## 22:                1                       14 Shots to the Dome
## 23:                2                       14 Shots to the Dome
## 24:               NA                       14 Shots to the Dome
## 25:               24                       14 Shots to the Dome
## 26:                1                                  Mr. Smith
## 27:                2                                  Mr. Smith
## 28:                1                                  Mr. Smith
## 29:               23 Beavis and Butt-Head Do America soundtrack
## 30:               NA                       Space Jam soundtrack
## 31:               14                                 Phenomenon
## 32:               10                                 Phenomenon
## 33:                1                                 Phenomenon
## 34:               NA                                 Phenomenon
## 35:               NA                        Bulworth soundtrack
## 36:               NA                   Deep Blue Sea soundtrack
## 37:               16                                   G.O.A.T.
## 38:               35                                   G.O.A.T.
## 39:               44                                   G.O.A.T.
## 40:               31                Any Given Sunday soundtrack
## 41:                2                                         10
## 42:               10                                         10
## 43:               NA                                         10
## 44:                4                             The DEFinition
## 45:               11                             The DEFinition
## 46:               NA                                 Todd Smith
## 47:                9                                 Todd Smith
## 48:               NA                                 Todd Smith
## 49:               NA                                    Exit 13
## 50:               10                                    Exit 13
## 51:               NA                           Non-album single
## 52:               NA                           Non-album single
## 53:               NA                           Non-album single
## 54:               NA                                  Authentic
## 55:               NA                                  Authentic
## 56:               NA                                  Authentic
## 57:               NA                                  Authentic
## 58:               NA                                  Authentic
## 59:               NA                                 G.O.A.T. 2
## 60:               NA                                        TBA
##     Top_US_Rap_Chart                                      Album
```


```r
rap_artist_singles_dt[Psuedonym %like% "LL", .(Psuedonym, Single, Top_US_Rap_Chart)]
##     Psuedonym
##  1: LL Cool J
##  2: LL Cool J
##  3: LL Cool J
##  4: LL Cool J
##  5: LL Cool J
##  6: LL Cool J
##  7: LL Cool J
##  8: LL Cool J
##  9: LL Cool J
## 10: LL Cool J
## 11: LL Cool J
## 12: LL Cool J
## 13: LL Cool J
## 14: LL Cool J
## 15: LL Cool J
## 16: LL Cool J
## 17: LL Cool J
## 18: LL Cool J
## 19: LL Cool J
## 20: LL Cool J
## 21: LL Cool J
## 22: LL Cool J
## 23: LL Cool J
## 24: LL Cool J
## 25: LL Cool J
## 26: LL Cool J
## 27: LL Cool J
## 28: LL Cool J
## 29: LL Cool J
## 30: LL Cool J
## 31: LL Cool J
## 32: LL Cool J
## 33: LL Cool J
## 34: LL Cool J
## 35: LL Cool J
## 36: LL Cool J
## 37: LL Cool J
## 38: LL Cool J
## 39: LL Cool J
## 40: LL Cool J
## 41: LL Cool J
## 42: LL Cool J
## 43: LL Cool J
## 44: LL Cool J
## 45: LL Cool J
## 46: LL Cool J
## 47: LL Cool J
## 48: LL Cool J
## 49: LL Cool J
## 50: LL Cool J
## 51: LL Cool J
## 52: LL Cool J
## 53: LL Cool J
## 54: LL Cool J
## 55: LL Cool J
## 56: LL Cool J
## 57: LL Cool J
## 58: LL Cool J
## 59: LL Cool J
## 60: LL Cool J
##     Psuedonym
##                                                                                       Single
##  1:                                                                          "I Need a Beat"
##  2:                                                          "I Can't Live Without My Radio"
##  3:                                                                         "Rock the Bells"
##  4:                                                                    "I Can Give You More"
##  5:                                                                            "You'll Rock"
##  6:                                                                                "I'm Bad"
##  7:                                                                            "I Need Love"
##  8:                                                                      "Go Cut Creator Go"
##  9:                                                                     "Going Back to Cali"
## 10:                                                                   "I'm That Type of Guy"
## 11:                                                                           "Big Ole Butt"
## 12:                                                                       "One Shot at Love"
## 13:                                                                          "Jingling Baby"
## 14:                                                                    "To da Break of Dawn"
## 15:                                                                     "The Boomin' System"
## 16:                                                                    "Around the Way Girl"
## 17:                                                                "Mama Said Knock You Out"
## 18:                                                            "Around the Way Girl" (remix)
## 19:                                                                  "6 Minutes of Pleasure"
## 20:                                                      "Who's Afraid of the Big Bad Wolf?"
## 21:                                                                      "Strictly Business"
## 22:                                                                         "How I'm Comin'"
## 23:                                                                 "Back Seat (Of My Jeep)"
## 24:                             "Pink Cookies In a Plastic Bag Getting Crushed by Buildings"
## 25:                                                                      "Stand By Your Man"
## 26:                                                      "Hey Lover" (featuring Boyz II Men)
## 27:                                                           "Doin' It" (featuring LeShaun)
## 28:                                                              "Loungin" (featuring Total)
## 29:                                                                           "Ain't Nobody"
## 30: "Hit 'Em High (The Monstars' Anthem)" (with B-Real, Busta Rhymes, Coolio and Method Man)
## 31:                                                                             "Phenomenon"
## 32:                              "4, 3, 2, 1" (featuring Method Man, Redman, Canibusand DMX)
## 33:                                                                                 "Father"
## 34:                                                                            "Hot Hot Hot"
## 35:                                                                    "Zoom" (with Dr. Dre)
## 36:                                                                         "Deepest Bluest"
## 37:                                                                           "Imagine That"
## 38:                                                                            "Take it Off"
## 39:                                                     "You and Me" (featuring Kelly Price)
## 40:                                                                          "Shut 'Em Down"
## 41:                                                                           "Luv U Better"
## 42:                                                           "Paradise" (featuring Ameriie)
## 43:                                                                                "Amazin'"
## 44:                                                                             "Headsprung"
## 45:                                                            "Hush" (featuring 7 Aurelius)
## 46:                                          "It's LL and Santana" (featuring Juelz Santana)
## 47:                                              "Control Myself" (featuring Jennifer Lopez)
## 48:                                                       "Freeze" (featuring Lyfe Jennings)
## 49:                                                                "Cry" (featuring Lil' Mo)
## 50:                                                             "Baby" (featuring The-Dream)
## 51:                                                              "NCIS: No Crew Is Superior"
## 52:                                                              "No More" (featuring Ne-Yo)
## 53:                                                                                "Ratchet"
## 54:                                                                "Take It" (featuring Joe)
## 55:                  "Whaddup" (featuring Chuck D, Travis Barker, Tom Morello and DJ Z-Trip)
## 56:                                "We Came to Party" (featuring Snoop Dogg andFatman Scoop)
## 57:                                                  "Live for You" (featuring Brad Paisley)
## 58:           "Not Leaving You Tonight" (featuring Fitz & The Tantrums with Eddie Van Halen)
## 59:                                                         "The Hustler" (featuring Mavado)
## 60:                                                       "You Already" (featuring Troy Ave)
##                                                                                       Single
##     Top_US_Rap_Chart
##  1:               NA
##  2:               NA
##  3:               NA
##  4:               NA
##  5:               NA
##  6:               NA
##  7:               NA
##  8:               NA
##  9:               NA
## 10:                1
## 11:               13
## 12:               NA
## 13:                6
## 14:               17
## 15:                1
## 16:                1
## 17:                1
## 18:               NA
## 19:                7
## 20:               NA
## 21:               NA
## 22:                1
## 23:                2
## 24:               NA
## 25:               24
## 26:                1
## 27:                2
## 28:                1
## 29:               23
## 30:               NA
## 31:               14
## 32:               10
## 33:                1
## 34:               NA
## 35:               NA
## 36:               NA
## 37:               16
## 38:               35
## 39:               44
## 40:               31
## 41:                2
## 42:               10
## 43:               NA
## 44:                4
## 45:               11
## 46:               NA
## 47:                9
## 48:               NA
## 49:               NA
## 50:               10
## 51:               NA
## 52:               NA
## 53:               NA
## 54:               NA
## 55:               NA
## 56:               NA
## 57:               NA
## 58:               NA
## 59:               NA
## 60:               NA
##     Top_US_Rap_Chart
```


```r
rap_artist_singles_dt[Psuedonym %like% "LL" & !is.na(Top_US_Rap_Chart), .(Psuedonym, Single, Top_US_Rap_Chart)]
##     Psuedonym                                                      Single
##  1: LL Cool J                                      "I'm That Type of Guy"
##  2: LL Cool J                                              "Big Ole Butt"
##  3: LL Cool J                                             "Jingling Baby"
##  4: LL Cool J                                       "To da Break of Dawn"
##  5: LL Cool J                                        "The Boomin' System"
##  6: LL Cool J                                       "Around the Way Girl"
##  7: LL Cool J                                   "Mama Said Knock You Out"
##  8: LL Cool J                                     "6 Minutes of Pleasure"
##  9: LL Cool J                                            "How I'm Comin'"
## 10: LL Cool J                                    "Back Seat (Of My Jeep)"
## 11: LL Cool J                                         "Stand By Your Man"
## 12: LL Cool J                         "Hey Lover" (featuring Boyz II Men)
## 13: LL Cool J                              "Doin' It" (featuring LeShaun)
## 14: LL Cool J                                 "Loungin" (featuring Total)
## 15: LL Cool J                                              "Ain't Nobody"
## 16: LL Cool J                                                "Phenomenon"
## 17: LL Cool J "4, 3, 2, 1" (featuring Method Man, Redman, Canibusand DMX)
## 18: LL Cool J                                                    "Father"
## 19: LL Cool J                                              "Imagine That"
## 20: LL Cool J                                               "Take it Off"
## 21: LL Cool J                        "You and Me" (featuring Kelly Price)
## 22: LL Cool J                                             "Shut 'Em Down"
## 23: LL Cool J                                              "Luv U Better"
## 24: LL Cool J                              "Paradise" (featuring Ameriie)
## 25: LL Cool J                                                "Headsprung"
## 26: LL Cool J                               "Hush" (featuring 7 Aurelius)
## 27: LL Cool J                 "Control Myself" (featuring Jennifer Lopez)
## 28: LL Cool J                                "Baby" (featuring The-Dream)
##     Psuedonym                                                      Single
##     Top_US_Rap_Chart
##  1:                1
##  2:               13
##  3:                6
##  4:               17
##  5:                1
##  6:                1
##  7:                1
##  8:                7
##  9:                1
## 10:                2
## 11:               24
## 12:                1
## 13:                2
## 14:                1
## 15:               23
## 16:               14
## 17:               10
## 18:                1
## 19:               16
## 20:               35
## 21:               44
## 22:               31
## 23:                2
## 24:               10
## 25:                4
## 26:               11
## 27:                9
## 28:               10
##     Top_US_Rap_Chart
```


```r
rap_artist_singles_dt[Psuedonym %like% "LL" & !is.na(Top_US_Rap_Chart), .(.N), c("Psuedonym", "Top_US_Rap_Chart")]
##     Psuedonym Top_US_Rap_Chart N
##  1: LL Cool J                1 8
##  2: LL Cool J               13 1
##  3: LL Cool J                6 1
##  4: LL Cool J               17 1
##  5: LL Cool J                7 1
##  6: LL Cool J                2 3
##  7: LL Cool J               24 1
##  8: LL Cool J               23 1
##  9: LL Cool J               14 1
## 10: LL Cool J               10 3
## 11: LL Cool J               16 1
## 12: LL Cool J               35 1
## 13: LL Cool J               44 1
## 14: LL Cool J               31 1
## 15: LL Cool J                4 1
## 16: LL Cool J               11 1
## 17: LL Cool J                9 1
```


```r
rap_artist_singles_dt[Psuedonym %like% "LL" & !is.na(Top_US_Rap_Chart), .(.N), c("Psuedonym", "Top_US_Rap_Chart")][order(Top_US_Rap_Chart)]
##     Psuedonym Top_US_Rap_Chart N
##  1: LL Cool J                1 8
##  2: LL Cool J                2 3
##  3: LL Cool J                4 1
##  4: LL Cool J                6 1
##  5: LL Cool J                7 1
##  6: LL Cool J                9 1
##  7: LL Cool J               10 3
##  8: LL Cool J               11 1
##  9: LL Cool J               13 1
## 10: LL Cool J               14 1
## 11: LL Cool J               16 1
## 12: LL Cool J               17 1
## 13: LL Cool J               23 1
## 14: LL Cool J               24 1
## 15: LL Cool J               31 1
## 16: LL Cool J               35 1
## 17: LL Cool J               44 1
```

