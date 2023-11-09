Writing Functions
================
Ruijie He
2023-11-09

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = 0.6,
  out.width = "90%"
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.color = "viridis",
  ggplot2.continuous.fill = "viridis"
)

scale_colour_discrete = scale_color_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.39572376 -0.29905501 -0.72202172 -0.53314513  0.65243937 -0.71696985
    ##  [7] -0.06819058 -1.32424939 -0.77403122 -0.43210355 -2.28132887  0.11991293
    ## [13]  0.28041737 -0.75664137 -1.16147330 -1.16965821  0.58587150  2.16777230
    ## [19]  1.02504123  1.11726976  0.41849725 -0.47864271 -0.97402235  1.17288305
    ## [25] -0.87040359  0.64774737  0.95976673  0.37432697  1.13129540  1.51297187

I want a **function** to compute z-score

``` r
z_scores = function(x) { ## Setting z-score equal to a whole function instead of computation
  
  if(!is.numeric(x)) {
    stop("Input must be numeric") # Stop with the error message in ""
  } # If statement to set numeric number only
  
  if(length(x)  < 3) {
    stop("Input must have at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x)
 
   return(z)
  
} # The body of the function

z_scores(x_vec)
```

    ##  [1]  0.39572376 -0.29905501 -0.72202172 -0.53314513  0.65243937 -0.71696985
    ##  [7] -0.06819058 -1.32424939 -0.77403122 -0.43210355 -2.28132887  0.11991293
    ## [13]  0.28041737 -0.75664137 -1.16147330 -1.16965821  0.58587150  2.16777230
    ## [19]  1.02504123  1.11726976  0.41849725 -0.47864271 -0.97402235  1.17288305
    ## [25] -0.87040359  0.64774737  0.95976673  0.37432697  1.13129540  1.51297187

Try my function on some other things (these should give errors)

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least three numbers

``` r
z_scores("my name is jeff") # Cannot take mean of non-numerical data
```

    ## Error in z_scores("my name is jeff"): Input must be numeric

``` r
z_scores(mtcars) # Cannot take mean of a dataset
```

    ## Error in z_scores(mtcars): Input must be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric
