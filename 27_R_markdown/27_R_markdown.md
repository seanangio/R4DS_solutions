---
title: "27 R Markdown Exercise Solutions"
author: "Sean Angiolillo"
date: "4/1/2018"
output: 
  html_document: 
    keep_md: yes
---



## 27.2.1 Exercises

1. Create a new notebook using `File > New File > R Notebook`. Read the instructions. Practice running the chunks. Verify that you can modify the code, re-run it, and see modified output.

NA

2. Create a new R Markdown document with `File > New File > R Markdown…` Knit it by clicking the appropriate button. Knit it by using the appropriate keyboard short cut. Verify that you can modify the input and see the output update.

NA

3. Compare and contrast the R notebook and R markdown files you created above. How are the outputs similar? How are they different? How are the inputs similar? How are they different? What happens if you copy the YAML header from one to the other?

By default, my YAML header for an R Markdown document includes author and date fields in addition to title and output. The important difference though is that the output field for an Rmd is html_document vs html_notebook for an R Notebook. Changing the output type can change the file from an Rmd to a notebook and vice versa.

Essentially an R notebook is an Rmd but its chunks can be executed independently and interactively. You'll notice in the top right corner of the html notebook an option to show or hide code. You can also run chunks on demand. Creating a notebook automatically renders a knit version, whereas a rmd has to be knit for html output.

4. Create one new R Markdown document for each of the three built-in formats: HTML, PDF and Word. Knit each of the three documents. How does the output differ? How does the input differ? (You may need to install LaTeX in order to build the PDF output — RStudio will prompt you if this is necessary.)

Formatting-- fonts, colors, spacing, centering, page breaks-- seems to be the key difference in output. The input changes are found in the YAML header. For example,

output:

  - html_document: default
  
  - pdf_document: default
  
  - word_document: default

## 27.3.1 Exercises

1. Practice what you’ve learned by creating a brief CV. The title should be your name, and you should include headings for (at least) education or employment. Each of the sections should include a bulleted list of jobs/degrees. Highlight the year in bold.

NA

2. Using the R Markdown quick reference, figure out how to:

* Add a footnote: 

Mark a footnote like this [^1]

[^1]: And here it is.

* Add a horizontal rule: Three asterisks (***) at the beginning of a line will do it.

* Add a block quote: 

> Check out this block quote!

* Copy and paste the contents of diamond-sizes.Rmd from https://github.com/hadley/r4ds/tree/master/rmarkdown in to a local R markdown document. Check that you can run it, then add text after the frequency polygon that describes its most striking features.

NA

## 27.4.7 Exercises

1. Add a section that explores how diamond sizes vary by cut, colour, and clarity. Assume you’re writing a report for someone who doesn’t know R, and instead of setting `echo = FALSE` on each chunk, set a global option.

Set a global option at the top of the document like this below:

\```{r include=FALSE}
knitr::opts_chunk$set(
  echo = FALSE
)
\```

2. Download `diamond-sizes.Rmd` from https://github.com/hadley/r4ds/tree/master/rmarkdown. Add a section that describes the largest 20 diamonds, including a table that displays their most important attributes.

Here's an example using `knitr::kable`.


```r
library(tidyverse)
knitr::kable(
    diamonds %>%
        arrange(-carat) %>%
        head(20)
)
```



 carat  cut         color   clarity    depth   table   price       x       y      z
------  ----------  ------  --------  ------  ------  ------  ------  ------  -----
  5.01  Fair        J       I1          65.5      59   18018   10.74   10.54   6.98
  4.50  Fair        J       I1          65.8      58   18531   10.23   10.16   6.72
  4.13  Fair        H       I1          64.8      61   17329   10.00    9.85   6.43
  4.01  Premium     I       I1          61.0      61   15223   10.14   10.10   6.17
  4.01  Premium     J       I1          62.5      62   15223   10.02    9.94   6.24
  4.00  Very Good   I       I1          63.3      58   15984   10.01    9.94   6.31
  3.67  Premium     I       I1          62.4      56   16193    9.86    9.81   6.13
  3.65  Fair        H       I1          67.1      53   11668    9.53    9.48   6.38
  3.51  Premium     J       VS2         62.5      59   18701    9.66    9.63   6.03
  3.50  Ideal       H       I1          62.8      57   12587    9.65    9.59   6.03
  3.40  Fair        D       I1          66.8      52   15964    9.42    9.34   6.27
  3.24  Premium     H       I1          62.1      58   12300    9.44    9.40   5.85
  3.22  Ideal       I       I1          62.6      55   12545    9.49    9.42   5.92
  3.11  Fair        J       I1          65.9      57    9823    9.15    9.02   5.98
  3.05  Premium     E       I1          60.9      58   10453    9.26    9.25   5.66
  3.04  Very Good   I       SI2         63.2      59   15354    9.14    9.07   5.75
  3.04  Premium     I       SI2         59.3      60   18559    9.51    9.46   5.62
  3.02  Fair        I       I1          65.2      56   10577    9.11    9.02   5.91
  3.01  Premium     I       I1          62.7      58    8040    9.10    8.97   5.67
  3.01  Premium     F       I1          62.2      56    9925    9.24    9.13   5.73

3. Modify `diamonds-sizes.Rmd` to use `comma()` to produce nicely formatted output. Also include the percentage of diamonds that are larger than 2.5 carats.

See below:


```r
comma <- function(x) format(x, digits = 2, big.mark = ",") # as given in text
smaller <- diamonds %>% filter(carat <= 2.5)
```

We have data about 53,940 diamonds. Only 
126 are larger than
2.5 carats. This is only 0.0023% of the dataset. 

4. Set up a network of chunks where d depends on c and b, and both b and c depend on a. Have each chunk print lubridate::now(), set cache = TRUE, then verify your understanding of caching.

[This](https://raw.githubusercontent.com/jrnold/r4ds-exercise-solutions/master/rmarkdown/caching.Rmd) is the best explanation I can find. But it seems to me that, even without setting the dependencies in this example, the same value is cached so it's not a problem. What I haven't fully worked out is reliably returning different cached values based on dependencies.
