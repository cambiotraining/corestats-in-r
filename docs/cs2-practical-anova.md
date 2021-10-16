# ANOVA {#cs2-anova}

## Objectives
:::objectives
**Questions**

- How do I...
- What do I...

**Objectives**

- Be able to...
- Use...
:::

## Purpose and aim
Analysis of variance or ANOVA is a test than can be used when we have multiple samples of continuous data. Whilst it is possible to use ANOVA with only two samples, it is generally used when we have three or more groups. It is used to find out if the samples came from parent distributions with the same mean. It can be thought of as a generalisation of the two-sample Student’s t-test.

### Section commands
New commands used in this section.

| Function| Description|
|:- |:- |
|`lm()`| Fits a linear model |
|`anova()`| Carries out an ANOVA on a linear model |

### Data and hypotheses
For example, suppose we measure the feeding rate of oyster catchers (shellfish per hour) at three sites characterised by their degree of shelter from the wind, imaginatively called `exposed` (E), `partially sheltered` (P) and `sheltered` (S). We want to test whether the data support the hypothesis that feeding rates don’t differ between locations. We form the following null and alternative hypotheses:

-	$H_0$: The mean feeding rates at all three sites is the same $\mu E = \mu P = \mu S$
-	$H_1$: The mean feeding rates are not all equal.

We will use a one-way ANOVA test to check this.

-	We use a **one-way** ANOVA test because we only have one predictor variable (the categorical variable location).
-	We’re using **ANOVA** because we have more than two groups and we don’t know any better yet with respect to the exact assumptions.

The data are stored in the file `CS2-oystercatcher.csv`.

### Summarise and visualise
First we read in the data.


```r
oystercatcher <- read.csv("data/raw/CS2-oystercatcher.csv")
```

Next we summarise the data and visualise them. We have a quick peek at the first few rows of our data with `head()` so we can see how the data are organised.

The data are in stacked format. The first column contains information on feeding rates and is called `feeding.` The second column has categorical data on the type of site and is called `site`.


```r
head(oystercatcher)
```

```
##   feeding    site
## 1    14.2 Exposed
## 2    16.5 Exposed
## 3     9.3 Exposed
## 4    15.1 Exposed
## 5    13.4 Exposed
## 6    18.4 Partial
```

```r
aggregate(feeding ~ site, data = oystercatcher, summary)
```

```
##        site feeding.Min. feeding.1st Qu. feeding.Median feeding.Mean
## 1   Exposed         9.30           13.40          14.20        13.70
## 2   Partial        13.00           16.50          17.40        17.14
## 3 Sheltered        21.50           22.20          24.10        23.64
##   feeding.3rd Qu. feeding.Max.
## 1           15.10        16.50
## 2           18.40        20.40
## 3           25.10        25.30
```

```r
boxplot(feeding ~ site, data = oystercatcher)
```

<img src="cs2-practical-anova_files/figure-html/unnamed-chunk-2-1.png" width="672" />

Looking at the data, there appears to be a noticeable difference in feeding rates between the three sites. We would probably expect a reasonably significant statistical result here.

### Implement test
Perform an ANOVA test on the data:


```r
lm_oystercatcher <- lm(feeding ~ site, data = oystercatcher)

anova(lm_oystercatcher)
```

The first line fits a linear model to the data (i.e. finds the means of the three groups and calculates a load of intermediary data that we need for the statistical analysis) and stores this information in an R object (which I’ve called `lm_oystercatchers`, but which you can call what you like). The second line actually carries out the ANOVA analysis.

-	The first argument must be in the formula format: `response ~ predictor`
-	If the data are stored in stacked format, then the second argument must be the name of the data frame
-	The `anova()` command takes a linear model object as its main argument

### Interpret output and report results
This is the output that you should now see in the console window:


```
## Analysis of Variance Table
## 
## Response: feeding
##           Df  Sum Sq Mean Sq F value    Pr(>F)    
## site       2 254.812 127.406  21.508 0.0001077 ***
## Residuals 12  71.084   5.924                      
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

-	The 1st line just tells you the that this is an ANOVA test
-	The 2nd line tells you what the response variable is (in this case feeding)
-	The 3rd, 4th and 5th lines are an ANOVA table which contain some useful values:
    - The `Df` column contains the degrees of freedom values on each row, 2 and 12 (which we’ll need for the reporting)
    - The `F` value column contains the F statistic, 21.508 (which again we’ll need for reporting).
    - The p-value is 0.0001077 and is the number directly under the `Pr(>F)` on the 4th line.
    - The other values in the table (in the `Sum Sq` and `Mean Sq`) columns are used to calculate the F statistic itself and we don’t need to know these.
-	The 6th line has some symbolic codes to represent how big (small) the p-value is; so, a p-value smaller than 0.001 would have a *** symbol next to it (which ours does). Whereas if the p-value was between 0.01 and 0.05 then there would simply be a * character next to it, etc. Thankfully we can all cope with actual numbers and don’t need a short-hand code to determine the reporting of our experiments (please tell me that’s true…!)

Again, the p-value is what we’re most interested in here and shows us the probability of getting samples such as ours if the null hypothesis were actually true.

Since the p-value is very small (much smaller than the standard significance level of 0.05) we can say "that it is very unlikely that these three samples came from the same parent distribution" and as such we can reject our null hypothesis and state that:

> A one-way ANOVA showed that the mean feeding rate of oystercatchers differed significantly between locations (F = 21.51, df = 2, 12, p = 0.00011).

Note that we have included (in brackets) information on the test statistic (F = 21.51), both degrees of freedom (df = 2, 12), and the p-value (p = 0.00011).

### Assumptions
To use an ANOVA test, we have to make three assumptions:

1.	The parent distributions from which the samples are taken are normally distributed
2.	Each data point in the samples is independent of the others
3.	The parent distributions should have the same variance

In a similar way to the two-sample tests we will consider the normality and equality of variance assumptions both using tests and by graphical inspection (and ignore the independence assumption).

**1. Normality**

Unstack the data and perform a Shapiro-Wilk test on each group separately.


```r
uns_oyster <- unstack(oystercatcher)

shapiro.test(uns_oyster$Exposed)
shapiro.test(uns_oyster$Partial)
shapiro.test(uns_oyster$Sheltered)
```

This is the output that you should now see in the console window:


```
## 
## 	Shapiro-Wilk normality test
## 
## data:  uns_oyster$Exposed
## W = 0.9151, p-value = 0.4988
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  uns_oyster$Partial
## W = 0.96913, p-value = 0.8697
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  uns_oyster$Sheltered
## W = 0.88532, p-value = 0.3341
```

We can see that all three groups appear to be normally distributed which is good.

For ANOVA however, considering each group in turn is often considered quite excessive and, in most cases, it is sufficient to consider the normality of the combined set of _residuals_ from the data. We’ll explain residuals properly in the [next session](#cs3-intro) but effectively they are the difference between each data point and its group mean. The residuals can be obtained directly from the linear model we fitted earlier.

Extract the residuals from the data and check their normality:



```r
resid_oyster <- residuals(lm_oystercatcher)

shapiro.test(resid_oyster)
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  resid_oyster
## W = 0.93592, p-value = 0.3338
```
Again, we can see that the combined residuals from all three groups appear to be normally distributed (which is as we would have expected given that they were all normally distributed individually!)

**2. Equality of Variance**

We now test for equality of variance using Bartlett’s test (since we’ve just found that all of the individual groups are normally distributed).

Perform Bartlett’s test on the data:


```r
bartlett.test(feeding ~ site, data = oystercatcher)
```

```
## 
## 	Bartlett test of homogeneity of variances
## 
## data:  feeding by site
## Bartlett's K-squared = 0.90632, df = 2, p-value = 0.6356
```

Where the relevant p-value is given on the 3rd line. Here we see that each group do appear to have the same variance.

**3. Graphical Interpretation and Diagnostic Plots**

R provides a convenient set of graphs that allow us to assess these assumptions graphically. If we simply ask R to plot the `lm` object we have created, then we can see some of these _diagnostic plots_.

Create the standard set diagnostic plots:


```r
# create a neat 2x2 window
par(mfrow = c(2,2))
# create the diagnostic plots
plot(lm_oystercatcher)
```

```r
# and return the window back to normal
par(mfrow = c(1,1))
```


```
## hat values (leverages) are all = 0.2
##  and there are no factor predictors; no plot no. 5
```

<img src="cs2-practical-anova_files/figure-html/cs2-anova-oyster-diagnostic-results-1.png" width="672" />

The second line creates the three diagnostic plots (it actually tries to create four plots but can’t do that for this dataset so you’ll also see some warning text output to the screen (something about hat values). I’ll go through this in the next session where it’s easier to explain).

-	In this example, two of the plots (top-left and bottom-left) show effectively the same thing: what the distribution of data between each group look like. These allow an informal check on the equality of variance assumption.
    - For the top-left graph we want all data to be symmetric about the 0 horizontal line and for the spread to be the same (please ignore the red line; it is an unhelpful addition to these graphs).
    - For the bottom-left graph, we will look at the red line as we want it to be approximately horizontal.
- The top-right graph is a familiar Q-Q plot that we used previously to assess normality, and this looks at the combined residuals from all of the groups (in much the same way as we looked at the Shapiro-Wilk test on the combined residuals).

We can see that these graphs are very much in line with what we’ve just looked at using the test, which is reassuring. The groups all appear to have the same spread of data, and whilst the QQ-plot isn’t perfect, it appears that the assumption of normality is allright.

:::note
At this stage, I should point out that I nearly always stick with the graphical method for assessing the assumptions of a test. Assumptions are rarely either completely met or not met and there is always some degree of personal assessment.

Whilst the formal statistical tests (like Shapiro) are technically fine, they can often create a false sense of things being absolutely right or wrong in spite of the fact that they themselves are still probabilistic statistical tests. In these exercises we are using both approaches whilst you gain confidence and experience in interpreting the graphical output and whilst it is absolutely fine to use both in the future I would strongly recommend that you don’t rely solely on the statistical tests in isolation.
:::

### Exercise
:::exercise
Exercise title

Exercise description

<details><summary>Answer</summary>

An elaborate answer

</details>
:::

## Key points

:::keypoints
- Point 1
- Point 2
- Point 3
:::
