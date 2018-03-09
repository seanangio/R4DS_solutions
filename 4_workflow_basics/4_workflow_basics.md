---
title: "Workflow:basics Solutions"
author: "Sean Angiolillo"
date: "9 March 2018"
output: 
  html_document: 
    keep_md: yes
---



## 4.4 Workflow:basics Practice

1. Why does this code not work?


```r
my_variable <- 10
my_varıable
#> Error in eval(expr, envir, enclos): object 'my_varıable' not found
```

Look carefully! (This may seem like an exercise in pointlessness, but training your brain to notice even the tiniest difference will pay off when programming.)

The object is spelled wrong.

2. Tweak each of the following R commands so that they run correctly:


```r
library(tidyverse)

ggplot(dota = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))

fliter(mpg, cyl = 8)
filter(diamond, carat > 3)
```

`ggplot(dota = mpg)` should be `ggplot(data = mpg)`
`fliter(mpg, cyl = 8)` should be `filter(mpg, cyl == 8)`
`filter(diamond, carat > 3)` should be `filter(diamonds, carat > 3)`


3. Press Alt + Shift + K. What happens? How can you get to the same place using the menus?

Pressing Alt + Shift + K brings up the keyboard shortcut quick reference. Using the menus, go to Help and find "Keyboard Shortcuts Help".
