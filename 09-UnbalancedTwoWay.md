# Unbalanced data in Two-Way ANOVA

What happens if $n_{ij}$ is not the same over crossed levels of the treatment factors? And, worse, what if some combination has zero replicates?!?<br><br>

This is very common for two reasons: in observational studies researchers have less control over the number of subjects with certain levels of factors.  Even when these are controlled through sampling or fixed in experimental studies there will be individuals that are lost/missing for some reason, and this ruins balance.  <br><br>

What goes wrong and what still works? (some of these things we haven't yet defined)<br>
 - Comparisons using contrasts - still work!
 - Partial F tests in linear model - still work!
 - least-squares means - still work!
 - Sums of squares (you guessed it) - fails miserably!
<br><br>

Throughout we'll assume missing observations/lack of balance occurs at random, that is, does not have anything to do with treatments.  If, however, a treatment level causes loss of observations, our analysis will have problems.  You can imagine a treatment that causes side-effects in humans may cause "loss to follow-up" when taking before-after measures, because people will dislike the treatment and drop out of the study.  So, independence of unbalance and treatment is not always a given.

## Raw and Least-Squares Means

Consider a two-way ANOVA where factors A and B each have 2 levels, simplest possible setup.  Given observations:

|   |  |Cell Means (Sample Sizes) |         |
|---|--|--------------------------|---------|
|   |  |B                         |         |
|   |  |1                         | 2       |
| A | 1|4.90(10)                  | 2.79(4) |
|   | 2|8.14(2)                   | 6.00(4) |

When the sample sizes were all equal (balanced) sample means were apples-to-apples.  Now, not so much.  We can define the "raw sample means" (sample-size weighted averages) by $\overline Y_{1\cdot\cdot} = 4.30= \frac{1}{14}(10\times 4.9 + 4\times 2.79)$.  The raw means are misleading if the other factor has an effect, because B at level 1 is more represented in the raw mean average than B at level 2.  The difference of raw means $\overline Y_{1\cdot\cdot} - \overline Y_{2\cdot\cdot}$ also contains an effect from B, which is hidden/disguised when we sum over levels of B.  The raw means for factor A are defined $\mu_{i\cdot}^R = [\sum_{j}n_{ij}]^{-1}\sum_j n_{ij}\mu_{ij}$.<br><br>

The "least-squares means" on the other hand, average the cell means without regard for sample size: mean of level 1 of A is $(2.79+4.9)/2 = 3.85$.  The LS means for factor A are defined  $\mu_{i\cdot} = \sum_{j} \mu_{ij}/b$ where $b$ is the number of levels of B. <br><br>

When the experiment is balanced, these are equal.<br><br>

The estimators of the raw means for factor A are simply the raw sample means $\hat\mu_{i\cdot}^{R} = [\sum_{j}n_{ij}]^{-1}\sum_j n_{ij}\overline Y_{ij}$.  These have standard error
\[\text{s.e.}(\hat\mu_{i\cdot}^{R})=\sigma\left[\sum_{j}n_{ij}\right]^{-1/2}.\]
The estimated LS means have standard error
\[\text{s.e.}(\hat\mu_{i\cdot})=\sigma\sqrt{b^{-2}\sum_j n_{ij}^{-1}}.\]
<br><br>

"Simple effects" compare two levels of one factor at a fixed level of another factor.  Differences of LS means are unweighted averages of simple effects:
\[\sum_j(\mu_{ij} - \mu_{i'j})/b\]

### Example: Turbine data

The following data describes characteristics of turbine jet engines.  Turbine engines feature many spinning blades.  Both the position and rate of revolutions of the moving blades affect the behavior of the outflow of air generated by the engines.  The Strouhal number is one quantity that characterizes the oscillations of this flow of air.  In the following data the Strouhal number is the response variable, and both rate and position of turbine blades are treatment factors.   


```{=html}
<a href="data:text/csv;base64,c3Ryb3VoYWwscG9zaXRpb24scmF0ZQ0KMjYwMTYuNjUsLTEsMA0KMjgyNTQuODQsLTEsMA0KMjc0MTMuNDUsLTEsMA0KMjg1NTYuNCwtMSwwDQoyNjAyOS41MiwxLDANCjI4MjcwLjIsMSwwDQoyNzM3MC45OCwxLDANCjI4NTQyLjYyLDEsMA0KMzAzNTkuNywtMSwxDQozMzE5MS43NCwtMSwxDQozMTQ4Ny4xOSwtMSwxDQozMzExMS4wNywtMSwxDQozMDA2Ni4yNSwxLDENCjMyODY4Ljk1LDEsMQ0KMzE3NTcuNTYsMSwxDQozMzQzNy45NSwxLDENCjMyMTg5LjQzLC0xLDINCjMzNzgyLjMxLC0xLDINCjM0MDkwLjExLC0xLDINCjMxMDk4LjI3LC0xLDINCjMwOTk0LjU2LDEsMg0KMzEwMTMuMTksMSwyDQozNDg5Mi4xMywxLDINCjMzMTE2Ljk4LDEsMg0K" download="flow.csv">Download flow.csv</a>
```

The turbine data is balanced, but we'll remove a few rows to create imbalance, just for illustration.


```r
strouhal.df <- read.csv('flow.csv')
strouhal.df
```

```
##    strouhal position rate
## 1  26016.65       -1    0
## 2  28254.84       -1    0
## 3  27413.45       -1    0
## 4  28556.40       -1    0
## 5  26029.52        1    0
## 6  28270.20        1    0
## 7  27370.98        1    0
## 8  28542.62        1    0
## 9  30359.70       -1    1
## 10 33191.74       -1    1
## 11 31487.19       -1    1
## 12 33111.07       -1    1
## 13 30066.25        1    1
## 14 32868.95        1    1
## 15 31757.56        1    1
## 16 33437.95        1    1
## 17 32189.43       -1    2
## 18 33782.31       -1    2
## 19 34090.11       -1    2
## 20 31098.27       -1    2
## 21 30994.56        1    2
## 22 31013.19        1    2
## 23 34892.13        1    2
## 24 33116.98        1    2
```

```r
strouhal.df<-strouhal.df[-c(1,8,24),]
strouhal.df
```

```
##    strouhal position rate
## 2  28254.84       -1    0
## 3  27413.45       -1    0
## 4  28556.40       -1    0
## 5  26029.52        1    0
## 6  28270.20        1    0
## 7  27370.98        1    0
## 9  30359.70       -1    1
## 10 33191.74       -1    1
## 11 31487.19       -1    1
## 12 33111.07       -1    1
## 13 30066.25        1    1
## 14 32868.95        1    1
## 15 31757.56        1    1
## 16 33437.95        1    1
## 17 32189.43       -1    2
## 18 33782.31       -1    2
## 19 34090.11       -1    2
## 20 31098.27       -1    2
## 21 30994.56        1    2
## 22 31013.19        1    2
## 23 34892.13        1    2
```

```r
strouhal.df$position<-as.factor(strouhal.df$position)
strouhal.df$rate<-as.factor(strouhal.df$rate)
```

Below are the crossed treatment mean responses, the LS means, and the raw means:


```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.2 --
## v ggplot2 3.3.6      v purrr   0.3.4 
## v tibble  3.1.8      v dplyr   1.0.10
## v tidyr   1.2.1      v stringr 1.4.1 
## v readr   2.1.2      v forcats 0.5.2 
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
by_trt <- group_by(strouhal.df, position, rate)
mean_resp <- summarise(by_trt, trt_mean = mean(strouhal))
```

```
## `summarise()` has grouped output by 'position'. You can override using the
## `.groups` argument.
```

```r
mean_resp
```

```
## # A tibble: 6 x 3
## # Groups:   position [2]
##   position rate  trt_mean
##   <fct>    <fct>    <dbl>
## 1 -1       0       28075.
## 2 -1       1       32037.
## 3 -1       2       32790.
## 4 1        0       27224.
## 5 1        1       32033.
## 6 1        2       32300.
```

```r
mean_resp<-as.matrix(mean_resp[,3])
LS_mean_pos <- c(mean(mean_resp[1:3,1]), mean(mean_resp[4:6,1]))
LS_mean_pos
```

```
## [1] 30967.45 30518.73
```

```r
LS_mean_rate <- c(mean(mean_resp[c(1,4),1]), mean(mean_resp[c(2,5),1]), mean(mean_resp[c(3,6),1]))
LS_mean_rate
```

```
## [1] 27649.23 32035.05 32544.99
```

```r
by_pos <- group_by(strouhal.df, position)
raw_mean_pos <- summarise(by_pos, trt_mean = mean(strouhal))
raw_mean_pos
```

```
## # A tibble: 2 x 2
##   position trt_mean
##   <fct>       <dbl>
## 1 -1         31230.
## 2 1          30670.
```

```r
by_rate <- group_by(strouhal.df, rate)
raw_mean_rate <- summarise(by_rate, trt_mean = mean(strouhal))
raw_mean_rate
```

```
## # A tibble: 3 x 2
##   rate  trt_mean
##   <fct>    <dbl>
## 1 0       27649.
## 2 1       32035.
## 3 2       32580
```

It is also convenient to compute the LS means using built-in functions from the package emmeans:.  The "emmean" stands for estimated marginal mean.  These are calculated based on a model.  If we use the two way model with interaction, then the emmeans are the LS means.


```r
library(emmeans)
emmeans(lm(strouhal~position+rate+position*rate, data = strouhal.df), 'position')
```

```
## NOTE: Results may be misleading due to involvement in interactions
```

```
##  position emmean  SE df lower.CL upper.CL
##  -1        30967 441 15    30028    31906
##  1         30519 462 15    29534    31504
## 
## Results are averaged over the levels of: rate 
## Confidence level used: 0.95
```

```r
emmeans(lm(strouhal~position+rate+position*rate, data = strouhal.df), 'rate')
```

```
## NOTE: Results may be misleading due to involvement in interactions
```

```
##  rate emmean  SE df lower.CL upper.CL
##  0     27649 591 15    26389    28909
##  1     32035 512 15    30944    33126
##  2     32545 553 15    31367    33723
## 
## Results are averaged over the levels of: position 
## Confidence level used: 0.95
```

Next, lets compute CIs for contrasts of LS means.  For example, consider comparing mean Strouhal values at rate level 0 versus rate level 1.  The point estimate is
\[\overline Y_{\cdot,0} - \overline Y_{\cdot,1}\]
the sample LS mean at level zero of rate summed over position minus the sample LS mean at level one of rate summed over position.  This difference of sample means has the following standard error:
\begin{align*}
V(\overline Y_{\cdot,0} - \overline Y_{\cdot,1}) & = V(\tfrac12(\overline Y_{-1,0} + \overline Y_{1,0}) -\tfrac12(\overline Y_{-1,1} + \overline Y_{1,1}))//
& = \sigma^2\tfrac14 (1/3 + 1/3 + 1/4 + 1/4)
\end{align*}
using the cell sample sizes calculated in R below.


```r
by_trt <- group_by(strouhal.df, position, rate)
n_resp <- summarise(by_trt, trt_mean = n())
```

```
## `summarise()` has grouped output by 'position'. You can override using the
## `.groups` argument.
```

```r
n_resp
```

```
## # A tibble: 6 x 3
## # Groups:   position [2]
##   position rate  trt_mean
##   <fct>    <fct>    <int>
## 1 -1       0            3
## 2 -1       1            4
## 3 -1       2            4
## 4 1        0            3
## 5 1        1            4
## 6 1        2            3
```

Then, a CI for the difference in LS means is given by
\[\left(\overline Y_{\cdot,0} - \overline Y_{\cdot,1} \pm t_{1-\alpha/2, n-p}\sqrt{MSE\tfrac14 (1/3 + 1/3 + 1/4 + 1/4) }\right)\]


```r
my.lm <- summary(lm(strouhal~position+rate+position*rate, data = strouhal.df))

c(LS_mean_rate[1] - LS_mean_rate[2] - qt(0.975, my.lm$df[2])*my.lm$sigma*sqrt((1/4)* (1/3 + 1/3 + 1/4 + 1/4)), LS_mean_rate[1] - LS_mean_rate[2] + qt(0.975, my.lm$df[2])*my.lm$sigma*sqrt((1/4)* (1/3 + 1/3 + 1/4 + 1/4)))
```

```
## [1] -6052.374 -2719.265
```

## Partial F tests and Type III SS

To test for interaction and main effects in unbalanced, to-way ANOVA, we cannot use the same sums of squares formulas as before.  These do not work for unbalanced data.  Essentially, they result in comparisons of raw, rather than LS means.  <br><br>

Instead, we test for interaction and main effects using *partial F tests*.  Think of the Gauss-Markov notation $Y=X\beta+\epsilon$.  A partial F test is used to test the hypothesis $H_0:\text{a certain set of }\beta_j\text{ coefficients is zero}$.  The set could be as small as one coefficient, or as large as all the coefficients except for the intercept.  Under the null hypothesis, inclusion in the model of the covariates corresponding to these coefficients does not substantially change the predicted responses $\hat Y$; and, as a result, does not substantially reduce the sum of squared residuals.  Therefore, the intuitive test of this null hypothesis is a comparison of SSE under models including versus excluding the given set of covariates:

\[F = \frac{[SSE(\text{reduced model}) - SSE(\text{full model})]/\text{number of excluded coefficients}}{MSE(\text{full model})}\]

Under $H_0$ the test statistic $F$ has an $F$ distribution with numerator degrees of freedom equal to the number of excluded coefficients and denominator degrees of freedom equal to $n-p$ where the full model coefficient vector $\beta$ has dimension $p\times 1$.
<br><br>
SAS, and certain R functions, will report *Type III sums of squares* for unbalanced experiments.  These are simply defined by partial F tests.  For example, the Type III SS for factor A in a two-way ANOVA with interaction is simply the difference in the sum of squared residuals for the full linear model corresponding to the two-way ANOVA versus the reduced linear model where the columns of $X$ corresponding to the factor A main effects have been removed.  Therefore, tests based on Type III sums of squares are exactly partial F tests.

### Example: Turbine Data tests

We can perform partial F tests by fitting reduced and full linear models and comparing sums of squared residuals.  The R package *car* will perform these tests automatically using the function *Anova*.  The following code computes a tests for the main effect of rate, both "by hand" by fitting full and reduced models, and using the Anova function.  Note the equivalence of F statistics.


```r
library(car)
```

```
## Loading required package: carData
```

```
## 
## Attaching package: 'car'
```

```
## The following object is masked from 'package:dplyr':
## 
##     recode
```

```
## The following object is masked from 'package:purrr':
## 
##     some
```

```r
Anova(lm(strouhal~position + rate+position*rate, data = strouhal.df), type = 'III')
```

```
## Anova Table (Type III tests)
## 
## Response: strouhal
##                   Sum Sq Df   F value    Pr(>F)    
## (Intercept)   2364599469  1 1128.1201 1.564e-15 ***
## position         1087144  1    0.5187  0.482485    
## rate            42206544  2   10.0681  0.001689 ** 
## position:rate     631584  2    0.1507  0.861425    
## Residuals       31440793 15                        
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r
summary(lm(strouhal~position + rate+position*rate, data = strouhal.df))
```

```
## 
## Call:
## lm(formula = strouhal ~ position + rate + position * rate, data = strouhal.df)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -1966.4 -1194.0   147.4  1046.6  2592.2 
## 
## Coefficients:
##                 Estimate Std. Error t value Pr(>|t|)    
## (Intercept)      28074.9      835.9  33.587 1.56e-15 ***
## position1         -851.3     1182.1  -0.720 0.482485    
## rate1             3962.5     1105.8   3.584 0.002716 ** 
## rate2             4715.1     1105.8   4.264 0.000679 ***
## position1:rate1    846.6     1563.8   0.541 0.596202    
## position1:rate2    361.3     1618.7   0.223 0.826403    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1448 on 15 degrees of freedom
## Multiple R-squared:  0.7511,	Adjusted R-squared:  0.6681 
## F-statistic: 9.053 on 5 and 15 DF,  p-value: 0.000396
```

```r
full.model <- lm(strouhal~position + rate+position*rate, data = strouhal.df)
SSE.full <- sum(full.model$residuals^2)
MSE <- SSE.full/(nrow(strouhal.df)-6)

X.R <- model.matrix(strouhal~position + rate+position*rate, data = strouhal.df)
X.R <- X.R[,-c(3,4)]

beta.hat.R <- solve(t(X.R)%*%X.R)%*%t(X.R)%*%strouhal.df$strouhal
pred.R <- X.R%*%beta.hat.R
SSE.R <- sum((strouhal.df$strouhal - pred.R)^2)

F <- ((SSE.R - SSE.full)/(2)) / (SSE.full / (nrow(X.R)-6))
F
```

```
## [1] 10.0681
```


## Follow-up tests

From the Anova output in the above example we see that the main effect for rate is significant.  Since rate has 3 factors, it makes sense to perform Tukey-corrected pairwise comparisons to better understand which levels of rate drive the significance of the effect.  <br><br>

For balanced experiments we typically use the R function TukeyHSD, which takes an aov object as its argument.  But, since aov objects are based on Type I SS (raw means) these pairwise comparisons are not the comparisons we want to make.  Rather, we want to make pairwise comparisons of LS means.  <br><br>

We can compute these by hand , as we did above for uncorrected pairwise comparisons.  The only difference is that we replace the Student's $t$ quantile in the CI calculation by the upper $1-\alpha$ quantile of Tukey's Studentized range distribution, multiplied by $1/\sqrt{2}$.  

### Example: Turbine data Tukey-corrected pairwise comparisons



```r
n_resp
```

```
## # A tibble: 6 x 3
## # Groups:   position [2]
##   position rate  trt_mean
##   <fct>    <fct>    <int>
## 1 -1       0            3
## 2 -1       1            4
## 3 -1       2            4
## 4 1        0            3
## 5 1        1            4
## 6 1        2            3
```

```r
c(LS_mean_rate[1] - LS_mean_rate[2] - sqrt(1/2)*qtukey(0.95,3, my.lm$df[2])*my.lm$sigma*sqrt((1/4)* (1/3 + 1/3 + 1/4 + 1/4)), LS_mean_rate[1] - LS_mean_rate[2] + sqrt(1/2)*qtukey(0.95,3, my.lm$df[2])*my.lm$sigma*sqrt((1/4)* (1/3 + 1/3 + 1/4 + 1/4)))
```

```
## [1] -6416.750 -2354.889
```

```r
1-ptukey(abs(LS_mean_rate[1] - LS_mean_rate[2])/(sqrt(1/2)*my.lm$sigma*sqrt((1/4)* (1/3 + 1/3 + 1/4 + 1/4))), 3, my.lm$df[2])
```

```
## [1] 0.0001390661
```

```r
c(LS_mean_rate[1] - LS_mean_rate[3] - sqrt(1/2)*qtukey(0.95,3, my.lm$df[2])*my.lm$sigma*sqrt((1/4)* (1/3 + 1/3 + 1/4 + 1/3)), LS_mean_rate[1] - LS_mean_rate[2] + sqrt(1/2)*qtukey(0.95,3, my.lm$df[2])*my.lm$sigma*sqrt((1/4)* (1/3 + 1/3 + 1/4 + 1/3)))
```

```
## [1] -6997.976 -2283.607
```

```r
1-ptukey(abs(LS_mean_rate[1] - LS_mean_rate[3])/(sqrt(1/2)*my.lm$sigma*sqrt((1/4)* (1/3 + 1/3 + 1/4 + 1/3))), 3, my.lm$df[2])
```

```
## [1] 6.250909e-05
```

```r
c(LS_mean_rate[2] - LS_mean_rate[3] - sqrt(1/2)*qtukey(0.95,3, my.lm$df[2])*my.lm$sigma*sqrt((1/4)* (1/4 + 1/4 + 1/4 + 1/3)), LS_mean_rate[1] - LS_mean_rate[2] + sqrt(1/2)*qtukey(0.95,3, my.lm$df[2])*my.lm$sigma*sqrt((1/4)* (1/4 + 1/4 + 1/4 + 1/3)))
```

```
## [1] -2466.998 -2428.766
```

```r
1-ptukey(abs(LS_mean_rate[2] - LS_mean_rate[3])/(sqrt(1/2)*my.lm$sigma*sqrt((1/4)* (1/4 + 1/4 + 1/4 + 1/3))), 3, my.lm$df[2])
```

```
## [1] 0.7802542
```

```r
library(emmeans)
em.strouhal<-emmeans(lm(strouhal~position + rate+position*rate, data = strouhal.df), "rate")
```

```
## NOTE: Results may be misleading due to involvement in interactions
```

```r
pairs(em.strouhal, adjust = 'tukey')
```

```
##  contrast      estimate  SE df t.ratio p.value
##  rate0 - rate1    -4386 782 15  -5.609  0.0001
##  rate0 - rate2    -4896 809 15  -6.049  0.0001
##  rate1 - rate2     -510 753 15  -0.677  0.7803
## 
## Results are averaged over the levels of: position 
## P value adjustment: tukey method for comparing a family of 3 estimates
```


## Unbalanced data and Simpson's Paradox

The following toy example of unbalanced data helps illustrate the difference between using raw and LS means.  The LS mean response for A level 1 is larger (-0.3) than the LS mean response for A level 2 (-0.7).  And, the mean response when A is level 1 is larger than when A is level 2 for every level of B.  However, for raw means, the reverse is true (-1.975 vs. 0.225)! If we look at the data we see this is due to the fact more samples are observed for the levels of B when A is level 1 and the response is lowest (10) compared to when it is highest (5).  Basically, the sampling is biased towards levels of B where the A level 1 response is low and the A level 2 response is high.  The raw means are not capturing a real difference in effect of A; rather, they are capturing a difference in sample weighting.  <br><br>

This is an example of **Simpson's Paradox** where the direction of an effect may change if effects are weighted by sample size.  


```r
Y1 <- (1:5)+0.1
Y2 <- (1:5)
Y3 <- c((1:5)-10,(1:5)-10) 
Y4 <- c((1:5),1:5)
Y5 <- (1:5)-0.1
Y6 <- (1:5)-11

A <- c(rep(1,20),rep(2,20))
B <- c(rep(1,5),rep(2,5), rep(3,10),rep(1,10),rep(2,5), rep(3,5))
Y <- c(Y1,Y2,Y3,Y4,Y5,Y6)
data.df <- data.frame(Y=Y,A=as.factor(A),B=as.factor(B))

by_trt <- group_by(data.df, A,B)
summarise(by_trt, trt_mean = mean(Y))
```

```
## `summarise()` has grouped output by 'A'. You can override using the `.groups`
## argument.
```

```
## # A tibble: 6 x 3
## # Groups:   A [2]
##   A     B     trt_mean
##   <fct> <fct>    <dbl>
## 1 1     1          3.1
## 2 1     2          3  
## 3 1     3         -7  
## 4 2     1          3  
## 5 2     2          2.9
## 6 2     3         -8
```

```r
emmeans(lm(Y~A+B+A*B, data = data.df), "A")
```

```
## NOTE: Results may be misleading due to involvement in interactions
```

```
##  A emmean    SE df lower.CL upper.CL
##  1   -0.3 0.362 34    -1.03   0.4348
##  2   -0.7 0.362 34    -1.43   0.0348
## 
## Results are averaged over the levels of: B 
## Confidence level used: 0.95
```

```r
emmeans(lm(Y~A+B+A*B, data = data.df), "B")
```

```
## NOTE: Results may be misleading due to involvement in interactions
```

```
##  B emmean    SE df lower.CL upper.CL
##  1   3.05 0.420 34     2.20     3.90
##  2   2.95 0.485 34     1.96     3.94
##  3  -7.50 0.420 34    -8.35    -6.65
## 
## Results are averaged over the levels of: A 
## Confidence level used: 0.95
```

```r
mean(data.df$Y[data.df$A==1])
```

```
## [1] -1.975
```

```r
mean(data.df$Y[data.df$A==2])
```

```
## [1] 0.225
```

```r
mean(data.df$Y[data.df$B==1])
```

```
## [1] 3.033333
```

```r
mean(data.df$Y[data.df$B==2])
```

```
## [1] 2.95
```

```r
mean(data.df$Y[data.df$B==3])
```

```
## [1] -7.333333
```








