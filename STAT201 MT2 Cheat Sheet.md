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

**The General Idea**
- Describe the Law of Large Numbers.
- Describe a normal distribution.
- Explain the Central Limit Theorem and other general asymptotic results (such as for quantiles), and their role in constructing confidence intervals.
- Write a computer script to calculate confidence intervals based on the assumption of normality or the Central Limit Theorem.
- Discuss the potential limitations of these methods.
- Decide whether to use asymptotic theory or bootstrapping to compute estimator uncertainty.

### Normal(Gaussian) distribution

**Fitting a normal curve on a normal distribution:**

```r
pop_dist_normal <- 
    data %>% 
    ggplot() + 
    geom_histogram(aes(x = RESPONSEVAR , y = ..density..), color = 'white', binwidth = 0.198)+
    theme(text = element_text(size=25)) +
    xlab("XLABEL") +
    ylab("Count") + 
    ggtitle("DISTRIBUTION_TITLE")+
    geom_line(data = data_normal, aes(x = RESPONSEVAR, y = density), color="red", lwd = 2)
```

This should look something like:

![alt text](https://github.com/JordanTreiv/STAT201CheatSheet/blob/main/download.png)


**Characteristics of a Normal Curve/Distribution:**

The Normal Curve is described by N(μ,σ) such that:

μ (Mu): Describes the center/mean of the curve. Controls the location of the curve.

σ (Sigma): Describes the standard devaition/spread of the curve. Controls its spread. As σ increases, the curve becomes wider, and vice versa

------------------------------------------------------------------------------------
- Approximately 68% of the observations are between [μ - 1σ ; μ + 1σ]
- Approximately 95% of the observations are between [μ - 2σ ; μ + 2σ]
- Approximately 99.7% of the observations are between [μ - 3σ ; μ + 3σ]

Z* for:
- 1xσ: 1
- 2xσ: 1.96
- 3xσ: 2.58

![alt text](https://github.com/JordanTreiv/STAT201CheatSheet/blob/main/download%20(1).png)


The above works very well for Normal Distributions, however, when the distribution of the population/sample is not normal, we need to consider the
Central Limit Theorum

### Central Limit Theorum (CLT)

**The Main Idea:**
Allows us to approximate the sampling distribution of many estimators by a Normal distribution, even when the population distribution is not Normal

This works by given a sufficiently large sample size, the sampling distribution of the mean for a variable will approximate a normal distribution 
regardless of that variable’s distribution in the population.

**The Code to do it:**

```r
samples_size10 <- weird_population %>%
    rep_sample_n(reps = 3000, size = 10)
```

```r
sampling_dist_size500 <-
    samples_size500 %>% 
    group_by(replicate) %>% 
    summarise(sample_mean = mean(value), `.groups` = "drop") %>% 
    ggplot() + 
    geom_histogram(aes(x = sample_mean, y = ..density..), color="white") +
    theme(text = element_text(size = 25))+
    xlab("Sample Means") +
    ggtitle("Sampling distribution of the sample mean for samples of size 500 from Weird Population.") + 
    geom_line(data = gaussian_densities %>% filter(sample_size == 500), aes(value, density), color = "red", lwd = 2) # Adds the red normal dist line
```

It is important to notice that CLT is not magic -- you should not automatically rely on CLT. There are three main things you need to check:

- Is the size of your sample large enough?
- Was the sample taken in an independent fashion?
- Is the estimator being used a sum of random components?

#### Confidence Intervals from CLT

**Steps:**
1. Take a sample; 
2. Construct the bootstrap sampling distribution.
3.Get the quantiles from the estimated sampling distribution.

**The Following is for a Continuous variable:**

To estimate the mean, we use the sample average, X. The CLT roughly says that X follows a Normal distribution with parameters μ and σ√n:

X ~ N(μ,σ√n)

Since we do not know the population μ and σ, we use their estimates: x, and s. You can acquire s with ``sd(variable)``.

**Confidence Intervals like: **

By ``qnorm()``:

```r
mean_body_mass_adelie_ci <-
    tibble(
         lower_ci = qnorm(0.025, mean, std_error),
         upper_ci = qnorm(0.975, mean, std_error)
     )
# standard error is acquired by: sd(variable) / sqrt(n)
# where n = sample size
```


By ``infer``: 

```r
bootstrap_ci <- 
    data %>% 
    filter(WTVR_IS_RELEVENT) %>% 
    specify(response = RESPONSE_VARIABLE) %>% 
    generate(type = "bootstrap", reps = WTVR_NUMBER) %>% 
    calculate(stat = ENTER_TYPE_HERE) %>% 
    get_ci()
bootstrap_ci
```

----------------------------------------------------------------------------

**The Following is for a Discrete variable:**

A proportion is the average of a random variable that can only assume either 0 or 1. Therefore, by calculating proportions, you are summing up random terms, and we can apply the CLT. The CLT for proportions states that the sample proportion follows a Normal distribution with mean equals to 
p,the population proportion, and standard deviation √p(1−p)/n:

p̂ ~ N(p,√(p(1-p)/n))

Given by:

```r
mean_body_mass_adelie_ci <-
    tibble(
         lower_ci = qnorm(0.025, phat, std_error),
         upper_ci = qnorm(0.975, phat, std_error)
     )
# standard error is acquired by: sqrt(phat * (1 - phat) / length(DATAFRAME)
# phat is acquired by: phat <- mean(VARIABLE > 4000) [for a greater than prop test]
```
-----------------------------------------------------------------------------
**Doing Confidence Intervals for  Differences in Summary Stats with CLT**

![alt text](https://github.com/JordanTreiv/STAT201CheatSheet/blob/main/Screen%20Shot%202022-08-04%20at%2012.46.48%20PM.png)

Code for difference in stats Confidence interval based on CLT:

```r
 diff_means_ci <- 
     tibble(
         lower_ci = mean(data1) - mean(data2) + qnorm(0.025, 0, 1) * sqrt(var(data1)/length(data1) + var(data2)/length(data2)),
         upper_ci = mean(data1) - mean(data2) - qnorm(0.025, 0, 1) * sqrt(var(data1)/length(data1) + var(data2)/length(data2))
     )
```




## Module 8: Classical Tests Based on Normal and t-Distributions

**General Ideas:**

- Describe the t-distribution family and its relationship with the normal distribution.
- Use results from the assumption of normality or the Central Limit Theorem to perform estimation and hypothesis testing.
- Compare and contrast the parts of estimation and hypothesis testing that differ between simulation- and resampling-based approaches with the assumption of normality or the Central Limit Theorem- based approaches.
- Write a computer script to perform hypothesis testing based on results from the assumption of normality or the Central Limit Theorem.
- Discuss the potential limitations of these methods.


A Two Sample T-Test (Two Tail) is for comparing 2 independent populations, otherwise, a t test is used for when the population paramaeters are unknown and the standard deviation is approximated by using a value from a sample:


If the population standard deviation is known and the sample size is greater than 30, Z-test is recommended to be used. If the population standard deviation is known, and the size of the sample is less than or equal to 30, T-test is recommended.










