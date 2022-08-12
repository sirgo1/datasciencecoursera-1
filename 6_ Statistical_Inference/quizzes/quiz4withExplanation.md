# Statistical Inference Quiz 4 (JHU) Coursera

Question 1
----------
A pharmaceutical company is interested in testing a potential blood pressure lowering medication. Their first examination considers only subjects that received the medication at baseline then two weeks later. The data are as follows (SBP in mmHg)

Subject | Baseline | Week 2
--- | --- | ---
1 |	140 |	132
2	| 138 |	135
3	| 150 |	151
4 |	148 |	146
5 |	135 |	130

Consider testing the hypothesis that there was a mean reduction in blood pressure? Give the P-value for the associated two sided T test.

(Hint, consider that the observations are paired.)

* 0.05

* 0.087

* 0.043

* 0.10

Answer: </br>

```{r}
subject <- c(1,2,3,4,5)
baseline <- c(140,138,150,148,135)
week2 <- c(132,135,151,146,130)
examinations <- data.frame(subject, baseline, week2)
test <- t.test(x = examinations$baseline, y = examinations$week2, alt = "two.sided", paired = TRUE)
pval <- test$p.value
round(pval,3)
```
OR
H0​:μd​=0 versus H0:μd≠0 H_0 : \mu_d \neq 0 H0​:μd​​=0 where μd \mu_d μd​ is the mean difference between followup and baseline.
```{r}
bl <- c(140, 138, 150, 148, 135)
fu <- c(132, 135, 151, 146, 130)
t.test(fu, bl, alternative = "two.sided", paired = TRUE)
```
OR
```{r}
t.test(fu - bl, alternative = "two.sided")
```
```{r}
# [1] 0.087
```


Note the difference if the test were one sided
```{r}
t.test(fu, bl, alternative = "less", paired = TRUE)
````


Question 2
----------
A sample of 9 men yielded a sample average brain volume of 1,100cc and a standard deviation of 30cc. What is the complete set of values of μ0 that a test of H0:μ=μ<sub>0</sub> would fail to reject the null hypothesis in a two sided 5% Students t-test?

* 1081 to 1119

* 1080 to 1120

* 1031 to 1169

* 1077 to 1123

Answer: </br>

```{r}
n <- 9
μ <- 1100
σ <- 30
quantile = 0.975 # is 95% with 2.5% on both sides of the range
confidenceInterval = μ + c(-1, 1) * qt(quantile, df=n-1) * σ / sqrt(n)
```
or
Let p p p be the proportion of people who prefer Coke. Then, we want to test

H0:p=.5 H_0:p=.5 H0​:p=.5 versus Ha:p>.5 H_a:p>.5 Ha​:p>.5. Let X X X be the number out of 4 4 4 that prefer

Coke; assume X∼Binomial(p,.5) X \sim Binomial(p, .5) X∼Binomial(p,.5).

Pvalue=P(X≥3)=choose(4,3)0.530.51+choose(4,4)0.540.50 Pvalue = P(X \geq 3) =  \mathrm{choose}(4,3) 0.5 ^ 3 0.5 ^ 1 +  \mathrm{choose}(4, 4) 0.5 ^4 0.5^0 Pvalue=P(X≥3)=choose(4,3)0.530.51+choose(4,4)0.540.50
```{r}
pbinom(2, size = 4, prob = 0.5, lower.tail = FALSE)
choose(4, 3) * 0.5^4 + choose(4, 4) * 0.5^4
---
```{r}
# [1] 1077 1123
```

Question 3
----------
Researchers conducted a blind taste test of Coke versus Pepsi. Each of four people was asked which of two blinded drinks given in random order that they preferred. The data was such that 3 of the 4 people chose Coke. Assuming that this sample is representative, report a P-value for a test of the hypothesis that Coke is preferred to Pepsi using a one sided exact test.

* 0.10

* 0.005

* 0.31

* 0.62

Answer: </br>

```{r}
n <- 4
x <- 3
test <- binom.test(x=x, n=n, alt="greater")
round(test$p.value,2)
```
or
Correct

H0:λ=0.01 H_0 : \lambda = 0.01 H0​:λ=0.01 versus Ha:λ<0.01 H_a : \lambda < 0.01 Ha​:λ<0.01. X=11 X = 11 X=11, t=1,787 t=1,787 t=1,787 and assume X∼H0Poisson(0.01×t) X \sim_{H_0} Poisson(0.01 \times t) X∼H0​​Poisson(0.01×t)
```{r}
ppois(10, lambda = 0.01 * 1787)
```
```{r}
# [1] 0.31
```

Question 4
----------
Infection rates at a hospital above 1 infection per 100 person days at risk are believed to be too high and are used as a benchmark. A hospital that had previously been above the benchmark recently had 10 infections over the last 1,787 person days at risk. About what is the one sided P-value for the relevant test of whether the hospital is *below* the standard?

* 0.22

* 0.52

* 0.11

* 0.03

Answer: </br>

```{r}
rate <- 1/100
errors <- 10
days <- 1787
test <-  poisson.test(errors, T = days, r = rate, alt="less")
round(test$p.value,2)
```

```
# [1] 0.03
```

Question 5
----------
Suppose that 18 obese subjects were randomized, 9 each, to a new diet pill and a placebo. Subjects’ body mass indices (BMIs) were measured at a baseline and again after having received the treatment or placebo for four weeks. The average difference from follow-up to the baseline (followup - baseline) was −3 kg/m2 for the treated group and 1 kg/m2 for the placebo group. The corresponding standard deviations of the differences was 1.5 kg/m2 for the treatment group and 1.8 kg/m2 for the placebo group. Does the change in BMI appear to differ between the treated and placebo groups? Assuming normality of the underlying data and a common population variance, give a pvalue for a two sided t test.

* Larger than 0.10

* Less than 0.10 but larger than 0.05

* Less than 0.01

* Less than 0.05, but larger than 0.01

Answer: </br>

Less than 0.01

```{r}
n_y <- 9 # subjects treated
n_x <- 9 # subjects placebo
σ_y <- 1.5# kg/m2 std.dev. treated 
σ_x <- 1.8# kg/m2 std.dev. placebo 
μ_y <- -3#  kg/m2 average difference treated
μ_x <- 1#  kg/m2 average difference placebo

# calculate pooled standard deviation
σ_p <- (((n_x - 1) * σ_x^2 + (n_y - 1) * σ_y^2)/(n_x + n_y - 2))
pval <- pt((μ_y - μ_x) / (σ_p * (1 / n_x + 1 / n_y)^.5), df=n_y + n_x -2)
pval
```
or
```{r}
n1 <- n2 <- 9
x1 <- -3 ##treated
x2 <- 1 ##placebo
s1 <- 1.5 ##treated
s2 <- 1.8 ##placebo
s <- sqrt(((n1 - 1) * s1^2 + (n2 - 1) * s2^2)/(n1 + n2 - 2))
ts <- (x1 - x2)/(s * sqrt(1/n1 + 1/n2))
2 * pt(ts, n1 + n2 - 2)
```
```{r}
# [1] 0.003504
```


Question 6
----------
Brain volumes for 9 men yielded a 90% confidence interval of 1,077 cc to 1,123 cc. Would you reject in a two sided 5% hypothesis test of

H0:μ=1,078?

* Yes you would reject.

* It's impossible to tell.

* No you wouldn't reject.

* Where does Brian come up with these questions?

Answer: </br>
No you wouldn't reject.

Explanation: 
No, you would fail to reject. The 95% interval would be wider than the 90% interval. Since 1,078 is in the narrower 90% interval, it would also be in the wider 95% interval. Thus, in either case it's in the interval and so you would fail to reject.

Question 7
----------
Researchers would like to conduct a study of 100 healthy adults to detect a four year mean brain volume loss of .01 mm<sup>3</sup>. Assume that the standard deviation of four year volume loss in this population is .04 mm<sup>3</sup>. About what would be the power of the study for a 5% one sided test versus a null hypothesis of no volume loss?

* 0.50

* 0.70

* 0.80

* 0.60

Answer: </br>

```{r}
n <- 100 #subject
μ <- 0.01# m^3 brain volume loss mean
σ <- 0.04# m^3 brain volume loss std. dev.
p <- 0.05 # sign level

pow <- power.t.test(n=n, delta=μ, sd=σ , sig.level=p, type="one.sample", alt="one.sided")$power
round(pow, 2)
```

```{r}
# [1] 0.8
```

Explanation: 

Question 8
----------
Researchers would like to conduct a study of n healthy adults to detect a four year mean brain volume loss of .01 mm<sup>3</sup>. Assume that the standard deviation of four year volume loss in this population is .04 mm<sup>3</sup>. About what would be the value of n needed for 90% power of type one error rate of 5% one sided test versus a null hypothesis of no volume loss?

* 180

* 160

* 140

* 120

Answer: </br>

```{r}
μ <- 0.01# m^3 brain volume loss mean
σ <- 0.04# m^3 brain volume loss std. dev.
p <- 0.05 # sign level
pow <- 0.9 #power

n <- power.t.test(power=pow, delta=μ, sd=σ , sig.level=p, type="one.sample", alt="one.sided")$n
ceiling(n/10)*10
```
or 
The hypothesis is H0:μΔ=0 H_0 : \mu_\Delta = 0 H0​:μΔ​=0 versus Ha:μΔ>0 H_a : \mu_\Delta > 0 Ha​:μΔ​>0 where μΔ \mu_\Delta μΔ​ is volume loss (change defined as Baseline - Four Weeks). The test statistics is XˉΔ.04/n \frac{\bar X_\Delta}{.04 / \sqrt{n} } .04/n

​XˉΔ​​ which is rejected if it is larger than Z.95=1.645 Z_{.95} = 1.645 Z.95​=1.645.

We want to calculate

P(XˉΔσΔ/n>1.645 ∣ μΔ=.01)=P(XˉΔ−.01.04/n>1.645−.01.04/n ∣ μΔ=.01)=P(Z>1.645−n/4)=.90 P\left(\frac{\bar X_\Delta}{\sigma_\Delta / \sqrt{n}} > 1.645 ~|~ \mu_\Delta = .01\right) = P\left( \frac{\bar X_\Delta - .01}{.04 / \sqrt{n}} > 1. 645 - \frac{.01}{.04 / \sqrt{n}} ~|~ \mu_\Delta = .01\right) = P(Z > 1.645 - \sqrt{n} / 4) = .90 P(σΔ​/n
​XˉΔ​​>1.645 ∣ μΔ​=.01)=P(.04/n
​XˉΔ​−.01​>1.645−.04/n
​.01​ ∣ μΔ​=.01)=P(Z>1.645−n

​/4)=.90

So we need 1.645−n/4=Z.10=−1.282 1.645 - \sqrt{n} / 4 = Z_{.10} = -1.282 1.645−n

​/4=Z.10​=−1.282 and thus

n=(4∗(1.645+1.282))2 n = ( 4 * (1.645 + 1.282)  )^ 2 n=(4∗(1.645+1.282))2.
```{r}
ceiling((4 * (qnorm(0.95) - qnorm(0.1)))^2)
```
```{r}
# [1] 140
```

Explanation: 

Question 9
----------
As you increase the type one error rate, α, what happens to power?

* You will get larger power.

* No, for real, where does Brian come up with these problems?

* You will get smaller power.

* It's impossible to tell given the information in the problem.

Answer: </br>
You will get larger power.

Explanation: 
As you require less evidence to reject, i.e. your α \alpha α rate goes up, you will have larger power.
