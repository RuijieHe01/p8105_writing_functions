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
x_vec = rnorm(30, mean = 5, sd = 3) # Input data of the function

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.16155157 -1.24714363 -1.23649109  0.18353803  0.24319865  1.46457821
    ##  [7]  0.77466234  0.46337115 -0.36384102  0.62212574 -1.76160903 -1.42929081
    ## [13] -0.85810715  0.01364349 -1.25737403  1.07980045  1.47617930  1.61329227
    ## [19] -0.06810653 -1.40688201  0.71893830  0.10109751  0.82112416 -0.08983868
    ## [25]  0.04367674  1.15110163 -0.51781102  0.62612505 -1.85500294  0.53349334

I want a **function** to compute z-score

``` r
z_scores = function(x) { # Setting z-score equal to a whole function instead of computation
  
  if(!is.numeric(x)) {
    stop("Input must be numeric") # Stop with the error message in ""
  } # If statement to set numeric number only
  
  if(length(x)  < 3) {
    stop("Input must have at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x) # Return objects
 
   return(z)
  
} # The body of the function

z_scores(x_vec)
```

    ##  [1]  0.16155157 -1.24714363 -1.23649109  0.18353803  0.24319865  1.46457821
    ##  [7]  0.77466234  0.46337115 -0.36384102  0.62212574 -1.76160903 -1.42929081
    ## [13] -0.85810715  0.01364349 -1.25737403  1.07980045  1.47617930  1.61329227
    ## [19] -0.06810653 -1.40688201  0.71893830  0.10109751  0.82112416 -0.08983868
    ## [25]  0.04367674  1.15110163 -0.51781102  0.62612505 -1.85500294  0.53349334

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

## Multipleoutputs

``` r
mean_and_sd = function(x) {
  
  if(!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if(length(x)  < 3) {
    stop("Input must have at least three numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
# Organize the multiple outputs
  
  tibble( #Make these output a daat frame
    mean = mean_x,
    sd = sd_x
  )
  
}
```

Check out the functions

``` r
x_vec = rnorm(100, mean = 3, sd = 4) # Creating new vector
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.18  3.81

## Multiple inputs

I’d like to do this with a function

``` r
sim_data = # Simulated dataset
  tibble(
    x = rnorm(n = 100, mean = 4, sd = 3)
  ) # Create data frame

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  ) # Give the data frame computations of mean and sd
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.57  3.01

Translate the above into functions

``` r
sim_mean_sd = function(sample_size, mu = 3, sigma = 4) {# Three inputs with given default value
  sim_data = # Simulated dataset
  tibble(
    x = rnorm(n = sample_size, mean = mu, sd = sigma)
  )
  
  sim_data %>% 
    summarize(
      mean = mean(x),
      sd = sd(x)
      )
} # Body of function

sim_mean_sd(sample_size = 100, mu = 6, sigma = 3) # Positional matching
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.78  3.15

``` r
sim_mean_sd(mu = 6, sample_size = 100, sigma = 3) # The new value will overwrite the defalut value
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.96  2.94
