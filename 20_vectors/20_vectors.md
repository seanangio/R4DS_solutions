---
title: "20 Vectors Exercises Solutions"
author: "Sean Angiolillo"
date: "3/31/2018"
output: 
  html_document: 
    keep_md: yes
---




```r
library(tidyverse)
```

## 20.3.5 Exercises

1. Describe the difference between `is.finite(x)` and `!is.infinite(x)`.

Seemingly these would returns the same results, and they do for numeric and infinity values. They do not behave this way for NA and NaN values, which seem to be neither finite nor infinite.


```r
x <- c(0, Inf, NA, NaN)
is.finite(x)
```

```
## [1]  TRUE FALSE FALSE FALSE
```

```r
!is.infinite(x)
```

```
## [1]  TRUE FALSE  TRUE  TRUE
```

2. Read the source code for `dplyr::near()` (Hint: to see the source code, drop the `()`). How does it work?

It checks if the absolute difference between two vectors of floating point numbers is less than a tolerance of comparison, which by default is set to machine precision of 0.


```r
dplyr::near
```

```
## function (x, y, tol = .Machine$double.eps^0.5) 
## {
##     abs(x - y) < tol
## }
## <environment: namespace:dplyr>
```

3. A logical vector can take 3 possible values. How many possible values can an integer vector take? How many possible values can a double take? Use google to do some research.

4. Brainstorm at least four functions that allow you to convert a double to an integer. How do they differ? Be precise.

`as.integer()` is an obvious one-- explicit coercion. I thought rounding functions `ceiling`, `floor`, `trunc` and `round` would work but they don't actually change the type to integer even though they look like integers.


```r
typeof(ceiling(1.5))
```

```
## [1] "double"
```

```r
typeof(floor(1.9))
```

```
## [1] "double"
```

```r
typeof(trunc(1.9))
```

```
## [1] "double"
```

```r
typeof(round(1.9))
```

```
## [1] "double"
```

I thought of trying to implictly coerce to an integer by converting to logical and then using a summary function but that also doesn't seem to work.


```r
typeof(mean(as.logical(1.5)))
```

```
## [1] "double"
```

5. What functions from the readr package allow you to turn a string into logical, integer, and double vector?


```r
str_lgl <- "TRUE"
readr::parse_logical(str_lgl) %>% typeof()
```

```
## [1] "logical"
```

```r
str_int <- "5"
readr::parse_integer(str_int) %>% typeof()
```

```
## [1] "integer"
```

```r
str_dbl <- "5.5"
readr::parse_double(str_dbl) %>% typeof()
```

```
## [1] "double"
```

## 20.4.6 Exercises

1. What does `mean(is.na(x))` tell you about a vector `x`? What about `sum(!is.finite(x))`?

`mean(is.na(x))` returns what percentage of values in the vector are `NA`. `sum(!is.finite(x))` returns the sum of the non-finite values in the vector. For example,


```r
x <- c(0, 1, 2, NA, Inf)
mean(is.na(x))
```

```
## [1] 0.2
```

```r
sum(!is.finite(x))
```

```
## [1] 2
```

2. Carefully read the documentation of `is.vector()`. What does it actually test for? Why does `is.atomic()` not agree with the definition of atomic vectors above?

`is.vector` returns `TRUE` if `x` is a vector of the specified mode having no attributes other than names. It returns `FALSE` otherwise.


```r
x <- c(one = 1)
is.vector(x)
```

```
## [1] TRUE
```

```r
y <- 1:10
attr(y, 'dim') <- c(2, 5)
is.vector(y)
```

```
## [1] FALSE
```

`is.atomic` returns `TRUE` if x is of an atomic type (or `NULL`) and `FALSE` otherwise. So it fails for lists, which are recursive.


```r
is.atomic(x)
```

```
## [1] TRUE
```

```r
is.atomic(y)
```

```
## [1] TRUE
```

```r
is.atomic(list(x,y))
```

```
## [1] FALSE
```

```r
is.recursive(list(x,y))
```

```
## [1] TRUE
```

3. Compare and contrast `setNames()` with `purrr::set_names()`.

`setNames()` sets the names on an object and returns the object. According to the documentation for `purrr::set_names()`, it is a snake case wrapper for `setNames()`, with tweaked defaults and stricter argument checking. The first example yield the same results for either function. But set_names() has more advanced options as the second and third cases show.


```r
setNames( 1:4, c("a", "b", "c", "d"))
```

```
## a b c d 
## 1 2 3 4
```

```r
set_names(1:4, c("a", "b", "c", "d"))
```

```
## a b c d 
## 1 2 3 4
```

```r
set_names(letters[1:5])
```

```
##   a   b   c   d   e 
## "a" "b" "c" "d" "e"
```

```r
# setNames(letters[1:5]) fails because nm is missing with no default

set_names(head(mtcars), toupper)
```

```
##                    MPG CYL DISP  HP DRAT    WT  QSEC VS AM GEAR CARB
## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```

```r
#setNames(head(mtcars), toupper) cannot supply a function
```

4. Create functions that take a vector as input and returns:

* The last value. Should you use `[` or `[[`?

If we use `[[` the function below returns the last single element even if given a list.

```r
w <- c(1:2, list(a = "a", b = "b"))
return_last <- function(x) { x[[length(x)]] }
return_last(w)
```

```
## [1] "b"
```

* The elements at even numbered positions.


```r
return_even <- function(x) { x[seq_along(x) %% 2 == 0] }
return_even(w)
```

```
## [[1]]
## [1] 2
## 
## $b
## [1] "b"
```

* Every element except the last value.


```r
get_all_but_last <- function(x) { x[-length(x)] }
get_all_but_last(w)
```

```
## [[1]]
## [1] 1
## 
## [[2]]
## [1] 2
## 
## $a
## [1] "a"
```

* Only even numbers (and no missing values).


```r
x <- c(1:4, NA, NA)
return_only_even <- function(x) { x[!is.na(x) & seq_along(x) %% 2 == 0] }
return_only_even(x)
```

```
## [1] 2 4
```

5. Why is `x[-which(x > 0)]` not the same as `x[x <= 0]`?

The first drops values at specified positions. The second keeps `TRUE` values according to a logical statement. They differ in their handling of special and missing values. For instance in the example below, `Nan` will be retained in the first but converted to `NA` in the second. It passes the bar for selection in the first but is modified to `NA` by the logical statement in the second.


```r
x <- c(-1, 0, 1, NA, Inf, NaN)
x[-which(x > 0)] # removes elements 3 and 5
```

```
## [1]  -1   0  NA NaN
```

```r
x[x <= 0] # elements 3 and 5 are false but NaN becomes NA
```

```
## [1] -1  0 NA NA
```

6. What happens when you subset with a positive integer that’s bigger than the length of the vector? What happens when you subset with a name that doesn’t exist?

It depends if you are subsetting with `[` or `[[`. Whether subsetting with an integer or a non-existent name, using `[` returns `NA`. But using `[[` reutrns an error.


```r
x <- 1:3

# [ yields NA
x[4]
```

```
## [1] NA
```

```r
x[c("four")]
```

```
## [1] NA
```

```r
# [[ yield subscript out of bounds error
#x[[4]] 
#x[[c("four")]]
```

## 20.5.4 Exercises

1. Draw the following lists as nested sets:

* `list(a, b, list(c, d), list(e, f))`

* `list(list(list(list(list(list(a))))))`

2. What happens if you subset a tibble as if you’re subsetting a list? What are the key differences between a list and a tibble?

A tibble is similar to a list of vectors of the same length, and so subsetting behaves similarly.

## 20.7.4 Exercises

1. What does `hms::hms(3600)` return? How does it print? What primitive type is the augmented vector built on top of? What attributes does it use?

It returns time in hours printed with hours, minutes and seconds separated by colons. It is built on top of the double primitive type. Attributes include units, which are seconds, and the classes "hms" and "difftime".


```r
hms::hms(3600)
```

```
## 01:00:00
```

```r
typeof(hms::hms(3600))
```

```
## [1] "double"
```

```r
attributes(hms::hms(3600))
```

```
## $units
## [1] "secs"
## 
## $class
## [1] "hms"      "difftime"
```

2. Try and make a tibble that has columns with different lengths. What happens?

You'll get an error specifying which columns must match the length of another.

3. Based on the definition above, is it ok to have a list as a column of a tibble?

Sure. Columns just need to be the same length.


```r
tbl <- tibble(one = list(1:4, list(1, 2, 3), 1:2, "dog"), two = 1:4)
tbl
```

```
## # A tibble: 4 x 2
##   one          two
##   <list>     <int>
## 1 <int [4]>      1
## 2 <list [3]>     2
## 3 <int [2]>      3
## 4 <chr [1]>      4
```

