# Assignment B1 Repository

Welcome to the repository for **Assignment B1**. This repository contains two files related to an R assignment, where the primary focus is on creating, documenting, and testing a custom R function for data analysis.

### Files:

1. **Assignment-B1.md**  
   This file is a markdown document that contains an overview of the assignment, including explanations and some output examples from running the code.
   
3. **Assignment-B1.Rmd**  
   This is the R Markdown file containing the actual R code for the assignment. It includes:
   - The function `summarize_by_group()`, which groups and summarizes a numeric variable by a specified categorical variable.
   - Documentation using `roxygen2`-style comments, which provide descriptions for the function and its parameters.
   - Multiple examples demonstrating the usage of the function.
   - Formal tests for the function using the `testthat` package, to ensure the function works as expected.

### Requirements

To run the code in the `Assignment-B1.Rmd` file, ensure that you have the following R packages installed:

```r
# Required packages
install.packages("dplyr")
install.packages("tidyverse")
install.packages("rlang")
install.packages("testthat")
```

### How to Run the Code

1. Open the **Assignment-B1.Rmd** file in RStudio.
2. Install the requirements.
3. Run the code chunks to see the function in action, along with the provided examples.
4. The function `summarize_by_group()` can be tested using the `testthat` package, with predefined tests located in the code.
