2021-10-20 (MvR): Annoyingly, the sidebar on the right displays up the h3 header level by default with the latest `bslib` package. I've not found a suitable long-term solution, but for now downgrading to bslib 0.2.4 seems to work. For more information see: [this issue](https://issueexplorer.com/issue/rstudio/bookdown/1269). 

This is an adapted version of the bookdown minimal template.

It renders the website using the bs4 bootstrap style.
To accommodate the style guide of the University of Cambridge the default HTML template normally used for bs4 is changed and now located in the main `bookdown` directory.
It's named `uoc_bs4.html`.

Requirements (these are loaded in `setup.R` but might need installing locally)
`library(knitr)`      # for some extra functionality
`library(tidyverse)`  # because we love it
`library(kableExtra)` # for nice tables
`library(car)`        # for Levene's test

Each Practical chapter is organised in the following way:

Topic
  |__ Objectives
  |__ Purpose and aim
  |__ Choosing a test
  |__  ... (test)
    |__ Section commands
    |__ Data and hypotheses
    |__ Summarise and visualise
    |__ Assumptions
    |__ Implement test
    |__ Interpret output and report results
    |__ Exercise
  |__ Key points
  
This order can vary slightly in the first practicals because more focus is given to the visual interpretation of the various assumption checks.
