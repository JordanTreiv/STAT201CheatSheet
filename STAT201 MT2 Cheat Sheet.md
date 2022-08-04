# STAT201 MT2 Cheat Sheet

## Module 6: Hypothesis Testing
***The Basic Steps***
- State hypotheses
- Set significance level
- Collect data
- Calculate test statistic
- Calculate P-value
- Draw a conclusion

**State Hypotheses**

In general, we have two hypothesis 𝐻0, called the null hypothesis, and 𝐻1 (or 𝐻𝐴), the alternative hypothesis.

The null hypothesis is the status quo, what you should conclude if there’s no evidence to say otherwise.
For example unless we see "strong" evidence that a new, untested drug works, we are going to assume that it does not.

Typically, the null is of the form:

𝐻0 = ***p*** ; for some p Population Parameter

And the alternative is of the form: 

𝐻𝐴 < ***p*** ; for some p Population Parameter

𝐻𝐴 > ***p*** ; for some p Population Parameter

𝐻𝐴 ≠ ***p*** ; for some p Population Parameter

### Code(Module 6)

**STEP 1:**

Identify Hypotheses:

𝐻0 = x

-------------------------------------------------------------

𝐻𝐴 < x

*or*

𝐻𝐴 > x

*or*

𝐻𝐴 ≠ x


Identify Response and Explanatory variables:

x = Response

y = Explanatory

**The Code:**

First, we find the *observed test statistic* from the original sample

```r
obs_test_stat <- data %>% 
  specify(Response ~ Explanatory, success = someCategoricalFactor %>% 
  calculate(stat = "ENTER_TYPE_HERE")
obs_test_stat
```

*(The following is for a categorical variable)*

We start the null model by using *specify* to identify response and/or explanatory variables:

```r
null_model_dist <- data %>%
specify(formula = Response ~ Explanatory, success = someCategoricalFactor)
```
Next, we create the null and alternative *hypotheses*:

Note, for ``hypothesize:"" `` we enter:

"independence" for when there are 2 samples

"point" for wheb there is 1 sample.

```r
null_model_dist <- data %>%
specify(formula = Response ~ Explanatory, success = someCategoricalFactor)%>%
hypothesize(null = "ENTER_TYPE_HERE")
```

Next, we *generate* replicates:

Note, for ``generate:"" `` we enter:

"bootstrap" for when we replicate ***WITH*** replacement

"permute" for when we replicate ***WITHOUT*** replacement

```r 
null_model_dist <- data %>%
specify(formula = Response ~ Explanatory, success = someCategoricalFactor)%>%
hypothesize(null = "ENTER_TYPE_HERE")%>%
generate(reps = DESIRED_REPLICATES, type = "ENTER_TYPE_HER")
```
After this, we *calculate* summary statistics:

Note, for ``calculate(stat= "") `` we enter:

"mean" - for calculating the means

"median" - for calculating the medians

"sum" - for calculating the summs

"sd" - for calculating the standard deviations

"prop" - for calculating the proportions

"count" - for calculating the total counts

"diff in means" - for calculating the difference in means **(ONLY FOR ``hypothesize(null = "independence"))`` )**

"diff in medians" - for calculating the difference in medians **(ONLY FOR ``hypothesize(null = "independence))`` )**

"diff in props" - for calculating the difference in proportions **(ONLY FOR ``hypothesize(null = "independence))`` )** 


```r
null_model_dist <- data %>%
specify(formula = Response ~ Explanatory, success = someCategoricalFactor)%>%
hypothesize(null = "ENTER_TYPE_HERE")%>%
generate(reps = DESIRED_REPLICATES, type = "ENTER_TYPE_HER")%>%
calculate(stat = "ENTER_TYPE_HERE")
# for calculate(stat = "diff in SMTH"), you can add the order of taking the difference like:
# calculate(stat = "diff in props", order = c("var1", "var2"))
```

To *visualize* the p-value, we do:

```r
null_model_dist_vis <- 
visualize(null_model_dist, bins = 10)
```

We can further shade in the region with the p-value depending on our Alternative Hypothesis:

Note, for ``shade_p_value(obs_stat = obs_test_stat, direction = "")``, for direction, we enter:

"right" - 𝐻𝐴 > ***p*** 

"left" - 𝐻𝐴 < ***p*** 

"both" - for when 𝐻𝐴 ≠ ***p***

```r
null_model_dist_vis <- 
visualize(null_model_dist, bins = 10)%>%
shade_p_value(obs_stat = obs_test_stat, direction = "ENTER_TYPE_HERE")
```
It should look something like this (for direction = "right"):

![alt text](https://github.com/JordanTreiv/STAT201CheatSheet/blob/main/null-distribution-infer-2-1.png)

Finally, we need to get our p-value, which we will compare with our significance level, and then determine wether or not to reject the null

```r
get_p_value(obs_stat = obs_test_stat, direction = "ENTER_TYPE_HERE")
```

Remember, this will spit out a *percentage* which represents the probability of getting a value *more extreme* than the test statistic.
If the p-value is smaller than the ⍺ level(significance level), we *reject the null*. Otherwise, we do not.

From here, if we so wish, we can easily continue to making confidence intervals with a few easy changes:

```r
bootstrap_distribution <- data %>% 
specify(formula = Response ~ Explanatory, success = someCategoricalFactor)%>% 
# Change 1 - Remove hypothesize():
# hypothesize(null = "ENTER_TYPE_HERE") %>% 
# Change 2 - make the generate type = "bootstrap":
generate(reps = 1000, type = "bootstrap") %>% 
calculate(stat = "ENTER_TYPE_HERE")%>%
```

```r
percentile_ci <- bootstrap_distribution %>% 
  get_confidence_interval(level = ENTER_CONFIDENCE_LEVEL_HERE, type = "percentile")
```

Next, we visualize the confidence interval:

```r
ci_vis <-
visualize(bootstrap_distribution) + 
  shade_confidence_interval(endpoints = percentile_ci)
```

It should look like:

![alt text](https://github.com/JordanTreiv/STAT201CheatSheet/blob/main/bootstrap-distribution-two-prop-percentile-1.png)



--------------------------------------------------------------------------------------------



*(The following is for a continuous variable)*

We start the null model by using *specify* to identify response and/or explanatory variables:

```r
null_model_dist <- data %>%
specify(response = Response)
```
Next, we create the null and alternative *hypotheses*:

Note, for ``hypothesize:"" `` we enter:

"independence" for when there are 2 samples

"point" for when there is 1 sample.

```r
null_model_dist <- data %>%
specify(response = Response) %>% 
hypothesize(null = "ENTER_TYPE_HERE")
```

Next, we *generate* replicates:

Note, for ``generate:"" `` we enter:

"bootstrap" for when we replicate ***WITH*** replacement

"permute" for when we replicate ***WITHOUT*** replacement

```r
null_model_dist <- data %>%
specify(response = Response)%>%
hypothesize(null = "ENTER_TYPE_HERE")%>%
generate(reps = DESIRED_REPLICATES, type = "ENTER_TYPE_HER")
```

After this, we *calculate* summary statistics:

Note, for ``calculate(stat= "") `` we enter:

"mean" - for calculating the means

"median" - for calculating the medians

"sum" - for calculating the summs

"sd" - for calculating the standard deviations

"prop" - for calculating the proportions

"count" - for calculating the total counts

"diff in means" - for calculating the difference in means **(ONLY FOR ``hypothesize(null = "independence))`` )**

"diff in medians" - for calculating the difference in medians **(ONLY FOR ``hypothesize(null = "independence))`` )**

"diff in props" - for calculating the difference in proportions **(ONLY FOR ``hypothesize(null = "independence))`` )** 

```r
null_model_dist <- data %>%
specify(response = Response)%>%
hypothesize(null = "ENTER_TYPE_HERE")%>%
generate(reps = DESIRED_REPLICATES, type = "ENTER_TYPE_HER")%>%
calculate(stat = "ENTER_TYPE_HERE")
# for calculate(stat = "diff in SMTH"), you can add the order of taking the difference like:
# calculate(stat = "diff in props", order = c("var1", "var2"))
```

To *visualize* the p-value, we do:

```r
null_model_dist_vis <- 
visualize(null_model_dist, bins = 10)
```

We can further shade in the region with the p-value depending on our Alternative Hypothesis:

Note, for ``shade_p_value(obs_stat = obs_test_stat, direction = "")``, for direction, we enter:

"right" - 𝐻𝐴 > ***p*** 

"left" - 𝐻𝐴 < ***p*** 

"both" - for when 𝐻𝐴 ≠ ***p***

```r
null_model_dist_vis <- 
visualize(null_model_dist, bins = 10)%>%
shade_p_value(obs_stat = obs_test_stat, direction = "ENTER_TYPE_HERE")
```

It should look something like this (for direction = "right"):

![alt text](https://github.com/JordanTreiv/STAT201CheatSheet/blob/main/null-distribution-infer-2-1.png)

Finally, we need to get our p-value, which we will compare with our significance level, and then determine wether or not to reject the null

```r
get_p_value(obs_stat = obs_test_stat, direction = "ENTER_TYPE_HERE")
```

Remember, this will spit out a *percentage* which represents the probability of getting a value *more extreme* than the test statistic.
If the p-value is smaller than the ⍺ level(significance level), we *reject the null*. Otherwise, we do not.


From here, if we so wish, we can easily continue to making confidence intervals with a few easy changes:

```r
bootstrap_distribution <- data %>% 
specify(response = Response)%>% 
# Change 1 - Remove hypothesize():
# hypothesize(null = "ENTER_TYPE_HERE") %>% 
# Change 2 - make the generate type = "bootstrap":
generate(reps = 1000, type = "bootstrap") %>% 
calculate(stat = "ENTER_TYPE_HERE")%>%
```

```r
percentile_ci <- bootstrap_distribution %>% 
  get_confidence_interval(level = ENTER_CONFIDENCE_LEVEL_HERE, type = "percentile")
```

Next, we visualize the confidence interval:

```r
ci_vis <-
visualize(bootstrap_distribution) + 
  shade_confidence_interval(endpoints = percentile_ci)
```

It should look like:

![alt text](https://github.com/JordanTreiv/STAT201CheatSheet/blob/main/bootstrap-distribution-two-prop-percentile-1.png)


## Module 7: Confidence Intervals (of means and proportions) based on the assumption of Normality or the Central Limit Theorem


## Module 8: Theory-based Hypothesis Tests
