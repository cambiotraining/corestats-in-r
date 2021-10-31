--- 
title: "Core Statistics in R"
author: "Matt Castle and Martin van Rongen"
date: "2021-10-31"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
# url: your book url like https://bookdown.org/yihui/bookdown
# cover-image: path to the social sharing image like images/cover.jpg
description: "These are the supporting materials for the Core Statistics modules of the PSLS Biostatistics Initiative, Cambridge University." 
---



# Overview

These sessions are intended to enable you to perform core data analysis techniques appropriately and confidently using R.

- 6 lecture-practicals
- Ongoing formative assessment exercises
- No formal assessment

- No mathematical derivations
- No pen and paper calculations

They are **not** a “how to mindlessly use a stats program” course!

## Core aims
:::highlight
To know what to do when presented with an arbitrary dataset e.g.

1. Know what data analysis techniques are available
2. Know which ones are allowable
3. Be able to carry these out and understand the results
:::

## Core topics

1. [Simple Hypothesis Testing](#cs1-intro)
2. [Categorical Predictor Variables](#cs2-intro)
3. [Continuous Predictors](#cs3-intro) 
4. [Multiple Predictor Variables](#cs4-intro)
5. Linear Models 
6. Errors, Power and Multiple Comparisons

## Datasets {#index-datasets}

This course uses various data sets. The easiest way of accessing these is by creating an R-project in RStudio. Then download the `data` folder and copy it into your working directory. Your data should then be accessible via `<working-directory-name>/data/raw`.
