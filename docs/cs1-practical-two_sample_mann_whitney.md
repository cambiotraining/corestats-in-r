


# Mann-Whitney U test {#cs1-mannwhitney-u-test}
This test also compares two samples, however for this test (in contrast to Student’s t-test) we don’t have to assume that the parent distributions are normally distributed. In order to compare the medians of the two groups we do still need the parent distributions (and consequently the samples) to both have the same shape and variance. In this test we look to see if the medians of the two parent distributions differ significantly from each other.

## Section commands
No new commands used in this section.

## Data and hypotheses
Again, we use the `rivers` dataset. We want to test whether the median body length of male guppies differs between samples. We form the following null and alternative hypotheses:

-	$H_0$: The difference in median body length between the two groups is 0 ($\mu A - \mu G = 0$)
-	H1: The difference in median body length between the two groups is not 0 ($\mu A - \mu G \neq 0$)

We use a two-tailed Mann-Whitney U test to see if we can reject the null hypothesis.

## Summarise and visualise
We did this in the [previous section](#cs1-students-sumvisual).

## Implement test
Perform a two-tailed, Mann-Whitney U test:


```r
wilcox.test(length ~ river, data = rivers,
            alternative = "two.sided")
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  length by river
## W = 841, p-value = 0.0006464
## alternative hypothesis: true location shift is not equal to 0
```
In this case, with the data in a tidy format:

-	The first argument must be in the formula format: `variable ~ category`
-	The second argument must be the name of the data frame
-	The third argument gives the type of alternative hypothesis and must be one of `two.sided`, `greater` or `less` 

## Interpret output and report results
You may get a warning message in the console stating `cannot compute exact p-value with ties`. This just means that some of the data points have exactly the same value which affects the internal mathematics slightly. However, given that the p-value is so very small, this is not something that we need to worry about.

After the warning message:

-	The 1st line gives the name of the test and the 2nd line reminds you what the dataset was called, and what variables were used
-	The 3rd line contains the two key outputs from the test:
    - The calculated W-value is 841 (we’ll use this in reporting)
    - The p-value is 0.0006464. 
-	The 4th line simply states the alternative hypothesis in terms of the difference between the two sample medians in that if there were a difference then one distribution would be shifted relative to the other. 

Given that the p-value is less than 0.05 we can reject the null hypothesis at this confidence level.
Again, the p-value on the 3rd line is what we’re most interested in. Since the p-value is very small (much smaller than the standard significance level) we choose to say "that it is very unlikely that these two samples came from the same parent distribution and as such we can reject our null hypothesis".

To put it more completely, we can state that:

> A Mann-Whitney test indicated that the median body length of male guppies in the Guanapo river (18.8 mm) differs significantly from the median body length of male guppies in the Aripo river (20.1 mm) (W = 841, p = 0.0006).

## Assumptions
We have checked these previously.

## Exercise
:::exercise
Analyse the turtle dataset from before using a Mann Whitney test.

We follow the same process as with Student's t-test.

<details><summary>Answer</summary>

**Hypotheses**

$H_0$ : male median $=$ female median

$H_1$ : male median $\neq$ female median

**Summarise and visualise**

This is the same as before.

**Assumptions**

We've already checked that the variances of the two groups are similar, so we're OK there. Whilst the Mann-Whitney test doesn't require normality or symmetry of distributions it does require that the distributions have the same shape. In this example, with just a handful of data points in each group, it's quite hard to make this call one way or another. My advice in this case would be say that unless it's obvious that the distributions are very different we can just allow this assumption to pass, and you're only going see obvious differences in distribution shape when you have considerably more data points than we have here.

**Carry out a Mann-Whitney test**


```r
wilcox.test(serum ~ sex, data = turtle,
            alternative = "two.sided")
```

```
## 
## 	Wilcoxon rank sum exact test
## 
## data:  serum by sex
## W = 26, p-value = 0.5338
## alternative hypothesis: true location shift is not equal to 0
```

This gives us exactly the same conclusion that we got from the two-sample t-test _i.e_. that there isn't any significant difference between the two groups.

A Mann-Whitney test indicated that there wasn't a significant difference in the median Serum Cholesterol levels between male and female turtles (W = 26, p = 0.534)

</details>
:::
