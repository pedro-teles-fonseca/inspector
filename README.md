
<!-- README.md is generated from README.Rmd. Please edit that file -->

# inspector: Validation of Arguments and Objects in User-Defined Functions

<!-- badges: start -->

[![Build
Status](https://travis-ci.com/pedro-teles-fonseca/inspector.svg?branch=master)](https://travis-ci.com/pedro-teles-fonseca/inspector)
[![R build
status](https://github.com/pedro-teles-fonseca/inspector/workflows/R-CMD-check/badge.svg)](https://github.com/pedro-teles-fonseca/inspector/actions)
![pkgdown](https://github.com/pedro-teles-fonseca/inspector/workflows/pkgdown/badge.svg)
[![codecov](https://codecov.io/gh/pedro-teles-fonseca/inspector/branch/master/graph/badge.svg)](https://codecov.io/gh/pedro-teles-fonseca/inspector)
[![MIT
license](https://img.shields.io/badge/License-MIT-brightgreen.svg)](https://lbesson.mit-license.org/)
<!-- badges: end -->

## Overview

The `inspector` package provides utility functions that implement and
automate common sets of validation tasks, namely:

  - `inspect_prob()` checks if an object is a numeric vector of valid
    probability values.

  - `inspect_bfactor()` checks if an object is a numeric vector of valid
    Bayes factors values.

  - `inspect_bfactor_log()` checks if an object is a numeric vector of
    valid logarithmic Bayes factors values.

  - `inspect_log_base()` checks if an object is a valid logarithmic
    base.

  - `inspect_true_or_false()` checks if an object is a non-missing
    logical value.

  - `inspect_scale` check if an object is a valid Bayes factor
    interpretation scale.

  - `inspect_categories()` validates factor levels.

  - `inspect_character()` validates character vectors.

  - `inspect_character_match()` validates character values with
    predefined allowed values.

  - `inspect_data_dichotomous()` validates dichotomous data

  - `inspect_data_categorical()` and `inspect_data_multinom_as_bern()`
    validate categorical data.

  - `inspect_par_bernoulli()` validates parameters for the Bernoulli and
    Binomial distributions.

  - `inspect_par_multinomial()` validates parameters for the Multinomial
    distribution.

  - `inspect_par_beta()` validates parameters for the Beta distribution.

  - `inspect_par_dirichlet()` validates parameters for the Dirichlet
    distribution.

  - `inspect_par_haldane()` validates parameters for the Haldane
    distribution.

These functions are particularly useful to validate inputs, intermediate
objects and output values in user-defined functions, resulting in tidier
and less verbose functions.

## Installation

The development version of `inspector` can be installed from
[GitHub](https://github.com/) using the `devtools` package:

``` r
# install.packages("devtools")
devtools::install_github("pedro-teles-fonseca/inspector")
```

## Usage

``` r

set.seed(123)

flip_coin <- function(n, bias){
  
  sample(c("heads", "tails"), size = n, replace = TRUE)

}

flip_coin(n = 5, bias = 0.5)
#> [1] "heads" "heads" "heads" "tails" "heads"
```

``` r

set.seed(123)

flip_coin <- function(n, bias){
  
  if(is.null(bias)){
    stop(paste("Invalid argument: bias is NULL."))
  }
  if(any(isFALSE(is.atomic(bias)), isFALSE(is.vector(bias)))){
    stop(paste("Invalid argument: bias must be an atomic vector."))
  }
  if(isFALSE(length(bias) == 1)){
    stop(paste("Invalid argument: bias must be of length 1."))
  }
  if(is.na(bias)){
    stop(paste("Invalid argument: bias is NA or NaN."))
  }
  if(isFALSE(is.numeric(bias))){
    stop(paste("Invalid argument: bias must be numeric."))
  }
  if(any(bias >= 1, bias <= 0)) {
    stop(paste("Invalid argument: bias must be in the (0, 1) interval."))
  }
  
  sample(c("heads", "tails"), size = n, replace = TRUE)
}

flip_coin(n = 5, bias = 0.5)
#> [1] "heads" "heads" "heads" "tails" "heads"
```

``` r

set.seed(123)

flip_coin <- function(n, bias){
  
  inspect_par_bernoulli(bias)
  
  sample(c("heads", "tails"), size = n, replace = TRUE)

}

flip_coin(n = 5, bias = 0.5)
#> [1] "heads" "heads" "heads" "tails" "heads"
```

turns Bayes factors into posterior probabilities using a formula from
Berger and Delampady (1987)

## Getting Help

If you find a bug, please file an issue with a minimal reproducible
example on [GitHub](https://github.com/pedro-teles-fonseca/inspector).
Feature requests are also welcome. You can contact me at
<pedro.teles.fonseca@phd.iseg.ulisboa.pt>.

## References

<div id="refs" class="references hanging-indent">

<div id="ref-bergerDelampady1987">

Berger, James O., and Mohan Delampady. 1987. “Testing Precise
Hypotheses.” *Statistical Science* 2 (3): 317–35.

</div>

</div>
