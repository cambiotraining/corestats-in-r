

# Paired two-sample t-test {#cs1-paired-two-sample-t-test}
A paired t-test is used when we have two samples of continuous data that can be paired (examples of these sort of data would be weights of individuals before and after a diet). This test is applicable if the number of paired points within the samples is large (>30) or, if the number of points is small, then this test also works when the parent distributions are normally distributed.

## Section commands
There are no new commands in this section.

## Data and hypotheses
For example, suppose we measure the cortisol levels in 20 adult females (nmol/l) first thing in the morning and again in the evening. We want to test whether the cortisol levels differs between the two measurement times. We will initially form the following null and alternative hypotheses:

- $H_0$: There is no difference in cortisol level between times ($\mu M = \mu E$)
-	$H_1$: There is a difference in cortisol levels between times ($\mu M \neq \mu E$)

We use a two-sample, two-tailed paired t-test to see if we can reject the null hypothesis.

-	We use a **two-sample** test because we now have two samples
-	We use a **two-tailed** t-test because we want to know if our data suggest that the true (population) means are different from one another rather than that one mean is specifically bigger or smaller than the other
-	We use a **paired** test because each data point in the first sample can be linked to another data point in the second sample by a connecting factor
-	We’re using a **t-test** because we’re assuming that the parent populations are normal and have equal variance (We’ll check this in a bit)

The data are stored in unstacked format in the file “CS1-twopaired.csv”.
Read this into R:


```r
cortisol <- read.csv("data/raw/CS1-twopaired.csv")
```

## Summarise and visualise


```r
summary(cortisol)
```

```
##     morning         evening     
##  Min.   :146.1   Min.   : 60.1  
##  1st Qu.:266.6   1st Qu.:137.8  
##  Median :320.5   Median :188.9  
##  Mean   :313.5   Mean   :197.4  
##  3rd Qu.:359.7   3rd Qu.:260.8  
##  Max.   :432.5   Max.   :379.3
```

```r
boxplot(cortisol, ylab = "Level (nmol/l)")
```

<img src="cs1-practical-two_sample_paired_t_test_files/figure-html/cs1-pairedt-sumvisual-1.png" width="672" />

The box plot does not capture how the cortisol level of each individual subject has changed though. We can explore the individual changes between morning and evening by creating a boxplot of the _differences_ between the two times of measurement.


```r
# calculate the difference between evening and morning values
changeCor <- cortisol$evening - cortisol$morning

boxplot(changeCor, ylab = "Change in cortisol (nmol/l)")
```

<img src="cs1-practical-two_sample_paired_t_test_files/figure-html/cs1-pairedt-diff-1.png" width="672" />

The differences in cortisol levels appear to be very much less than zero, (meaning that the evening cortisol levels appear to be much lower than the morning ones). As such we would expect that the test would give a pretty significant result.

## Assumptions
You will do this in the exercise!

## Implement test
Perform a two-sample, two-tailed, paired t-test:


```r
t.test(cortisol$evening, cortisol$morning,
       alternative = "two.sided", paired = TRUE)
```

-	The first two arguments must be vectors containing the numerical data for both samples
-	The third argument gives the type of alternative hypothesis and must be one of `two.sided`, `greater` or `less` 
-	The fourth argument says that the data are paired

## Interpret output and report results

```
## 
## 	Paired t-test
## 
## data:  cortisol$evening and cortisol$morning
## t = -5.1833, df = 19, p-value = 5.288e-05
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -162.96038  -69.20962
## sample estimates:
## mean of the differences 
##                -116.085
```

From our perspective the value of interested is on the 3rd line (p-value = 5.288e10<sup>-5</sup>). Given that this is substantially less than 0.05 we can reject the null hypothesis and state:

> A two-tailed, paired t-test indicated that the cortisol level in adult females differed significantly between the morning (313.5 nmol/l) and the evening (197.4 nmol/l) (t = -5.1833, df = 19, p = 5.3x10<sup>-5</sup>).

## Exercise: Assumptions
:::exercise
Checking assumptions

Check the assumptions necessary for this this paired t-test.
Was a paired t-test an appropriate test?

<details><summary>Answer</summary>

A paired test is really just a one-sample test in disguise. We actually don't care too much about the distributions of the individual groups. Instead we care about the properties of the **differences**. So for a paired t-test to be valid for this dataset, we need the differences between the morning and evening values to be normally distributed.

Let's check this with Shapiro-Wilk and Q-Q plots using the `changeCor` variable we created earlier.


```r
shapiro.test(changeCor)
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  changeCor
## W = 0.92362, p-value = 0.1164
```

```r
qqnorm(changeCor)
qqline(changeCor, col = "red")
```

<img src="cs1-practical-two_sample_paired_t_test_files/figure-html/unnamed-chunk-2-1.png" width="672" />

The Shapiro-Wilk test says that the data are normal enough and whilst the Q-Q plot is mostly fine, there is some suggestion of snaking at the bottom left. I'm actually OK with this because the suggestion of snaking is actually only due to a single point (the last point on the left). If you cover that point up with your thumb (or finger of your choice) then the remaining points in the Q-Q plot look pretty damn good, and so the suggestion of snaking is actually driven by only a single point (which can happen by chance). As such I'm actually happy that the assumption of normality is well met in this case. This **single point** check is a useful thing to remember when assessing diagnostic plots.

So, yep, a paired t-test is appropriate for this dataset.

</details>
:::
