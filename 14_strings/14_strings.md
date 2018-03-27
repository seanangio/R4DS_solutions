---
title: "14 Strings Exercises Solutions"
author: "Sean Angiolillo"
date: "24 March 2018"
output: 
  html_document: 
    keep_md: yes
---




```r
library(tidyverse)
library(stringr)
```


## 14.2.5 Exercises

1. In code that doesn’t use `stringr`, you’ll often see `paste()` and `paste0()`. What’s the difference between the two functions? What `stringr` function are they equivalent to? How do the functions differ in their handling of NA?

`paste()` has a default `sep = " "` whereas `paste0()` does not, as seen below.


```r
x <- "R for"
y <- "Data Science"
paste(x,y)
```

```
## [1] "R for Data Science"
```

```r
paste0(x,y)
```

```
## [1] "R forData Science"
```

They are equivalent to the `stringr` function `str_c`, which by default has `sep = ""`.


```r
str_c(x,y) # mimic paste0()
```

```
## [1] "R forData Science"
```

```r
str_c(x,y, sep = " ") # mimic paste()
```

```
## [1] "R for Data Science"
```

`str_c` treats `NA` values as contagious (i.e. it handles `NA` values like `mean()` or `sum()`) whereas `paste()` and `paste0()` treats `NA` like any other string.


```r
x <- c("string")
y <- NA
str_c(x,y)
```

```
## [1] NA
```

```r
paste(x,y)
```

```
## [1] "string NA"
```

```r
paste0(x,y)
```

```
## [1] "stringNA"
```

2. In your own words, describe the difference between the `sep` and `collapse` arguments to `str_c()`.

By default `collapse` is set to NULL, so `str_c()` concatenates strings element-wise and recyling vectors where necessary. In these cases, the `sep` argument specifies a string to insert between input vecotrs. For example,


```r
# vectors recycled, sep = ","
a <- c("string1", "string2", "string3")
b <- c("string4")
str_c(a,b, sep = ",")
```

```
## [1] "string1,string4" "string2,string4" "string3,string4"
```

```r
length(str_c(a,b, sep = ","))
```

```
## [1] 3
```

The `collapse` argument can be set to combine the input vectors into a single string of length 1. The `sep` argument still specifies a string to insert between the vectors. See below.


```r
str_c(a,b, sep = "SEP", collapse = "COL")
```

```
## [1] "string1SEPstring4COLstring2SEPstring4COLstring3SEPstring4"
```

```r
length(str_c(a,b, sep = "SEP", collapse = "COL"))
```

```
## [1] 1
```

3. Use `str_length()` and `str_sub()` to extract the middle character from a string. What will you do if the string has an even number of characters?

If we apply the `ceiling()` function to round up the `str_length()` of a string, we can use that result as both the start and end argument to `str_sub()` to extract the middle character from a string. If it's an even string, this approach would give us the character before the middle point. We can add one to get the character after the middle point.


```r
odds <- c("odd","fiver")
evens <- c("even","steven")

str_sub(odds, start = ceiling(str_length(odds)/2), end = ceiling(str_length(odds)/2))
```

```
## [1] "d" "v"
```

```r
str_sub(evens, start = ceiling(str_length(evens)/2), end = ceiling(str_length(evens)/2))
```

```
## [1] "v" "e"
```

```r
str_sub(evens, start = str_length(evens)/2 + 1, end = str_length(evens)/2 + 1)
```

```
## [1] "e" "v"
```

4. What does `str_wrap()` do? When might you want to use it?

`str_wrap()` wraps strings into nicely formatted paragraphs. You can specify arguments like width and indent. It's useful in particular when printing a lot of text to the console in conjunction with `cat()` and you want it formatted in a more readable way. See the example below from the documentation.


```r
thanks_path <- file.path(R.home("doc"), "THANKS")
thanks <- str_c(readLines(thanks_path), collapse = "\n")
thanks <- word(thanks, 1, 3, fixed("\n\n"))
thanks # difficult to read
```

```
## [1] "R would not be what it is today without the invaluable help of these\npeople, who contributed by donating code, bug fixes and documentation:\n\nValerio Aimale, Thomas Baier, Henrik Bengtsson, Roger Bivand,\nBen Bolker, David Brahm, G\"oran Brostr\"om, Patrick Burns, Vince Carey,\nSaikat DebRoy, Matt Dowle, Brian D'Urso, Lyndon Drake, Dirk Eddelbuettel,\nClaus Ekstrom, Sebastian Fischmeister, John Fox, Paul Gilbert,\nYu Gong, Gabor Grothendieck, Frank E Harrell Jr, Torsten Hothorn,\nRobert King, Kjetil Kjernsmo, Roger Koenker, Philippe Lambert,\nJan de Leeuw, Jim Lindsey, Patrick Lindsey, Catherine Loader,\nGordon Maclean, John Maindonald, David Meyer, Ei-ji Nakama,\nJens Oehlschaegel, Steve Oncley, Richard O'Keefe, Hubert Palme,\nRoger D. Peng, Jose' C. Pinheiro, Tony Plate, Anthony Rossini,\nJonathan Rougier, Petr Savicky, Guenther Sawitzki, Marc Schwartz,\nArun Srinivasan, Detlef Steuer, Bill Simpson, Gordon Smyth, Adrian Trapletti,\nTerry Therneau, Rolf Turner, Bill Venables, Gregory R. Warnes,\nAndreas Weingessel, Morten Welinder, James Wettenhall, Simon Wood, and\nAchim Zeileis.\n\nOthers have written code that has been adopted by R and is\nacknowledged in the code files, including"
```

```r
cat(str_wrap(thanks), "\n") # much easier to read
```

```
## R would not be what it is today without the invaluable help of these people,
## who contributed by donating code, bug fixes and documentation: Valerio Aimale,
## Thomas Baier, Henrik Bengtsson, Roger Bivand, Ben Bolker, David Brahm, G"oran
## Brostr"om, Patrick Burns, Vince Carey, Saikat DebRoy, Matt Dowle, Brian D'Urso,
## Lyndon Drake, Dirk Eddelbuettel, Claus Ekstrom, Sebastian Fischmeister, John
## Fox, Paul Gilbert, Yu Gong, Gabor Grothendieck, Frank E Harrell Jr, Torsten
## Hothorn, Robert King, Kjetil Kjernsmo, Roger Koenker, Philippe Lambert, Jan
## de Leeuw, Jim Lindsey, Patrick Lindsey, Catherine Loader, Gordon Maclean, John
## Maindonald, David Meyer, Ei-ji Nakama, Jens Oehlschaegel, Steve Oncley, Richard
## O'Keefe, Hubert Palme, Roger D. Peng, Jose' C. Pinheiro, Tony Plate, Anthony
## Rossini, Jonathan Rougier, Petr Savicky, Guenther Sawitzki, Marc Schwartz, Arun
## Srinivasan, Detlef Steuer, Bill Simpson, Gordon Smyth, Adrian Trapletti, Terry
## Therneau, Rolf Turner, Bill Venables, Gregory R. Warnes, Andreas Weingessel,
## Morten Welinder, James Wettenhall, Simon Wood, and Achim Zeileis. Others have
## written code that has been adopted by R and is acknowledged in the code files,
## including
```

5. What does `str_trim()` do? What’s the opposite of `str_trim()`?

`str_trim()` removes whitespace from a string. You can specify which sides of strings to be trimmed.


```r
whitespace <- "  whitespace  "
no_whitespace <- str_trim(whitespace, side = "both")
no_whitespace
```

```
## [1] "whitespace"
```

The opposite is `str_pad()`. You have to specify a width argument that is the minimum width of strings to be padded. So you are specifying the total length of the string after padding rather than how much padding is to be added. Strings longer than this given threshold won't be affected. 


```r
no_whitespace <- "whitespace"
str_pad(no_whitespace, width = 12, side = "both")
```

```
## [1] " whitespace "
```

```r
str_pad("string is too long for padding", width = 12, side = "both")
```

```
## [1] "string is too long for padding"
```

6. Write a function that turns (e.g.) a vector `c("a", "b", "c")` into the string "a, b, and c". Think carefully about what it should do if given a vector of length 0, 1, or 2.

This gives the desired outputs.


```r
my_func <- function(x, sep = ", ", ending = ", and ") {
  if (length(x) > 2) {
    str_c(str_c(x[-length(x)], collapse = sep),
                x[length(x)],
                sep = ending)
  } else if (length(x) == 2) {
      str_c(c("a","b"), collapse = " and ")
  } else {
    x
  }
}

my_func(c("a", "b", "c"))
```

```
## [1] "a, b, and c"
```

```r
my_func(c("a", "b"))
```

```
## [1] "a and b"
```

```r
my_func("a")
```

```
## [1] "a"
```

```r
my_func("")
```

```
## [1] ""
```

## 14.3.1.1 Exercises

1. Explain why each of these strings don’t match a \: "\", "\\", "\\\".

As given in the example the way to match a \ is


```r
x <- "a\\b"
str_view(x, "\\\\")
```

<!--html_preserve--><div id="htmlwidget-25b902f68fbb54b9924a" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-25b902f68fbb54b9924a">{"x":{"html":"<ul>\n  <li>a<span class='match'>\\<\/span>b<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

Counting from the end, the second backslash is to escape the literal in order to create the regular expression. To represent that regular expression as a string, another backslash is needed, which also needs to be escaped (bringing the total to 4).

2. How would you match the sequence "'\?

The first challenge is representing the given sequence as a string and secondly as a regular expression.


```r
str_view("\"'\\", "\"'\\\\")
```

<!--html_preserve--><div id="htmlwidget-843bfdcf33299b465248" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-843bfdcf33299b465248">{"x":{"html":"<ul>\n  <li><span class='match'>\"'\\<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

3. What patterns will the regular expression \..\..\.. match? How would you represent it as a string?

To represent it as a string, we need to add a escape each backslash and place it inside quotes. It matches dot, something, dot, something, dot, something.


```r
str_view(c(".a.b.c", ".a.b", "..a..b..c.."), "\\..\\..\\..")
```

<!--html_preserve--><div id="htmlwidget-e2945be357c33f7df9bd" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-e2945be357c33f7df9bd">{"x":{"html":"<ul>\n  <li><span class='match'>.a.b.c<\/span><\/li>\n  <li>.a.b<\/li>\n  <li>..a..b..c..<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## 14.3.2.1 Exercises

1. How would you match the literal string "$^$"?

Each of the special characters needs to be escaped with backslashes (which also need to be escaped).


```r
str_view(c("$$^$", "$^$"), "^\\$\\^\\$$")
```

<!--html_preserve--><div id="htmlwidget-d9a1a91d35d087258140" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-d9a1a91d35d087258140">{"x":{"html":"<ul>\n  <li>$$^$<\/li>\n  <li><span class='match'>$^$<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

2. Given the corpus of common words in `stringr::words`, create regular expressions that find all words that:

`str_view()` works well interactively because you can see the exact match but to return words in a nicer format I'll use `str_subset()`.

* Start with “y”.


```r
str_subset(words, "^y")
```

```
## [1] "year"      "yes"       "yesterday" "yet"       "you"       "young"
```

* End with “x”


```r
str_subset(words, "x$")
```

```
## [1] "box" "sex" "six" "tax"
```

* Are exactly three letters long. (Don’t cheat by using `str_length()`!)


```r
str_subset(words, "^...$") %>% head
```

```
## [1] "act" "add" "age" "ago" "air" "all"
```

```r
#str_subset(words, "^.{3}$")
```

* Have seven letters or more.


```r
str_subset(words, ".......") %>% head
```

```
## [1] "absolute"  "account"   "achieve"   "address"   "advertise" "afternoon"
```

```r
#str_subset(words, "^.{7,}$")
```

## 14.3.3.1 Exercises

1. Create regular expressions to find all words that:

* Start with a vowel.


```r
str_subset(words, "^[aeiou]") %>% head
```

```
## [1] "a"        "able"     "about"    "absolute" "accept"   "account"
```

* That only contain consonants. (Hint: thinking about matching “not”-vowels.)


```r
str_subset(words, "^[^aeiou]+$")
```

```
## [1] "by"  "dry" "fly" "mrs" "try" "why"
```

* End with ed, but not with eed.


```r
str_subset(words, "[^e]ed$")
```

```
## [1] "bed"     "hundred" "red"
```

* End with ing or ise.


```r
str_subset(words, "i(ng|se)$")
```

```
##  [1] "advertise" "bring"     "during"    "evening"   "exercise" 
##  [6] "king"      "meaning"   "morning"   "otherwise" "practise" 
## [11] "raise"     "realise"   "ring"      "rise"      "sing"     
## [16] "surprise"  "thing"
```

* Empirically verify the rule “i before e except after c”.

In a vector of this size, simply searching for "ie" seems to be the easiest way to verify empirically. It's not always true.


```r
str_subset(words, "ie")
```

```
##  [1] "achieve"    "believe"    "brief"      "client"     "die"       
##  [6] "experience" "field"      "friend"     "lie"        "piece"     
## [11] "quiet"      "science"    "society"    "tie"        "view"
```

* Is “q” always followed by a “u”?

In this vector, yes.


```r
str_subset(words, "q[^u]")
```

```
## character(0)
```

4. Write a regular expression that matches a word if it’s probably written in British English, not American English.

We can cherry pick a few differences in endings but this is hardly comprehensive.


```r
x <- c("color", "colour", "apologize", "apologise", "analyze", "analyse", "defense","defence", "catalog", "catalogue")
str_subset(x, "our$|ise$|yse$|ence$|ogue$")
```

```
## [1] "colour"    "apologise" "analyse"   "defence"   "catalogue"
```

We could modify this to extract both British or American spellings.


```r
str_subset(x, "ou?r$|i(s|z)e$|y(s|z)e$|en(c|s)e$|ogu?e?$")
```

```
##  [1] "color"     "colour"    "apologize" "apologise" "analyze"  
##  [6] "analyse"   "defense"   "defence"   "catalog"   "catalogue"
```

5. Create a regular expression that will match telephone numbers as commonly written in your country.

This matches the strictest interpretation of the US format (without country code).


```r
phone_numbers <- c("555-555-5555", "55555555555", "+55 555 555 5555")
str_subset(phone_numbers, "\\d{3}-\\d{3}-\\d{4}")
```

```
## [1] "555-555-5555"
```

## 14.3.4.1 Exercises

1. Describe the equivalents of ?, +, * in {m,n} form.

? -> {0,1} (0 or 1)

```r
x <- "1888 is the longest year in Roman numerals: MDCCCLXXXVIII"
str_extract(x, 'CC?')
```

```
## [1] "CC"
```

```r
str_extract(x, "CC{0,1}")
```

```
## [1] "CC"
```

+ -> {1,} (1 or more)

```r
str_extract(x, 'CC+')
```

```
## [1] "CCC"
```

```r
str_extract(x, "CC{1,}")
```

```
## [1] "CCC"
```

* -> {0,} (0 or more)

```r
str_extract(x, 'CC*')
```

```
## [1] "CCC"
```

```r
str_extract(x, "CC{0,}")
```

```
## [1] "CCC"
```

2. Describe in words what these regular expressions match: (read carefully to see if I’m using a regular expression or a string that defines a regular expression.)

* ^.*$

match strings of any length or content

```r
x <- c("abc123", "123", "ABC", "{string}", "5555-55-55")
str_extract(x, "^.*$")
```

```
## [1] "abc123"     "123"        "ABC"        "{string}"   "5555-55-55"
```

* "\\{.+\\}"

matches any string inside curly brackets

```r
x <- c("abc123", "123", "ABC", "{string}", "5555-55-55")
str_extract(x, "\\{.+\\}")
```

```
## [1] NA         NA         NA         "{string}" NA
```

* \d{4}-\d{2}-\d{2}

Note that we need to add escape characters to turn the given regexp into its string representation. Then it matches four digits, hyphen, two digits, hyphen, two digits, which we could use for one particular date format.

```r
x <- c("abc123", "123", "ABC", "{string}", "5555-55-55")
str_extract(x, "\\d{4}-\\d{2}-\\d{2}")
```

```
## [1] NA           NA           NA           NA           "5555-55-55"
```

* "\\\\{4}"


```r
x <- c("abc123", "123", "ABC", "{string}", "5555-55-55", "\\\\\\\\")
str_extract(x, "\\\\{4}")
```

```
## [1] NA         NA         NA         NA         NA         "\\\\\\\\"
```

3. Create regular expressions to find all words that:

* Start with three consonants.


```r
str_subset(words, "^[^aieou]{3}")
```

```
##  [1] "Christ"    "Christmas" "dry"       "fly"       "mrs"      
##  [6] "scheme"    "school"    "straight"  "strategy"  "street"   
## [11] "strike"    "strong"    "structure" "system"    "three"    
## [16] "through"   "throw"     "try"       "type"      "why"
```

* Have three or more vowels in a row.


```r
str_subset(words, "[aieou]{3}")
```

```
## [1] "beauty"   "obvious"  "previous" "quiet"    "serious"  "various"
```

* Have two or more vowel-consonant pairs in a row.

For a smaller list, check three in a row.

```r
str_subset(words, "([aieou][^aieou]){3,}")
```

```
##  [1] "america"    "benefit"    "business"   "definite"   "develop"   
##  [6] "document"   "economy"    "educate"    "eleven"     "evening"   
## [11] "evidence"   "general"    "holiday"    "however"    "imagine"   
## [16] "operate"    "original"   "particular" "politic"    "positive"  
## [21] "separate"   "similar"    "specific"   "television" "therefore" 
## [26] "university"
```

4. Solve the beginner regexp crosswords at https://regexcrossword.com/challenges/beginner.

## 14.3.5.1 Exercises

```r
str_view(fruit, "(..)\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-4c7ae81c933504ad283f" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-4c7ae81c933504ad283f">{"x":{"html":"<ul>\n  <li>b<span class='match'>anan<\/span>a<\/li>\n  <li><span class='match'>coco<\/span>nut<\/li>\n  <li><span class='match'>cucu<\/span>mber<\/li>\n  <li><span class='match'>juju<\/span>be<\/li>\n  <li><span class='match'>papa<\/span>ya<\/li>\n  <li>s<span class='match'>alal<\/span> berry<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

1. Describe, in words, what these expressions will match:

* (.)\1\1

matches the same character three times consecutively

```r
str_subset(c("aaa", "111"), "(.)\\1\\1")
```

```
## [1] "aaa" "111"
```

* "(.)(.)\\2\\1"

matches a consecutive sequence of a character, another character, the second character and the first character.

```r
str_subset(words, "(.)(.)\\2\\1")
```

```
##  [1] "afternoon"   "apparent"    "arrange"     "bottom"      "brilliant"  
##  [6] "common"      "difficult"   "effect"      "follow"      "indeed"     
## [11] "letter"      "million"     "opportunity" "oppose"      "tomorrow"
```

* (..)\1

matches a pair of characters repeated

```r
str_subset(words, "(..)\\1")
```

```
## [1] "remember"
```

* "(.).\\1.\\1"

matches a consecutive sequence of a first character, another character, the first character, another character, and the first character again.

```r
str_subset(words, "(.).\\1.\\1")
```

```
## [1] "eleven"
```

* "(.)(.)(.).*\\3\\2\\1"

matches any three characters followed by zero or more characters, followed by the third, second and first initially matched characters.

```r
str_view(words, "(.)(.)(.).*\\3\\2\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-675e1a57dd49047ccd80" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-675e1a57dd49047ccd80">{"x":{"html":"<ul>\n  <li><span class='match'>paragrap<\/span>h<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

2. Construct regular expressions to match words that:

* Start and end with the same character.


```r
str_subset(words, "^(.).*\\1$")
```

```
##  [1] "america"    "area"       "dad"        "dead"       "depend"    
##  [6] "educate"    "else"       "encourage"  "engine"     "europe"    
## [11] "evidence"   "example"    "excuse"     "exercise"   "expense"   
## [16] "experience" "eye"        "health"     "high"       "knock"     
## [21] "level"      "local"      "nation"     "non"        "rather"    
## [26] "refer"      "remember"   "serious"    "stairs"     "test"      
## [31] "tonight"    "transport"  "treat"      "trust"      "window"    
## [36] "yesterday"
```

* Contain a repeated pair of letters (e.g. “church” contains “ch” repeated twice.)


```r
str_view(words, "([A-Za-z][A-Za-z]).*\\1", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-b5c0954cfb499fc62832" style="width:960px;height:auto;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-b5c0954cfb499fc62832">{"x":{"html":"<ul>\n  <li>ap<span class='match'>propr<\/span>iate<\/li>\n  <li><span class='match'>church<\/span><\/li>\n  <li>c<span class='match'>ondition<\/span><\/li>\n  <li><span class='match'>decide<\/span><\/li>\n  <li><span class='match'>environmen<\/span>t<\/li>\n  <li>l<span class='match'>ondon<\/span><\/li>\n  <li>pa<span class='match'>ragra<\/span>ph<\/li>\n  <li>p<span class='match'>articular<\/span><\/li>\n  <li><span class='match'>photograph<\/span><\/li>\n  <li>p<span class='match'>repare<\/span><\/li>\n  <li>p<span class='match'>ressure<\/span><\/li>\n  <li>r<span class='match'>emem<\/span>ber<\/li>\n  <li><span class='match'>repre<\/span>sent<\/li>\n  <li><span class='match'>require<\/span><\/li>\n  <li><span class='match'>sense<\/span><\/li>\n  <li>the<span class='match'>refore<\/span><\/li>\n  <li>u<span class='match'>nderstand<\/span><\/li>\n  <li>w<span class='match'>hethe<\/span>r<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

* Contain one letter repeated in at least three places (e.g. “eleven” contains three “e”s.)


```r
str_subset(words, "([A-Za-z]).*\\1.*\\1")
```

```
##  [1] "appropriate" "available"   "believe"     "between"     "business"   
##  [6] "degree"      "difference"  "discuss"     "eleven"      "environment"
## [11] "evidence"    "exercise"    "expense"     "experience"  "individual" 
## [16] "paragraph"   "receive"     "remember"    "represent"   "telephone"  
## [21] "therefore"   "tomorrow"
```


## 14.4.2 Exercises

1. For each of the following challenges, try solving it by using both a single regular expression, and a combination of multiple `str_detect()` calls.

* Find all words that start or end with x.


```r
str_subset(words, "^x|x$")
```

```
## [1] "box" "sex" "six" "tax"
```

```r
# words[str_detect(words, "^x") | str_detect(words, "x$")]
```

* Find all words that start with a vowel and end with a consonant.


```r
str_subset(words, "^[aeiou].*[^aeiou]$")
```

```
##   [1] "about"       "accept"      "account"     "across"      "act"        
##   [6] "actual"      "add"         "address"     "admit"       "affect"     
##  [11] "afford"      "after"       "afternoon"   "again"       "against"    
##  [16] "agent"       "air"         "all"         "allow"       "almost"     
##  [21] "along"       "already"     "alright"     "although"    "always"     
##  [26] "amount"      "and"         "another"     "answer"      "any"        
##  [31] "apart"       "apparent"    "appear"      "apply"       "appoint"    
##  [36] "approach"    "arm"         "around"      "art"         "as"         
##  [41] "ask"         "at"          "attend"      "authority"   "away"       
##  [46] "awful"       "each"        "early"       "east"        "easy"       
##  [51] "eat"         "economy"     "effect"      "egg"         "eight"      
##  [56] "either"      "elect"       "electric"    "eleven"      "employ"     
##  [61] "end"         "english"     "enjoy"       "enough"      "enter"      
##  [66] "environment" "equal"       "especial"    "even"        "evening"    
##  [71] "ever"        "every"       "exact"       "except"      "exist"      
##  [76] "expect"      "explain"     "express"     "identify"    "if"         
##  [81] "important"   "in"          "indeed"      "individual"  "industry"   
##  [86] "inform"      "instead"     "interest"    "invest"      "it"         
##  [91] "item"        "obvious"     "occasion"    "odd"         "of"         
##  [96] "off"         "offer"       "often"       "okay"        "old"        
## [101] "on"          "only"        "open"        "opportunity" "or"         
## [106] "order"       "original"    "other"       "ought"       "out"        
## [111] "over"        "own"         "under"       "understand"  "union"      
## [116] "unit"        "university"  "unless"      "until"       "up"         
## [121] "upon"        "usual"
```

```r
# words[str_detect(words, "^[aeiou]") & str_detect(words, "[^aeiou]$")]
```

* Are there any words that contain at least one of each different vowel?


```r
words[str_detect(words, "a") & str_detect(words, "e") & str_detect(words, "i") & str_detect(words, "o") & str_detect(words, "u")] 
```

```
## character(0)
```

2. What word has the highest number of vowels? What word has the highest proportion of vowels? (Hint: what is the denominator?)

A few words all have 5 vowels in them. "a" technically has the highest proportion of vowels.


```r
tibble(words) %>% 
    mutate(vowel_count = str_count(words, "[aeiou]"),
           vowel_prop = vowel_count / str_count(words)) %>% 
    #arrange(-vowel_count) %>%
    arrange(-vowel_prop)
```

```
## # A tibble: 980 x 3
##    words  vowel_count vowel_prop
##    <chr>        <int>      <dbl>
##  1 a                1      1.00 
##  2 area             3      0.750
##  3 idea             3      0.750
##  4 age              2      0.667
##  5 ago              2      0.667
##  6 air              2      0.667
##  7 die              2      0.667
##  8 due              2      0.667
##  9 eat              2      0.667
## 10 europe           4      0.667
## # ... with 970 more rows
```

## 14.4.3.1 Exercises

```r
head(sentences)
```

```
## [1] "The birch canoe slid on the smooth planks." 
## [2] "Glue the sheet to the dark blue background."
## [3] "It's easy to tell the depth of a well."     
## [4] "These days a chicken leg is a rare dish."   
## [5] "Rice is often served in round bowls."       
## [6] "The juice of lemons makes fine punch."
```

```r
colours <- c("red", "orange", "yellow", "green", "blue", "purple")
colour_match <- str_c(colours, collapse = "|")
```

1. In the previous example, you might have noticed that the regular expression matched “flickered”, which is not a colour. Modify the regex to fix the problem.

Words like "covered" will also be a problem because they contain "red". We can solve this problem by placing a boundary around each colour.


```r
colours <- c("red", "orange", "yellow", "green", "blue", "purple")
colour_match <- str_c(str_c("\\b", colours, "\\b"), collapse = "|")
sentences[str_count(sentences, colour_match) > 1]
```

```
## [1] "It is hard to erase blue or red ink."          
## [2] "The sky in the west is tinged with orange red."
```

2. From the Harvard sentences data, extract:

* The first word from each sentence.


```r
str_extract(sentences, "^[a-zA-Z0-9]+") %>% head()
```

```
## [1] "The"   "Glue"  "It"    "These" "Rice"  "The"
```

`stringr` also has a handy `word()` function that can capture a certain position.


```r
word(sentences, 1) %>% head()
```

```
## [1] "The"   "Glue"  "It's"  "These" "Rice"  "The"
```

* All words ending in ing.


```r
ing_words <- str_extract(sentences, "\\b[A-Za-z]+ing\\b")
ing_words[!is.na(ing_words)]
```

```
##  [1] "spring"    "evening"   "morning"   "winding"   "living"   
##  [6] "king"      "Adding"    "making"    "raging"    "playing"  
## [11] "sleeping"  "ring"      "glaring"   "sinking"   "dying"    
## [16] "Bring"     "lodging"   "filing"    "making"    "morning"  
## [21] "wearing"   "Bring"     "wading"    "swing"     "nothing"  
## [26] "ring"      "morning"   "sing"      "sleeping"  "painting" 
## [31] "king"      "walking"   "bring"     "bring"     "shipping" 
## [36] "spring"    "ring"      "winding"   "puzzling"  "spring"   
## [41] "landing"   "thing"     "waiting"   "whistling" "nothing"  
## [46] "timing"    "thing"     "spring"    "changing"  "drenching"
## [51] "moving"    "working"   "ring"
```

* All plurals.

We can extract words more than three characters than end in "s" but this won't be a perfect list because of many non-plural words that happen to end in "s" like "pass".


```r
sentences %>%
    str_extract_all("\\b[a-zA-Z]{3,}s\\b") %>%
    unlist() %>%
    unique() %>%
    head()
```

```
## [1] "planks" "days"   "bowls"  "lemons" "makes"  "hogs"
```

## 14.4.4.1 Exercises

```r
noun <- "(a|the) ([^ ]+)"

has_noun <- sentences %>%
  str_subset(noun) %>%
  head(10)
has_noun %>% 
  str_extract(noun)
```

```
##  [1] "the smooth" "the sheet"  "the depth"  "a chicken"  "the parked"
##  [6] "the sun"    "the huge"   "the ball"   "the woman"  "a helps"
```

```r
has_noun %>% 
  str_match(noun)
```

```
##       [,1]         [,2]  [,3]     
##  [1,] "the smooth" "the" "smooth" 
##  [2,] "the sheet"  "the" "sheet"  
##  [3,] "the depth"  "the" "depth"  
##  [4,] "a chicken"  "a"   "chicken"
##  [5,] "the parked" "the" "parked" 
##  [6,] "the sun"    "the" "sun"    
##  [7,] "the huge"   "the" "huge"   
##  [8,] "the ball"   "the" "ball"   
##  [9,] "the woman"  "the" "woman"  
## [10,] "a helps"    "a"   "helps"
```

1. Find all words that come after a “number” like “one”, “two”, “three” etc. Pull out both the number and the word.

This approach is limited to the specific number words we give. 


```r
tibble(sentence = sentences) %>% 
  tidyr::extract(
    sentence, c("number", "after"), 
        "(zero|one|two|three|four|five|six|seven|eight|nine|ten) ([^ ]+)", 
    remove = FALSE) %>%
  filter(!is.na(number))    
```

```
## # A tibble: 36 x 3
##    sentence                                    number after  
##    <chr>                                       <chr>  <chr>  
##  1 Rice is often served in round bowls.        ten    served 
##  2 Lift the square stone over the fence.       one    over   
##  3 The rope will bind the seven books at once. seven  books  
##  4 The two met while playing on the sand.      two    met    
##  5 There are more than two factors here.       two    factors
##  6 He lay prone and hardly moved a limb.       one    and    
##  7 Type out three lists of orders.             three  lists  
##  8 Two plus seven is less than ten.            seven  is     
##  9 Drop the two when you add the figures.      two    when   
## 10 Torn scraps littered the stone floor.       one    floor. 
## # ... with 26 more rows
```

2. Find all contractions. Separate out the pieces before and after the apostrophe.


```r
tibble(sentence = sentences) %>% 
  tidyr::extract(
    sentence, c("before", "after"), "([a-zA-Z]+)'([a-zA-Z]+)", 
    remove = FALSE) %>%
  filter(!is.na(before)) 
```

```
## # A tibble: 15 x 3
##    sentence                                     before   after
##    <chr>                                        <chr>    <chr>
##  1 It's easy to tell the depth of a well.       It       s    
##  2 The soft cushion broke the man's fall.       man      s    
##  3 Open the crate but don't break the glass.    don      t    
##  4 Add the store's account to the last cent.    store    s    
##  5 The beam dropped down on the workmen's head. workmen  s    
##  6 Let's all join as we sing the last chorus.   Let      s    
##  7 The copper bowl shone in the sun's rays.     sun      s    
##  8 A child's wit saved the day for us.          child    s    
##  9 A ripe plum is fit for a king's palate.      king     s    
## 10 It's a dense crowd in two distinct ways.     It       s    
## 11 We don't get much money but we have fun.     don      t    
## 12 Ripe pears are fit for a queen's table.      queen    s    
## 13 We don't like to admit our small faults.     don      t    
## 14 Dig deep in the earth for pirate's gold.     pirate   s    
## 15 She saw a cat in the neighbor's house.       neighbor s
```

## 14.4.5.1 Exercises

1. Replace all forward slashes in a string with backslashes.


```r
original <- c("22/03/1998", "hello/world")
replaced <- str_replace_all(original, "\\/", "\\\\")
writeLines(replaced)
```

```
## 22\03\1998
## hello\world
```

2. Implement a simple version of `str_to_lower()` using `replace_all()`.

Recall that `str_replace_all()` also works with a named vector.


```r
my_str_lower <- function(x) {
  names(letters) <- LETTERS
  str_replace_all(x, letters)
}
head(my_str_lower(sentences))
```

```
## [1] "the birch canoe slid on the smooth planks." 
## [2] "glue the sheet to the dark blue background."
## [3] "it's easy to tell the depth of a well."     
## [4] "these days a chicken leg is a rare dish."   
## [5] "rice is often served in round bowls."       
## [6] "the juice of lemons makes fine punch."
```

3. Switch the first and last letters in `words`. Which of those strings are still words?

We can find overlap with `intersect()` after the initial match. In most cases, it's where the first and last letters is the same.

```r
reversed <- str_replace_all(str_to_lower(words), "^([a-z])(.*)([a-z])$", "\\3\\2\\1")
intersect(reversed,words)
```

```
##  [1] "a"          "america"    "area"       "dad"        "dead"      
##  [6] "lead"       "read"       "depend"     "god"        "educate"   
## [11] "else"       "encourage"  "engine"     "europe"     "evidence"  
## [16] "example"    "excuse"     "exercise"   "expense"    "experience"
## [21] "eye"        "dog"        "health"     "high"       "knock"     
## [26] "deal"       "level"      "local"      "nation"     "on"        
## [31] "non"        "no"         "rather"     "dear"       "refer"     
## [36] "remember"   "serious"    "stairs"     "test"       "tonight"   
## [41] "transport"  "treat"      "trust"      "window"     "yesterday"
```

## 14.4.6.1 Exercises

```r
x <- "This is a sentence? This is another sentence..And another! The"
str_split(x, boundary("word"))[[1]]
```

```
##  [1] "This"     "is"       "a"        "sentence" "This"     "is"      
##  [7] "another"  "sentence" "And"      "another"  "The"
```

1. Split up a string like `"apples, pears, and bananas"` into individual components.


```r
x <- c("apples, pears, and bananas")
str_split(x, boundary("word")) %>% unlist
```

```
## [1] "apples"  "pears"   "and"     "bananas"
```

```r
# if we want only the fruit
str_split(x, ", +(and +)?") %>% unlist
```

```
## [1] "apples"  "pears"   "bananas"
```

2. Why is it better to split up by `boundary("word")` than `" "`?

If we tried splitting the previous exercise by " ", commas and/or other punctuation remain.

```r
str_split(x, " ") %>% unlist
```

```
## [1] "apples," "pears,"  "and"     "bananas"
```

3. What does splitting with an empty string ("") do? Experiment, and then read the documentation.

It is equivalent to `str_split(x, boundary("character")) %>% unlist`.

```r
str_split(x, "") %>% unlist
```

```
##  [1] "a" "p" "p" "l" "e" "s" "," " " "p" "e" "a" "r" "s" "," " " "a" "n"
## [18] "d" " " "b" "a" "n" "a" "n" "a" "s"
```

## 14.5.1 Exercises

1. How would you find all strings containing \ with `regex()` vs. with `fixed()`?


```r
eg <- c("contains \\", "no slashes")

str_subset(eg, regex("\\\\"))
```

```
## [1] "contains \\"
```

```r
str_subset(eg, fixed("\\"))
```

```
## [1] "contains \\"
```

2. What are the five most common words in `sentences`?


```r
str_split(sentences, boundary("word")) %>% 
    unlist() %>%
    str_to_lower() %>%
    tibble(word = .) %>%
    group_by(word) %>% 
    count() %>% 
    arrange(-n) %>%
    head(5)
```

```
## # A tibble: 5 x 2
## # Groups:   word [5]
##   word      n
##   <chr> <int>
## 1 the     751
## 2 a       202
## 3 of      132
## 4 to      123
## 5 and     118
```

## 14.7.1 Exercises

1. Find the `stringi` functions that:


```r
library(stringi)
```

* Count the number of words.


```r
stri_count_words(sentences) %>% head
```

```
## [1] 8 8 9 9 7 7
```

* Find duplicated strings.


```r
stri_duplicated(sentences) %>% head
```

```
## [1] FALSE FALSE FALSE FALSE FALSE FALSE
```

* Generate random text.


```r
stri_rand_strings(5, 10) # random strings of desired lengths
```

```
## [1] "9i0xYjKiZI" "WEzoCAbHJH" "saMtLlFUyj" "qde7CoW9VF" "ITu6ZC0f9F"
```

```r
stri_rand_lipsum(1) # lorem ipsum text
```

```
## [1] "Lorem ipsum dolor sit amet, primis molestie luctus sed. Eu rutrum et torquent lacus eget in vivamus vel ac in. Nec maximus. Ac ultricies ad faucibus interdum sem euismod eu in in, lacus. Risus congue ante varius nec vel non litora mattis orci. Cursus etiam etiam in, sed libero proin, lacus efficitur. Morbi eget sed pellentesque cum lorem et vel, eros, eget tincidunt erat lectus vel. Eget aliquet est parturient et maecenas ligula sed magna. Lobortis non primis non, ipsum. Sed, varius, et ut vel lacus vitae, cum."
```

```r
stri_rand_shuffle(stri_paste(letters, collapse = ''))
```

```
## [1] "eoxkrplfchuwgvdjatmizsqnby"
```

2. How do you control the language that `stri_sort()` uses for sorting?

With the locale argument.

