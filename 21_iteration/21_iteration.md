---
title: "21 Iteration Exercises Solutions"
author: "Sean Angiolillo"
date: "4/1/2018"
output: 
  html_document: 
    keep_md: yes
---




```r
library(tidyverse)
library(stringr)
library(nycflights13)
```

## 21.2.1 Exercises

1. Write for loops to:

* Compute the mean of every column in mtcars.


```r
means <- vector("double", ncol(mtcars))
names(means) <- names(mtcars)
for (i in seq_along(mtcars)) {
    means[[i]] <- mean(mtcars[[i]])
}
means
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500   0.406250   3.687500   2.812500
```

* Determine the type of each column in `nycflights13::flights`.


```r
types <- vector("character", ncol(flights))
names(types) <- names(flights)
for (i in seq_along(flights)) {
    types[[i]] <- typeof(flights[[i]])
    names(types) <- names(flights)
}
types
```

```
##           year          month            day       dep_time sched_dep_time 
##      "integer"      "integer"      "integer"      "integer"      "integer" 
##      dep_delay       arr_time sched_arr_time      arr_delay        carrier 
##       "double"      "integer"      "integer"       "double"    "character" 
##         flight        tailnum         origin           dest       air_time 
##      "integer"    "character"    "character"    "character"       "double" 
##       distance           hour         minute      time_hour 
##       "double"       "double"       "double"       "double"
```

If we wanted classes instead of types, we could ask for a list because a column can have multiple classes. Note `time_hour` for instance.


```r
classes <- vector("list", ncol(flights))
names(classes) <- names(flights)
for (i in seq_along(flights)) {
    classes[[i]] <- class(flights[[i]])
}
classes
```

```
## $year
## [1] "integer"
## 
## $month
## [1] "integer"
## 
## $day
## [1] "integer"
## 
## $dep_time
## [1] "integer"
## 
## $sched_dep_time
## [1] "integer"
## 
## $dep_delay
## [1] "numeric"
## 
## $arr_time
## [1] "integer"
## 
## $sched_arr_time
## [1] "integer"
## 
## $arr_delay
## [1] "numeric"
## 
## $carrier
## [1] "character"
## 
## $flight
## [1] "integer"
## 
## $tailnum
## [1] "character"
## 
## $origin
## [1] "character"
## 
## $dest
## [1] "character"
## 
## $air_time
## [1] "numeric"
## 
## $distance
## [1] "numeric"
## 
## $hour
## [1] "numeric"
## 
## $minute
## [1] "numeric"
## 
## $time_hour
## [1] "POSIXct" "POSIXt"
```

* Compute the number of unique values in each column of iris.


```r
n_unique_vals <- vector("double", ncol(iris))
names(n_unique_vals) <- names(iris)
for (i in seq_along(iris)) {
    n_unique_vals[[i]] <- length(unique(iris[[i]]))
}
n_unique_vals
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##           35           23           43           22            3
```

* Generate 10 random normals for each of μ= -10, 0, 10, and 100


```r
mus <- c(-10, 0, 10, 100)
rnorms <- vector("list", length(mus))
names(rnorms) <- mus
for (i in seq_along(mus)) {
    set.seed(1)
    rnorms[[i]] <- rnorm(10, mean = mus[[i]])
}
rnorms
```

```
## $`-10`
##  [1] -10.626454  -9.816357 -10.835629  -8.404719  -9.670492 -10.820468
##  [7]  -9.512571  -9.261675  -9.424219 -10.305388
## 
## $`0`
##  [1] -0.6264538  0.1836433 -0.8356286  1.5952808  0.3295078 -0.8204684
##  [7]  0.4874291  0.7383247  0.5757814 -0.3053884
## 
## $`10`
##  [1]  9.373546 10.183643  9.164371 11.595281 10.329508  9.179532 10.487429
##  [8] 10.738325 10.575781  9.694612
## 
## $`100`
##  [1]  99.37355 100.18364  99.16437 101.59528 100.32951  99.17953 100.48743
##  [8] 100.73832 100.57578  99.69461
```

2. Eliminate the for loop in each of the following examples by taking advantage of an existing function that works with vectors:

These can be simplified to `str_c()`, `sd()`, and `cumsum()`.


```r
out <- ""
for (x in letters) {
  out <- stringr::str_c(out, x)
}
identical(out, str_c(letters, collapse = ""))
```

```
## [1] TRUE
```

```r
x <- sample(100)
sd <- 0
for (i in seq_along(x)) {
  sd <- sd + (x[i] - mean(x)) ^ 2
}
sd <- sqrt(sd / (length(x) - 1))
identical(sd, sd(x))
```

```
## [1] TRUE
```

```r
x <- runif(100)
out <- vector("numeric", length(x))
out[1] <- x[1]
for (i in 2:length(x)) {
  out[i] <- out[i - 1] + x[i]
}
identical(out, cumsum(x))
```

```
## [1] TRUE
```

3. Combine your function writing and for loop skills:

* Write a for loop that `prints()` the lyrics to the children’s song “Alice the camel”.


```r
humps <- c("five", "four", "three", "two", "one", "no")
for (i in humps) {
  if (i != "one") {
    cat(str_c("Alice the camel has ", rep(i, 3), " humps.",
             collapse = "\n"), "\n")
  } else {
    cat(str_c("Alice the camel has ", rep(i, 3), " hump.",
             collapse = "\n"), "\n")  
  }
  if (i == "no") {
    cat("Now Alice is a horse.\n")
  } else {
    cat("So go, Alice, go.\n")
  }
  cat("\n")
}
```

```
## Alice the camel has five humps.
## Alice the camel has five humps.
## Alice the camel has five humps. 
## So go, Alice, go.
## 
## Alice the camel has four humps.
## Alice the camel has four humps.
## Alice the camel has four humps. 
## So go, Alice, go.
## 
## Alice the camel has three humps.
## Alice the camel has three humps.
## Alice the camel has three humps. 
## So go, Alice, go.
## 
## Alice the camel has two humps.
## Alice the camel has two humps.
## Alice the camel has two humps. 
## So go, Alice, go.
## 
## Alice the camel has one hump.
## Alice the camel has one hump.
## Alice the camel has one hump. 
## So go, Alice, go.
## 
## Alice the camel has no humps.
## Alice the camel has no humps.
## Alice the camel has no humps. 
## Now Alice is a horse.
```

* Convert the nursery rhyme “ten in the bed” to a function. Generalise it to any number of people in any sleeping structure.


```r
ten_in_bed <- function(n, structure) {
    for (i in seq(n, 1)) {
        if (i != 1) {
            cat(str_c("There were ", i, " in the ", structure, "\n"))
        } else {
            cat(str_c("There was ", i, " in the ", structure, "\n"))
        }
        cat("And the little one said,\n")
        if (i != 1) {
            cat("Roll over! Roll over!\n")
            cat("So they all rolled over and one fell out\n")
        } else {
            cat("Alone at last!")
        }
        cat("\n")
    }
}
ten_in_bed(2, "bed")
```

```
## There were 2 in the bed
## And the little one said,
## Roll over! Roll over!
## So they all rolled over and one fell out
## 
## There was 1 in the bed
## And the little one said,
## Alone at last!
```

* Convert the song “99 bottles of beer on the wall” to a function. Generalise to any number of any vessel containing any liquid on any surface.


```r
drinking_song <- function(n, vessel_s, vessel_p, liquid, surface) {
    for (i in seq(n, 0)) {
        
        if (i > 2) {
            cat(str_c(i, " ", vessel_p, " of ", liquid, " on the ", surface, ", ", i, 
                      " ", vessel_p, " of ", liquid, ".\n"))
            cat(str_c("Take one down and pass it around, ", i - 1, " ", vessel_p, " of ",
                      liquid, " on the ", surface, ".\n"))
        }
        
        if (i == 2) {
            cat(str_c(i, " ", vessel_p, " of ", liquid, " on the ", surface, ", ", i,
                      " ", vessel_p, " of ", liquid, ".\n"))
            cat(str_c("Take one down and pass it around, ", i - 1, " ", vessel_s, " of ",
                      liquid, " on the ", surface, ".\n"))
        }
        
        if (i == 1) {
            cat(str_c(i, " ", vessel_s, " of ", liquid, " on the ", surface, ", ", i,
                      " ", vessel_s, " of ", liquid, ".\n"))
            cat(str_c("Take one down and pass it around, no more ", vessel_p, " of ",
                      liquid, " on the ", surface, ".\n"))
        }
        
        if (i == 0) {
            cat(str_c("No more ", vessel_p, " of ", liquid, " on the ", surface, 
                        ", no more ", vessel_p, " of ", liquid, ".\n"))
            cat(str_c("Go to the store and buy some more, ", i, " ", vessel_p, " of ",
                      liquid, " on the ", surface, "."))
        }
        cat("\n")
    }   
}    
drinking_song(3, "bottle", "bottles", "beer", "wall")    
```

```
## 3 bottles of beer on the wall, 3 bottles of beer.
## Take one down and pass it around, 2 bottles of beer on the wall.
## 
## 2 bottles of beer on the wall, 2 bottles of beer.
## Take one down and pass it around, 1 bottle of beer on the wall.
## 
## 1 bottle of beer on the wall, 1 bottle of beer.
## Take one down and pass it around, no more bottles of beer on the wall.
## 
## No more bottles of beer on the wall, no more bottles of beer.
## Go to the store and buy some more, 0 bottles of beer on the wall.
```

4. It’s common to see for loops that don’t preallocate the output and instead increase the length of a vector at each step:


```r
output <- vector("integer", 0)
for (i in seq_along(x)) {
  output <- c(output, lengths(x[[i]]))
}
output
```

How does this affect performance? Design and execute an experiment.

We'll create two functions that add elements to a vector, one pre-allocating the output and one that doesn't and then compare the two functions using the `microbenchmark` package. Comparing the mean times shows the considerable difference between a seemingly small change to the code.


```r
add_to_vector <- function(n) {
  output <- vector("integer", 0) # no pre-allocation
  for (i in seq_len(n)) {
    output <- c(output, i)
  }
  output
}

add_to_vector2 <- function(n) {
  output <- vector("integer", n) # pre-allocation length
  for (i in seq_len(n)) {
    output[[i]] <- i
  }
  output
}

library(microbenchmark)
n <- 10000
microbenchmark(add_to_vector(n), add_to_vector2(n), times = 3)
```

```
## Unit: milliseconds
##               expr        min         lq       mean     median        uq
##   add_to_vector(n) 195.541574 226.021139 243.300438 256.500704 267.17987
##  add_to_vector2(n)   7.076345   8.163813   9.334531   9.251282  10.46362
##        max neval cld
##  277.85904     3   b
##   11.67596     3  a
```

## 21.3.5 Exercises

1. Imagine you have a directory full of CSV files that you want to read in. You have their paths in a vector, `files <- dir("data/", pattern = "\\.csv$", full.names = TRUE)`, and now want to read each one with `read_csv()`. Write the for loop that will load them into a single data frame.

Follow Hadley's advice: "You might be generating a big data frame. Instead of sequentially `rbind()`ing in each iteration, save the output in a list, then use `dplyr::bind_rows(output)` to combine the output into a single data frame."


```r
files <- dir("data/", pattern = "\\.csv$", full.names = TRUE)
output <- vector("list", length(files))
for (fname in seq_along(files)) {
  output[[i]] <- read_csv(files[[i]])
}
df <- bind_rows(output)
```

2. What happens if you use `for (nm in names(x))` and `x` has no names? What if only some of the elements are named? What if the names are not unique?

When `x` has no names, `names(x)` is `NULL` so its length is 0. No output is returned.


```r
x <- 1:5
for (nm in names(x)) {
    print(mat[[nm]])
}
```

When only some elements are named, we get an error when trying to access an element without a name.


```r
names(x) <- c("one", NA, "")
for (nm in names(x)) {
    print(x[[nm]])
}
```

```
## [1] 1
```

```
## Error in x[[nm]]: subscript out of bounds
```

If we have duplicate names, we can only access the element of the first instance of the duplicated name.


```r
names(x) <- c("one", "two", "two", "four", "five")

for (nm in names(x)) {
    #print(nm)
    print(x[[nm]])
}
```

```
## [1] 1
## [1] 2
## [1] 2
## [1] 4
## [1] 5
```

3. Write a function that prints the mean of each numeric column in a data frame, along with its name. For example, `show_mean(iris)` would print:


```r
show_mean(iris)
#> Sepal.Length: 5.84
#> Sepal.Width:  3.06
#> Petal.Length: 3.76
#> Petal.Width:  1.20
```

(Extra challenge: what function did I use to make sure that the numbers lined up nicely, even though the variable names had different lengths?)

This would be a situation where we could loop over the dataframe's column names. We can still pre-allocate the empty means vector.


```r
show_means <- function(df, digits = 2) {
    num_df <- keep(df, is.numeric)
    means <- vector("double", ncol(num_df))
    names(means) <- names(num_df)
    for (nm in names(num_df)) {
        means[[nm]] <- round(mean(num_df[[nm]]), digits)
    }
    means
}
show_means(iris)
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width 
##         5.84         3.06         3.76         1.20
```

If we want the format as described though, it seems more trouble than it's worth to do that pre-allocation. (I haven't right-aligned).


```r
show_means2 <- function(df, digits = 2) {
    num_df <- keep(df, is.numeric)
    for (i in seq_along(num_df)) {
        cat(str_c(names(num_df)[[i]], ": ", round(mean(num_df[[i]]), digits), "\n"))
    }
}
show_means2(iris)
```

```
## Sepal.Length: 5.84
## Sepal.Width: 3.06
## Petal.Length: 3.76
## Petal.Width: 1.2
```

4. What does this code do? How does it work?

`trans` is a list containing two functions that we want to apply to specific columns of a dataframe. We loop over the list applying the function at each iteration point to the column of the dataframe matching the function name and saving the result. Notice the difference before and after we run the loop.


```r
mtcars %>% select(disp, am) %>% head # before
```

```
##                   disp am
## Mazda RX4          160  1
## Mazda RX4 Wag      160  1
## Datsun 710         108  1
## Hornet 4 Drive     258  0
## Hornet Sportabout  360  0
## Valiant            225  0
```

```r
trans <- list( 
  disp = function(x) x * 0.0163871,
  am = function(x) {
    factor(x, labels = c("auto", "manual"))
  }
)
for (var in names(trans)) {
  mtcars[[var]] <- trans[[var]](mtcars[[var]])
}

mtcars %>% select(disp, am) %>% head # after
```

```
##                       disp     am
## Mazda RX4         2.621936 manual
## Mazda RX4 Wag     2.621936 manual
## Datsun 710        1.769807 manual
## Hornet 4 Drive    4.227872   auto
## Hornet Sportabout 5.899356   auto
## Valiant           3.687098   auto
```

## 21.4.1 Exercises

1. Read the documentation for `apply()`. In the 2d case, what two for loops does it generalise?

In the 2d case, we can loop over the rows or colums of a matrix with a function we supply. Note how we can get row sums or column sums by specifying 1 or 2.


```r
x <- cbind(x1 = 3, x2 = c(4:1, 2:5))
col.sums <- apply(x, 2, sum)
row.sums <- apply(x, 1, sum)
rbind(cbind(x, Rtot = row.sums), Ctot = c(col.sums, sum(col.sums)))
```

```
##      x1 x2 Rtot
##       3  4    7
##       3  3    6
##       3  2    5
##       3  1    4
##       3  2    5
##       3  3    6
##       3  4    7
##       3  5    8
## Ctot 24 24   48
```

2. Adapt `col_summary()` so that it only applies to numeric columns. You might want to start with an `is_numeric()` function that returns a logical vector that has a `TRUE` corresponding to each numeric column.

`is_numeric()` is deprecated but the predicate function `keep()` seems to fit this description.


```r
col_summary2 <- function(df, fun) {
  num_df <- keep(df, is.numeric)
  out <- vector("double", length(num_df))
  names(out) <- names(num_df)
  for (i in seq_along(num_df)) {
    out[i] <- fun(num_df[[i]])
  }
  out
}
col_summary2(iris, median)
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width 
##         5.80         3.00         4.35         1.30
```

## 21.5.3 Exercises

1. Write code that uses one of the map functions to:

* Compute the mean of every column in mtcars.


```r
# previous for loop
means <- vector("double", ncol(mtcars))
names(means) <- names(mtcars)
for (i in seq_along(mtcars)) {
    means[[i]] <- mean(mtcars[[i]])
}
```

```
## Warning in mean.default(mtcars[[i]]): argument is not numeric or logical:
## returning NA
```

```r
map_dbl(mtcars, mean) #sapply(mtcars, mean)
```

```
## Warning in mean.default(.x[[i]], ...): argument is not numeric or logical:
## returning NA
```

```
##        mpg        cyl       disp         hp       drat         wt 
##  20.090625   6.187500   3.780862 146.687500   3.596563   3.217250 
##       qsec         vs         am       gear       carb 
##  17.848750   0.437500         NA   3.687500   2.812500
```

* Determine the type of each column in nycflights13::flights.


```r
# previous for loop
types <- vector("character", ncol(flights))
for (i in seq_along(flights)) {
    types[[i]] <- typeof(flights[[i]])
    names(types) <- names(flights)
}

map_chr(flights, typeof) #sapply(flights, typeof)
```

```
##           year          month            day       dep_time sched_dep_time 
##      "integer"      "integer"      "integer"      "integer"      "integer" 
##      dep_delay       arr_time sched_arr_time      arr_delay        carrier 
##       "double"      "integer"      "integer"       "double"    "character" 
##         flight        tailnum         origin           dest       air_time 
##      "integer"    "character"    "character"    "character"       "double" 
##       distance           hour         minute      time_hour 
##       "double"       "double"       "double"       "double"
```

* Compute the number of unique values in each column of iris.


```r
# previous for loop
n_unique_vals <- vector("double", ncol(iris))
for (i in seq_along(iris)) {
    n_unique_vals[[i]] <- length(unique(iris[[i]]))
    names(n_unique_vals) <- names(iris)
}

map_int(iris, ~length(unique(.))) # map_int(iris, function(x) length(unique(x)))
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##           35           23           43           22            3
```

```r
#sapply(iris, function(x) length(unique(x)))
```

* Generate 10 random normals for each of μ = -10, 0, 10, and 100


```r
# previous for loop
mus <- c(-10, 0, 10, 100)
rnorms <- vector("list", length(mus))
for (i in seq_along(mus)) {
    set.seed(1)
    rnorms[[i]] <- rnorm(10, mean = mus[[i]])
    names(rnorms) <- mus
}

set.seed(1)
map(mus, rnorm, n = 10) # mus %>% map(rnorm, n = 10)
```

```
## [[1]]
##  [1] -10.626454  -9.816357 -10.835629  -8.404719  -9.670492 -10.820468
##  [7]  -9.512571  -9.261675  -9.424219 -10.305388
## 
## [[2]]
##  [1]  1.51178117  0.38984324 -0.62124058 -2.21469989  1.12493092
##  [6] -0.04493361 -0.01619026  0.94383621  0.82122120  0.59390132
## 
## [[3]]
##  [1] 10.918977 10.782136 10.074565  8.010648 10.619826  9.943871  9.844204
##  [8]  8.529248  9.521850 10.417942
## 
## [[4]]
##  [1] 101.35868  99.89721 100.38767  99.94619  98.62294  99.58501  99.60571
##  [8]  99.94069 101.10003 100.76318
```

```r
# sapply(mus, rnorm, n = 10) outputs a matrix instead of a list (type inconsistent)
```

2. How can you create a single vector that for each column in a data frame indicates whether or not it’s a factor?


```r
map_lgl(iris, is.factor)
```

```
## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
##        FALSE        FALSE        FALSE        FALSE         TRUE
```

3. What happens when you use the map functions on vectors that aren’t lists? What does `map(1:5, runif)` do? Why?

Generally, `map` applies a function to each element of a vector and returns a vector the same length as the input. If passing a dataframe to a `map()` function, it will iterate over the columns of the dataframe. If passing a list, it iterates through the list. But if passing an atomic vector like in this example, `map()` iterates over each element. 

So the input of the first element is of length 1 and so it returns a generates a random uniform vector of length 1 and so on.


```r
map(1:5, runif)
```

```
## [[1]]
## [1] 0.4346595
## 
## [[2]]
## [1] 0.7125147 0.3999944
## 
## [[3]]
## [1] 0.3253522 0.7570871 0.2026923
## 
## [[4]]
## [1] 0.7111212 0.1216919 0.2454885 0.1433044
## 
## [[5]]
## [1] 0.23962942 0.05893438 0.64228826 0.87626921 0.77891468
```

4. What does `map(-2:2, rnorm, n = 5)` do? Why? What does `map_dbl(-2:2, rnorm, n = 5)` do? Why?

`map(-2:2, rnorm, n = 5)` generates a list of vectors of random normals of length 5 with means varying from -2 to 2. 

`map_dbl(-2:2, rnorm, n = 5)` fails because result 1 is not a length 1 atomic vector. map_dbl expects the function to return a numeric vector of length one.


```r
map(-2:2, rnorm, n = 5)
```

```
## [[1]]
## [1] -1.1679529 -2.2273287 -1.7338626 -2.3767027  0.4413646
## 
## [[2]]
## [1] -1.7953391 -1.0548775 -0.7498587 -0.3817567 -1.1726235
## 
## [[3]]
## [1] -2.22390027 -1.26361438  0.35872890 -0.01104548 -0.94064916
## 
## [[4]]
## [1]  0.8841747  0.1850313  1.2422635 -0.4250984  1.3659411
## 
## [[5]]
## [1] 2.248413 2.065288 2.019156 2.257338 1.350990
```

`map_dbl(-2:2, rnorm, n = 5)` fails because result 1 is not a length 1 atomic vector. `map_dbl()`, unlike `map()`, expects the function to return a numeric vector of length one.


```r
map_dbl(-2:2, rnorm, n = 5)
```

```
## Error: Result 1 is not a length 1 atomic vector
```

We could however use `flatten_dbl()` on the previous result from `map()` to combine the list of 5 elements into a single vector.


```r
flatten_dbl(map(-2:2, rnorm, n = 5))
```

```
##  [1] -2.912068367 -3.437586241 -2.797089525 -0.745916894 -1.227857814
##  [6] -1.219515627 -1.424810283 -1.418980099 -0.003013139 -1.275778029
## [11]  1.256018817  0.646674390  1.299312303 -0.873262112  0.008370960
## [16]  0.119128277  1.596259017  1.119717641  0.717826123  2.455988401
## [21]  2.229019591  2.996543929  2.781859185  1.223223378  1.384010092
```

5. Rewrite `map(x, function(df) lm(mpg ~ wt, data = df))` to eliminate the anonymous function.


```r
map(x, function(df) lm(mpg ~ wt, data = df))
map(x, ~ lm(mpg ~ wt, data = .))
```

## 21.9.3 Exercises

1. Implement your own version of `every()` using a for loop. Compare it with `purrr::every()`. What does purrr’s version do that your version doesn’t?

`purrr::every()` gives a logical test if every element of a list satisfies a predicate. For instance:


```r
y <- list(0:10, 5.5)
y %>% every(is.integer)
```

```
## [1] FALSE
```

Here's my own version using a for loop.


```r
my_every <- function(x, f) {
    v_lgl <- vector("logical", length(x))
    names(v_lgl) <- names(x)
    for (i in seq_along(x)) {
        v_lgl[[i]] <- f(x[[i]])
    }
    all(v_lgl)
}
```

It works for many cases.


```r
identical(every(y, is.integer), my_every(y, is.integer))
```

```
## [1] TRUE
```

```r
identical(every(iris, is.numeric), my_every(iris, is.numeric))
```

```
## [1] TRUE
```

```r
identical(every(list(), mean), my_every(list(), mean))
```

```
## [1] TRUE
```

But I'm sure there are edge cases that will fail. For instance,


```r
x <- list(0, 1, TRUE)
x %>% every(identity)
```

```
## [1] TRUE
```

```r
x %>% my_every(identity)
```

```
## Warning in all(v_lgl): coercing argument of type 'double' to logical
```

```
## [1] FALSE
```

2. Create an enhanced `col_sum()` that applies a summary function to every numeric column in a data frame.


```r
my_col_sum <- function(df, f, ...) {
    num_df <- keep(df, is.numeric)
    map(num_df, f)
}
my_col_sum(iris, summary)
```

```
## $Sepal.Length
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   4.300   5.100   5.800   5.843   6.400   7.900 
## 
## $Sepal.Width
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   2.000   2.800   3.000   3.057   3.300   4.400 
## 
## $Petal.Length
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.000   1.600   4.350   3.758   5.100   6.900 
## 
## $Petal.Width
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   0.100   0.300   1.300   1.199   1.800   2.500
```

```r
my_col_sum(iris, median)
```

```
## $Sepal.Length
## [1] 5.8
## 
## $Sepal.Width
## [1] 3
## 
## $Petal.Length
## [1] 4.35
## 
## $Petal.Width
## [1] 1.3
```

3. A possible base R equivalent of `col_sum()` is:


```r
col_sum3 <- function(df, f) {
  is_num <- sapply(df, is.numeric)
  df_num <- df[, is_num]

  sapply(df_num, f)
}
```

But it has a number of bugs as illustrated with the following inputs:


```r
df <- tibble(
  x = 1:3, 
  y = 3:1,
  z = c("a", "b", "c")
)
# OK
col_sum3(df, mean)
```

```
## x y 
## 2 2
```

```r
# Has problems: don't always return numeric vector
col_sum3(df[1:2], mean)
```

```
## x y 
## 2 2
```

```r
col_sum3(df[1], mean)
```

```
## x 
## 2
```

```r
col_sum3(df[0], mean)
```

```
## Error: Unsupported index type: list
```

What causes the bugs?

The problem is that `sapply()` is type inconsistent. It doesn't always return the same type of output. It could be a matrix or a vector. And in the last call, when the input is of length 0, calling `sapply(df[0], is.numeric)` results in an empty list, causing the function to fail in the next line.
