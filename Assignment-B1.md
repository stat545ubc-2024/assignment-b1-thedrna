Assignment B1
================
2024-10-30

# Assignment B1

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ forcats   1.0.0     ✔ readr     2.1.5
    ## ✔ ggplot2   3.5.1     ✔ stringr   1.5.1
    ## ✔ lubridate 1.9.3     ✔ tibble    3.2.1
    ## ✔ purrr     1.0.2     ✔ tidyr     1.3.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rlang)
```

    ## 
    ## Attaching package: 'rlang'
    ## 
    ## The following objects are masked from 'package:purrr':
    ## 
    ##     %@%, flatten, flatten_chr, flatten_dbl, flatten_int, flatten_lgl,
    ##     flatten_raw, invoke, splice

``` r
library(roxygen2)
library(testthat)
```

    ## 
    ## Attaching package: 'testthat'
    ## 
    ## The following objects are masked from 'package:rlang':
    ## 
    ##     is_false, is_null, is_true
    ## 
    ## The following object is masked from 'package:purrr':
    ## 
    ##     is_null
    ## 
    ## The following objects are masked from 'package:readr':
    ## 
    ##     edition_get, local_edition
    ## 
    ## The following object is masked from 'package:tidyr':
    ## 
    ##     matches
    ## 
    ## The following object is masked from 'package:dplyr':
    ## 
    ##     matches

## Exercise 1: Writing a function in R

My goal is to create a function that summarizes a numerical variable by
a grouping variable. It will compute **mean**, **median**, **standard
deviation**, and **count**, which are common summary statistics.

``` r
summarize_by_group <- function(data, group_var, num_var) {
  group_var <- sym(group_var)  # Convert group_var string to symbol
  num_var <- sym(num_var)      # Convert num_var string to symbol
  
  # Check if the num_var column is numeric
  if (!is.numeric(data[[as.character(num_var)]])) {
    stop("The 'num_var' column must be numeric.")
  }
  
  data %>%
    group_by(!!group_var) %>%  # Use !! to unquote the symbol
    summarize(
      mean = mean(!!num_var, na.rm = TRUE),
      median = median(!!num_var, na.rm = TRUE),
      sd = sd(!!num_var, na.rm = TRUE),
      n = n()
    )
}
```

Here is how the function should be called on a dataset:

``` r
summarize_by_group(mtcars, "cyl", "mpg")
```

    ## # A tibble: 3 × 5
    ##     cyl  mean median    sd     n
    ##   <dbl> <dbl>  <dbl> <dbl> <int>
    ## 1     4  26.7   26    4.51    11
    ## 2     6  19.7   19.7  1.45     7
    ## 3     8  15.1   15.2  2.56    14

## Exercise 2: Documenting function

Here is the documented function using `roxygen2` tags:

``` r
#' Summarize a numeric variable by a grouping variable
#'
#' This function takes a dataset and summarizes a numeric variable (e.g., mean, median, standard deviation) for each level of a grouping variable.
#'
#' @param data A data frame containing the data to be summarized. The name `data` was chosen because it clearly indicates the main input for the function.
#' @param group_var A string specifying the name of the column to group by. I named this `group_var` because it directly reflects its purpose — to specify the grouping variable.
#' @param num_var A string specifying the name of the numeric column to summarize. The name `num_var` was chosen because it represents the numeric variable to summarize across the groups.
#' @return A tibble with the grouping variable and calculated summary statistics: mean, median, standard deviation, and count of observations for each group.
#' 
#' @examples
#' # Summarizing the 'mpg' variable by the number of cylinders in the 'mtcars' dataset
#' summarize_by_group(mtcars, "cyl", "mpg")

summarize_by_group <- function(data, group_var, num_var) {
  group_var <- sym(group_var)  # Convert group_var string to symbol
  num_var <- sym(num_var)      # Convert num_var string to symbol
  
  # Check if the num_var column is numeric
  if (!is.numeric(data[[as.character(num_var)]])) {
    stop("The 'num_var' column must be numeric.")
  }
  
  data %>%
    group_by(!!group_var) %>%
    summarize(
      mean = mean(!!num_var, na.rm = TRUE),
      median = median(!!num_var, na.rm = TRUE),
      sd = sd(!!num_var, na.rm = TRUE),
      n = n()
    )
}
```

## Exercise 3: Including (more) examples

### Example 1

In this example, I use the `mtcars` dataset to summarize ‘**mpg**’ by
‘**cyl**’. This will calculate the mean, median, standard deviation, and
count of miles per gallon for each cylinder group.

``` r
summarize_by_group(mtcars, "cyl", "mpg")
```

    ## # A tibble: 3 × 5
    ##     cyl  mean median    sd     n
    ##   <dbl> <dbl>  <dbl> <dbl> <int>
    ## 1     4  26.7   26    4.51    11
    ## 2     6  19.7   19.7  1.45     7
    ## 3     8  15.1   15.2  2.56    14

### Example 2

This example uses the `iris` dataset to show that the function works on
different datasets. I summarize **Sepal.Length** by the **Species**
column.

This demonstrates the flexibility of the function across different
datasets.

``` r
summarize_by_group(iris, "Species", "Sepal.Length")
```

    ## # A tibble: 3 × 5
    ##   Species     mean median    sd     n
    ##   <fct>      <dbl>  <dbl> <dbl> <int>
    ## 1 setosa      5.01    5   0.352    50
    ## 2 versicolor  5.94    5.9 0.516    50
    ## 3 virginica   6.59    6.5 0.636    50

### Example 3

Using `iris` as the dataset, I am deliberately passing a non-numeric
column (**Species**) as the numeric variable to summarize. This should
raise an error, as the function expects **num_var** to be numeric.

``` r
summarize_by_group(iris, "Species", "Species")
```

    ## Error in summarize_by_group(iris, "Species", "Species"): The 'num_var' column must be numeric.

## Exercise 4: Test

### Test 1: Function with Numeric Column (No NA’s)

I check if the function works properly with a clean numeric column
(**mpg**), ensuring the result is a tibble, the number of rows is
correct (3 groups), and the column names are accurate.

``` r
test_that("Function works with numeric column without NA's", {
  result <- summarize_by_group(mtcars, "cyl", "mpg")
  expect_s3_class(result, "tbl_df")  # Check if the result is a tibble
  expect_equal(nrow(result), 3)      # Check if the number of rows is correct (3 unique 'cyl' groups)
  expect_named(result, c("cyl", "mean", "median", "sd", "n"))  # Check if column names are correct
})
```

    ## Test passed 🥇

### Test 2: **Function with NA’s in Numeric Column**

I introduce `NA` values in the **mpg** column and check if the function
handles these missing values properly. The result should have no `NA`
values in the summary statistics.

``` r
test_that("Function handles numeric column with NA's", {
  # Introduce some NA's into the 'mpg' column
  mtcars_with_na <- mtcars
  mtcars_with_na$mpg[c(1, 2)] <- NA
  
  result <- summarize_by_group(mtcars_with_na, "cyl", "mpg")
  expect_true(all(!is.na(result$mean)))  # Check that no NA values are in the result
})
```

    ## Test passed 🥳

### Test 3: Non-Numeric Column

I ensure that the function throws an error when a non-numeric column is
provided (e.g., passing **Species** as both **group_var** and
**num_var**).

``` r
test_that("Function throws error when non-numeric column is provided", {
  expect_error(summarize_by_group(iris, "Species", "Species"), "must be numeric")
})
```

    ## Test passed 🎉

### Test 4: Empty dataset

This test ensures that the function can handle an empty dataset without
error and returns a tibble with 0 rows.

``` r
test_that("Function works with empty dataset", {
  empty_data <- mtcars[0, ]
  result <- summarize_by_group(empty_data, "cyl", "mpg")
  expect_equal(nrow(result), 0)  # Expect 0 rows as the input data is empty
})
```

    ## Test passed 🥇
