---
title: "11 Data Import Exercise Solutions"
author: "Sean Angiolillo"
date: "18 March 2018"
output: 
  html_document: 
    keep_md: yes
---




```r
library(tidyverse)
```

## 11.2.2 Exercises

1. What function would you use to read a file where fields were separated with
“|”?

`read_delim()` is for importing files where you want to specify your own (typically less common) delimiter. The syntax would be as follows:


```r
read_delim(filename, delim = "|")
```

2. Apart from `file`, `skip`, and `comment`, what other arguments do `read_csv()` and `read_tsv()` have in common?

All arguments are the same. The only difference is the delimiter: `read_csv()` has a comma and `read_tsv()` has a tab.


```r
union(names(formals(read_csv)), names(formals(read_tsv)))
```

```
##  [1] "file"      "col_names" "col_types" "locale"    "na"       
##  [6] "quoted_na" "quote"     "comment"   "trim_ws"   "skip"     
## [11] "n_max"     "guess_max" "progress"
```

3. What are the most important arguments to `read_fwf()`?

`read_fwf()` reads fixed width files into a tibble. `col_positions` is the key argument because that is where you specify the column positions.

4. Sometimes strings in a CSV file contain commas. To prevent them from causing problems they need to be surrounded by a quoting character, like `"` or `'`. By convention, `read_csv()` assumes that the quoting character will be `"`, and if you want to change it you’ll need to use `read_delim()` instead. What arguments do you need to specify to read the following text into a data frame?


```r
x <- "x,y\n1,'a,b'"
read_delim(x, delim = ",", quote = "'")
```

```
## # A tibble: 1 x 2
##       x y    
##   <int> <chr>
## 1     1 a,b
```

5. Identify what is wrong with each of the following inline CSV files. What happens when you run the code?


```r
read_csv("a,b\n1,2,3\n4,5,6")
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 5 col     row col   expected  actual    file         expected   <int> <chr> <chr>     <chr>     <chr>        actual 1     1 <NA>  2 columns 3 columns literal data file 2     2 <NA>  2 columns 3 columns literal data
```

```
## # A tibble: 2 x 2
##       a     b
##   <int> <int>
## 1     1     2
## 2     4     5
```

Two columns are specified in the header but three columns are given in each row so the last column is dropped in each row.


```r
read_csv("a,b,c\n1,2\n1,2,3,4")
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 5 col     row col   expected  actual    file         expected   <int> <chr> <chr>     <chr>     <chr>        actual 1     1 <NA>  3 columns 2 columns literal data file 2     2 <NA>  3 columns 4 columns literal data
```

```
## # A tibble: 2 x 3
##       a     b     c
##   <int> <int> <int>
## 1     1     2    NA
## 2     1     2     3
```

The second row is missing a value for the third column and the third row has a value for a non-existent fourth row.


```r
read_csv("a,b\n\"1")
```

```
## Warning: 2 parsing failures.
## row # A tibble: 2 x 5 col     row col   expected                     actual    file         expected   <int> <chr> <chr>                        <chr>     <chr>        actual 1     1 a     closing quote at end of file ""        literal data file 2     1 <NA>  2 columns                    1 columns literal data
```

```
## # A tibble: 1 x 2
##       a b    
##   <int> <chr>
## 1     1 <NA>
```
There's a backslash in the first row escaping the quotes which renders the one as a numeric instead of a character, which may have been the intention.


```r
read_csv("a,b\n1,2\na,b")
```

```
## # A tibble: 2 x 2
##   a     b    
##   <chr> <chr>
## 1 1     2    
## 2 a     b
```

No problem here but note that both columns are character vectors, which may not have been the intention.


```r
read_csv("a;b\n1;3")
```

```
## # A tibble: 1 x 1
##   `a;b`
##   <chr>
## 1 1;3
```

Here the author probably wanted to use `read_csv2()` because it is semicolon-delimited instead of comma-delimited.

## 11.3.5 Exercises

1. What are the most important arguments to `locale()`?

`locale()` arguments encoding, timezone (`tz`), date and time formats, and number formats.

2. What happens if you try and set `decimal_mark` and `grouping_mark` to the same character? What happens to the default value of `grouping_mark` when you set `decimal_mark` to `“,”`? What happens to the default value of `decimal_mark` when you set the `grouping_mark` to `“.”`?

`decimal_mark` and `grouping_mark` must be different as we'd see if we ran the code below.


```r
locale(decimal_mark = ".", grouping_mark = ".")
```

Setting `decimal_mark` to "." turns `grouping_mark` to ",".


```r
locale(decimal_mark = ".")$grouping_mark
```

```
## [1] ","
```

Setting `grouping_mark` to "," turns `decimal_mark` to ".".
 

```r
locale(grouping_mark = ",")$decimal_mark
```

```
## [1] "."
```

3. I didn’t discuss the `date_format` and `time_format` options to `locale()`. What do they do? Construct an example that shows when they might be useful.

The `date_format` and `time_format` options make it easier to parse dates in different languages. For instance:


```r
parse_date("1 enero 2015", "%d %B %Y", locale = locale("es"))
```

```
## [1] "2015-01-01"
```

4. If you live outside the US, create a new locale object that encapsulates the settings for the types of file you read most commonly.

In India one common difference is the lakh and crore numbering system. But it doesn't create any parsing problems.


```r
parse_number("INR 1,50,000.50")
```

```
## [1] 150000.5
```

5. What’s the difference between `read_csv()` and `read_csv2()`?

`read_csv()` is for comma-delimited files and `read_csv2()` for semicolon-delimited files.

6. What are the most common encodings used in Europe? What are the most common encodings used in Asia? Do some googling to find out.

[This article]("http://kunststube.net/encoding/") is a good place to start.

7. Generate the correct format string to parse each of the following dates and times:


```r
d1 <- "January 1, 2010"
parse_date(d1, "%B %d, %Y")
```

```
## [1] "2010-01-01"
```

```r
d2 <- "2015-Mar-07"
parse_date(d2, "%Y-%b-%d")
```

```
## [1] "2015-03-07"
```

```r
d3 <- "06-Jun-2017"
parse_date(d3, "%d-%b-%Y")
```

```
## [1] "2017-06-06"
```

```r
d4 <- c("August 19 (2015)", "July 1 (2015)")
parse_date(d4, "%B %d (%Y)")
```

```
## [1] "2015-08-19" "2015-07-01"
```

```r
d5 <- "12/30/14" # Dec 30, 2014
parse_date(d5, "%m/%d/%y")
```

```
## [1] "2014-12-30"
```

```r
t1 <- "1705"
parse_time(t1, "%H%M")
```

```
## 17:05:00
```

```r
t2 <- "11:15:10.12 PM"
parse_time(t2, "%H:%M:%OS %p")
```

```
## 23:15:10.12
```

