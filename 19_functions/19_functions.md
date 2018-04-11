---
title: "19 Functions Exercises Solutions"
author: "Sean Angiolillo"
date: "29 March 2018"
output: 
  html_document: 
    keep_md: yes
---



## 19.2.1 Practice

1. Why is `TRUE` not a parameter to `rescale01()`? What would happen if `x` contained a single missing value, and `na.rm` was `FALSE`?

Rather than a parameter to `rescale01()`, the function is written with `na.rm = TRUE` as an argument to the `range()` function.


```r
rescale01 <- function(x) {
  rng <- range(x, na.rm = TRUE)
  (x - rng[1]) / (rng[2] - rng[1])
}
rescale01(c(0, 5, 10))
```

```
## [1] 0.0 0.5 1.0
```

If na.rm was `FALSE`, then all values would be returned as `NA` because both range values would be `NA` as seen below.


```r
rescale02 <- function(x) {
  rng <- range(x, na.rm = FALSE)
  (x - rng[1]) / (rng[2] - rng[1])
}
rescale02(c(0, 5, 10, NA))
```

```
## [1] NA NA NA NA
```

2. In the second variant of `rescale01()`, infinite values are left unchanged. Rewrite `rescale01()` so that `-Inf` is mapped to 0, and `Inf` is mapped to 1.

We could use an `if` statement to check for infinite values first but it seems just as easy to reset their values after rescaling the values.


```r
x <- c(1:5, -Inf, Inf)
rescale01 <- function(x) {
  rng <- range(x, na.rm = TRUE, finite = TRUE)
  x <- (x - rng[1]) / (rng[2] - rng[1])
  x[x == Inf] <- 1
  x[x == -Inf] <- 0
  x
}
rescale01(x)
```

```
## [1] 0.00 0.25 0.50 0.75 1.00 0.00 1.00
```

3. Practice turning the following code snippets into functions. Think about what each function does. What would you call it? How many arguments does it need? Can you rewrite it to be more expressive or less duplicative?


```r
x <- c(0:4, NA)

#mean(is.na(x))
prop_na <- function(x) { mean(is.na(x)) }
prop_na(x)
```

```
## [1] 0.1666667
```

```r
#x / sum(x, na.rm = TRUE)
get_weights <- function(x) {x / sum(x, na.rm = TRUE) }
get_weights(x)
```

```
## [1] 0.0 0.1 0.2 0.3 0.4  NA
```

```r
#sd(x, na.rm = TRUE) / mean(x, na.rm = TRUE)
coef_variation <- function(x) {
    x <- x[!is.na(x)]
    sd(x)/mean(x)
}
coef_variation(x)
```

```
## [1] 0.7905694
```

4. Follow http://nicercode.github.io/intro/writing-functions.html to write your own functions to compute the variance and skew of a numeric vector.


```r
set.seed(1)
x <- rnorm(10)
variance <- function(x) {
    n <- length(x)
    m <- mean(x)
    (1/(n - 1)) * sum((x - m)^2)
}
dplyr::near(variance(x), var(x))
```

```
## [1] TRUE
```

```r
set.seed(1)
x <- rnorm(10)
skewness <- function(x) {
    n <- length(x)
    v <- var(x)
    m <- mean(x)
    third.moment <- (1/(n - 2)) * sum((x - m)^3)
    third.moment/(var(x)^(3/2))
}

skewness(x)
```

```
## [1] 0.3161183
```

5. Write `both_na()`, a function that takes two vectors of the same length and returns the number of positions that have an `NA` in both vectors.


```r
x <- c(0, NA, 1, NA, 2, NA)
y <- c(5, NA, 2, 0, NA, NA)

both_na <- function(x,y) {
    if (length(x) == length(y)) {
        x_na <- which(is.na(x))
        y_na <- which(is.na(y))
        length(intersect(x_na, y_na))
    } else {
        print("Vectors need to be the same length")
    }
} 
```

We can write a much shorter function if we take advantage of logical subsetting.


```r
both_na <- function(x, y) {
    if (length(x) == length(y)) {
        sum(is.na(x) & is.na(y))
    } else {
        print("Vectors need to be the same length")
    }
}
both_na(x,y)
```

```
## [1] 2
```

6. What do the following functions do? Why are they useful even though they are so short?

`is_directory()` checks if a file path is a directory. `is_readable()` checks if a file path is readable. They're useful because the names make it easier to see what they do.  


```r
x <- "./19_functions/19_functions.Rmd"
y <- "19_functions"
is_directory <- function(x) file.info(x)$isdir
is_readable <- function(x) file.access(x, 4) == 0

is_directory(x)
```

```
## [1] NA
```

```r
is_directory(y)
```

```
## [1] NA
```

```r
is_readable(dir("."))
```

```
## 19_functions.html  19_functions.Rmd 
##              TRUE              TRUE
```

7. Read the complete lyrics to “Little Bunny Foo Foo”. There’s a lot of duplication in this song. Extend the initial piping example to recreate the complete song, and use functions to reduce the duplication.

## 19.3.1 Exercises

1. Read the source code for each of the following three functions, puzzle out what they do, and then brainstorm better names.

`f1` returns `TRUE` if the second argument matches the beginning of the first argument. `matches_prefix` would be a better name.


```r
# matches_prefix
f1 <- function(string, prefix) {
  substr(string, 1, nchar(prefix)) == prefix
}
f1("antecedent", "ante")
```

```
## [1] TRUE
```

```r
f1("reassert", "pre")
```

```
## [1] FALSE
```

```r
f1("The dog", "The")
```

```
## [1] TRUE
```

`f2` takes a numeric vector and if a greater length than 1, it removes the last element and returns the vector. A better name could be `chop_last`.

```r
# chop_last
f2 <- function(x) {
  if (length(x) <= 1) return(NULL)
  x[-length(x)]
}
f2(1:5)
```

```
## [1] 1 2 3 4
```

`f3` takes numeric vectors and creates a sequence of the second vector that repeats each element in the second vector so the new vector is the length of the first. It's not the most succinct but a new name could be `fill_y_with_length_x`.


```r
# fill_y_with_length_x
f3 <- function(x, y) {
  rep(y, length.out = length(x))
}
f3(1:5, 3:4)
```

```
## [1] 3 4 3 4 3
```

2. Take a function that you’ve written recently and spend 5 minutes brainstorming a better name for it and its arguments.

3. Compare and contrast `rnorm()` and `MASS::mvrnorm()`. How could you make them more consistent?

`rnorm()` generates a vector of random numbers from the normal distribution. `n` is a mandatory argument but `mean` and `sd` have defaults of 0 and 1. `MASS::mvrnorm()` generates a matrix of samples from a specified multivariate normal distribution. `n` defaults to 1, but its arguments are the Greek parameters `mu` and `Sigma`, which have no defaults. Consistency would require the argument names to match but I'm not sure this is necessarily a good idea given the difference in what the multivariate normal distribution represents.

4. Make a case for why `norm_r()`, `norm_d()` etc would be better than `rnorm()`, `dnorm()`. Make a case for the opposite.

Having `norm_*()` would enable easier searching for the d, p, q, and r functions for the normal distribution. The functions in the `stats` package however works the opposite way. Functions are grouped by the action first and then the distribution. First you think of the action you want to execuite (e.g. generate random numbers) and then choose the distribution (normal, Poisson, uniform, etc). While functions like `runif()`, `rnorm()` and `rpois()` will be together, having only one letter won't be enough to pull up the other functions in the way that having the distribution first would.

## 19.4.4 Exercises

1. What’s the difference between `if` and `ifelse()`? Carefully read the help and construct three examples that illustrate the key differences.

The key difference I see is that `ifelse()` is vectorized whereas a simple `if` statement is not. Because it must evaluate to a single `TRUE` or `FALSE`, an `if` statement therefore would likely be found in a for loop or a long conditional tree.

`ifelse()` on the other hand returns a value with the same shape as the supplied test argument depending on whether each element is `TRUE` or `FALSE`. It can take a vector like a column from a dataframe, which makes it easy to mutate new columns. For example,


```r
library(dplyr)
mtcars %>%
    mutate(heavy = ifelse(wt > 3, TRUE, FALSE)) %>%
    select(wt, heavy) %>% head
```

```
##      wt heavy
## 1 2.620 FALSE
## 2 2.875 FALSE
## 3 2.320 FALSE
## 4 3.215  TRUE
## 5 3.440  TRUE
## 6 3.460  TRUE
```

We could try something similar with if but we'd get an error if we pass a vector to an if statement. It needs to evaluate to a single logical statement.


```r
if (mtcars$wt > 3) {
    print(TRUE)
} else {
    print(FALSE)
}
```

2. Write a greeting function that says “good morning”, “good afternoon”, or “good evening”, depending on the time of day. (Hint: use a time argument that defaults to `lubridate::now()`. That will make it easier to test your function.)


```r
greeting <- function(time = lubridate::now()) {
    hour <- lubridate::hour(time)
    if (hour > 4 & hour < 12) {
        print("good morning")
    } else if (hour >= 12 & hour < 19) {
        print("good afternoon")
    } else {
        print("good evening")
    }
}
greeting()
```

```
## [1] "good afternoon"
```

```r
greeting("2018-01-01 08:00:00")
```

```
## [1] "good morning"
```

```r
greeting("2018-01-01 13:00:00")
```

```
## [1] "good afternoon"
```

```r
greeting("2018-01-01 22:00:00")
```

```
## [1] "good evening"
```


3. Implement a `fizzbuzz` function. It takes a single number as input. If the number is divisible by three, it returns “fizz”. If it’s divisible by five it returns “buzz”. If it’s divisible by three and five, it returns “fizzbuzz”. Otherwise, it returns the number. Make sure you first write working code before you create the function.


```r
fizzbuzz <- function(x) {
    if (length(x) == 1 && class(x) == "numeric") {
        if (x %% 3 == 0 && x %% 5 == 0) {
            print("fizzbuzz")
        } else if (x %% 3 == 0) {
            print("fizz")
        } else if (x %% 5 == 0) {
            print("buzz")
        } else {
            print(x)
        }
    } else {
        print("Input must be a single number")
    }
}
fizzbuzz(15)
```

```
## [1] "fizzbuzz"
```

```r
fizzbuzz(5)
```

```
## [1] "buzz"
```

```r
fizzbuzz(33)
```

```
## [1] "fizz"
```

```r
fizzbuzz(c(3,5))
```

```
## [1] "Input must be a single number"
```

```r
fizzbuzz("text")
```

```
## [1] "Input must be a single number"
```

We could have also checked values with `stopifnot`, which throws an error when `FALSE`.


```r
fizzbuzz <- function(x) {
        stopifnot(length(x) == 1 && class(x) == "numeric")
        if (x %% 3 == 0 && x %% 5 == 0) {
            print("fizzbuzz")
        } else if (x %% 3 == 0) {
            print("fizz")
        } else if (x %% 5 == 0) {
            print("buzz")
        } else {
            print(x)
        }
}
fizzbuzz(c(3,5))
```

```
## Error: length(x) == 1 && class(x) == "numeric" is not TRUE
```

4. How could you use `cut()` to simplify this set of nested if-else statements?


```r
if (temp <= 0) {
  "freezing"
} else if (temp <= 10) {
  "cold"
} else if (temp <= 20) {
  "cool"
} else if (temp <= 30) {
  "warm"
} else {
  "hot"
}
```

`cut()` breaks numerics into factors and so is quite useful here.


```r
tibble(temp = seq(-10, 40, by = 5),
      category = cut(temp, breaks = c(-Inf, 0, 10, 20, 30, Inf),
                     labels = c("freezing", "cold", "cool", "warm", "hot"),
                     right = TRUE)) # if <=
```

```
## # A tibble: 11 x 2
##     temp category
##    <dbl> <fct>   
##  1  -10. freezing
##  2   -5. freezing
##  3    0. freezing
##  4    5. cold    
##  5   10. cold    
##  6   15. cool    
##  7   20. cool    
##  8   25. warm    
##  9   30. warm    
## 10   35. hot     
## 11   40. hot
```

How would you change the call to `cut()` if I’d used `<` instead of `<=`? What is the other chief advantage of `cut()` for this problem? (Hint: what happens if you have many values in `temp`?)

Set `right = FALSE` in `cut()`. The other big advantage is that `cut()` is vectorized whereas using conditional statements would check one number at a time.

5. What happens if you use `switch()` with numeric values?

If we use a numeric value in place of the expression, it will return the list element at that position.


```r
switch(2,"red","green","blue")
```

```
## [1] "green"
```

6. What does this `switch()` call do? What happens if `x` is “e”? Experiment, then carefully read the documentation.

Inputs of `a` and `b` return "ab"; `c` and `d` return "cd". "e" (and many other inputs) don't seem to return anything. 


```r
x <- ""
switch(x, 
  a = ,
  b = "ab",
  c = ,
  d = "cd"
)
```

## 19.5.5 Exercises

1. What does `commas(letters, collapse = "-")` do? Why?


```r
commas <- function(...) stringr::str_c(..., collapse = ", ")
commas(letters[1:10])
```

```
## [1] "a, b, c, d, e, f, g, h, i, j"
```

The function `commas` uses `...` to forward on any arguments not dealt with in the function body. But because the argument of collapse is provided again in the call to `commas` we get an error. The formal argument "collapse" is matched by multiple actual arguments. It would be the same as calling `stringr:str_c(letters, collapse = ", ", collapse = "-")`.

2. It’d be nice if you could supply multiple characters to the `pad` argument, e.g. `rule("Title", pad = "-+")`. Why doesn’t this currently work? How could you fix it?


```r
rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  cat(title, " ", stringr::str_dup(pad, width), "\n", sep = "")
}
rule("Important output")
```

```
## Important output ------------------------------------------------------
```

If we set pad to more than one character (as seen below), the output is more than one line because `width` implicitly assumes pad is one character.


```r
rule("Title", pad = "-+")
```

```
## Title -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

We can fix this by requiring the function to take account of the number of characters in the `pad` argument into its width calculation.


```r
rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  pad_char <- nchar(pad)
  cat(title, " ", stringr::str_dup(pad, width %/% pad_char), "\n", sep = "")
}
rule("Title", pad = "-+")
```

```
## Title -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

3. What does the `trim` argument to `mean()` do? When might you use it?

The `trim` argument is the fraction of observations to be trimmed from each end of x before the mean is computed. Values of trim outside that range are taken as the nearest endpoint. So if `trim` is non-zero, a symmetrically trimmed mean is computed with a fraction of 'trimmed' observations deleted from each end before the mean is computed. This would be useful if you want to calculate a robust mean (i.e. one with outliers removed).


```r
x <- c(50, 0:2)
mean(x, trim = 0)
```

```
## [1] 13.25
```

```r
mean(x, trim = 0.3)
```

```
## [1] 1.5
```

4. The default value for the `method` argument to `cor()` is `c("pearson", "kendall", "spearman")`. What does that mean? What value is used by default?

Any of the three strings can be used, but pearson is default because it is first.


```r
cor(mtcars$wt, mtcars$mpg) # pearson by default
```

```
## [1] -0.8676594
```

```r
cor(mtcars$wt, mtcars$mpg, method = "kendall")
```

```
## [1] -0.7278321
```

```r
cor(mtcars$wt, mtcars$mpg, method = "spearman")
```

```
## [1] -0.886422
```

