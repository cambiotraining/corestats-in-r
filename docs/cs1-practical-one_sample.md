# One-sample tests {#cs1-one-sample-tests}

## Objectives
:::objectives
**Questions**

- When do I perform a one-sample test?
- Which one-sample tests are there and what are the assumptions?
- How do I interpret and present the results of the tests?

**Objectives**

- Set out your hypothesis for single sample continuous data
- Be able to summarise and visualise the data in R
- Understand and assess the underlying assumptions of the tests
- Perform a one-sample t-test and Wilcoxon signed-rank test in R
- Know which test is appropriate when
- Be able to interpret and report the results
:::

## Purpose and aim
These tests are used when we have a single sample of continuous data. It is used to find out if the sample came from a parent distribution with a given mean (or median). This essentially boils down to finding out if the sample mean (or median) is "close enough" to our hypothesised parent population mean (or median).
So, in the figure below, we could use these tests to see what the probability is that the sample of ten points comes from the distribution plotted above it i.e. a population with a mean of 20 mm.

<img src="cs1-practical-one_sample_files/figure-html/cs1-one-sample-intro-1.png" width="672" />

## Choosing a test
There are two tests that we are going to look at in this situation; the one-sample t-test, and the one-sample Wilcoxon signed rank test. Both tests work on the sort of data that we’re considering here, but they both have different assumptions.

If your data is normally distributed, then a one-sample t-test is appropriate. If your data aren’t normally distributed, but their distribution is symmetric, and the sample size is small then a one-sample Wilcoxon signed rank test is more appropriate.

For each statistical test we consider there will be five tasks. These will come back again and again, so pay extra close attention.

:::highlight
1. Setting out of the hypothesis
2. Summarise and visualisation of the data
3. Assessment of assumptions
4. Implementation of the statistical test
5. Interpreting the output and presentation of results
:::

We won’t always carry these out in exactly the same order, but we will always consider each of the five tasks for every test.
