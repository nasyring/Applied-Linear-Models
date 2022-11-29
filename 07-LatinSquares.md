# Latin Squares for two blocking variables

## Designs for multiple blocks

Suppose we have two blocking variables and one treatment variable, and suppose all three of these categorical variables have the same number of levels.  For example, suppose an experiment to measure the miles per gallon (MPG) of automobiles using 4 types of gasoline fuel additives (the treatments) is blocked by 4 models of car and 4 different drivers driving the cars on the same road course.  In order to carry out an RCBD we would consider the 16 crossed blocks given by driver times car model.  And, we require each treatment to be replicated at least once per block, which means we need a total of at least 64 samples.  <br><br>

The Latin square design is a special design for the case of 2 (or more) blocking variables and one treatment variable all having the same number of levels.  The Latin square (or cube, or hypercube!) only requires one treatment be used for each combination of blocks, but according to a strict pattern: If we write down a grid or matrix corresponding to crossing the levels of the two blocking variables, then a Latin square design corresponds to every treatment level appearing in each row and column exactly once (think of a sudoku puzzle).

### Example: MPG experiment

Four for drivers (1,2,3,4), four cars (1,2,3,4), and four treatments/additives (A,B,C,D) we have the following Latin square design:

|     	|   	| Driver 	|   	|   	|   	|
|-----	|---	|--------	|---	|---	|---	|
|     	|   	| 1      	| 2 	| 3 	| 4 	|
| Car 	| 1 	| A      	| B 	| C 	| D 	|
|     	| 2 	| B      	| C 	| D 	| A 	|
|     	| 3 	| C      	| D 	| A 	| B 	|
|     	| 4 	| D      	| A 	| B 	| C 	|


## Model notation

A Latin square design may be represented by the follwowing model:
\[Y_{ijk} = \mu + \beta_i + \gamma_j + \tau_k + \epsilon_{ijk}\]
where $i, j, k = 1, \ldots, r$, $\beta$ and $\gamma$ refer to row and column block effects and $\tau$ is the treatment effect.  As always, the model is over-parametrized, and constraints are necessary; for example, the baseline constraint $\beta_r = \gamma_r = \tau_r = 0$.



## Sums of squares and test for treatment effects

The row (SSBr) and column (SSBc) block sums of squares, and treatment (SSTr) sums of squares are given by:
\[SSBr = r\sum_{i}(\overline Y_{i\cdot\cdot} - \overline Y_{\cdot\cdot\cdot})^2, \quad SSBc = r\sum_{j}(\overline Y_{\cdot j\cdot} - \overline Y_{\cdot\cdot\cdot})^2, \quad SSBr = r\sum_{k}(\overline Y_{\cdot\cdot k} - \overline Y_{\cdot\cdot\cdot})^2.\]
Each have $r-1$ degrees of freedom (r summands minus 1 estimated parameter, which is $\overline Y_{\cdot\cdot\cdot}$ estimating the overall mean parameter).  The total sum of squares is
\[SST = \sum_i\sum_j\sum_k(Y_{ijk} - \overline{Y}_{\cdot\cdot\cdot})^2\]
which has $r^2 - 1$ degrees of freedom ($r^2$ summands minus one estimated parameter, which is $\overline Y_{\cdot\cdot\cdot}$ estimating the overall mean parameter).  The SSE is most easily obtained by subtraction:
\[SSE = SST - SSBr - SSBc - SSTr\]
and has degrees of freedom (obtained by subtraction) $(r-1)(r-2)$.  <br><br>

The most important test is the test for significant treatments: $H_0: \tau_k = 0, \text{for all } k$ versus $H_a: \text{ at least one treatment has an effect}$ on the mean response.

### Example:  MPG experiment

The following MPG measurements were obtained using a Latin square design.


|     	|   	| Driver 	|        	|        	|        	|
|-----	|---	|--------	|--------	|--------	|--------	|
|     	|   	| 1      	| 2      	| 3      	| 4      	|
| Car 	| 1 	| A = 24 	| B = 26 	| C = 16 	| D = 20 	|
|     	| 2 	| B = 15 	| C = 26 	| D = 20 	| A = 16 	|
|     	| 3 	| C = 17 	| D = 13 	| A = 20 	| B = 27 	|
|     	| 4 	| D = 23 	| A = 15 	| B = 20 	| C = 25 	|



```r
mpg <- data.frame(car = c(1,2,3,4,1,2,3,4,1,2,3,4,1,2,3,4), driver = c(1,1,1,1,2,2,2,2,3,3,3,3,4,4,4,4), additive = c('A', 'B', 'C','D','B','C','D','A','C','D','A','B','D','A','B','C'), mpg = c(24,15,17,23,26,26,13,15,16,20,20,20,20,16,27,25) )
mpg$car <- factor(mpg$car, levels = unique(mpg$car))
mpg$driver <- factor(mpg$driver, levels = unique(mpg$driver))
mpg$additive <- factor(mpg$additive, levels = unique(mpg$additive))

my.aov <- aov(mpg~additive + car+driver, data = mpg)
summary(my.aov)
```

```
##             Df Sum Sq Mean Sq F value Pr(>F)
## additive     3  29.69    9.90   0.241  0.865
## car          3  15.19    5.06   0.124  0.943
## driver       3  19.69    6.56   0.160  0.919
## Residuals    6 245.87   40.98
```


## Relative efficiency of blocking

The efficiency of a Latin square design may be assessed relative to a CRD or an RCBD using only the row or only the column blocking variable.  For comparison to and RCBD using the row block, we have the formula:

\[RE = \frac{MSE_{row}}{MSE_{LS}} \times \text{df correction}\]
where $MSE_{row} = \frac{MS_{row} + (r-1)MSE_{LS}}{r}$, where $MSE_{LS}$ denotes the observed MSE from the Latin square experiment, $MS_{row}$ denotes the oserved mean sum of squares of the row blocking variable from the Latin square experiment, and where
\[\text{df correction} = \frac{df_{E, LS}+1}{df_{E,RCBD} + 1}\times\frac{df_{E,RCBD}+3}{df_{E,LS}+3} = \frac{r^2-3r+3}{r^2-2r+2}\times\frac{r^2-2r+4}{r^2-3r+5}.\]


### Example: RE ignoring driver blocks

Neither blocking variable was particularly helpful.  The RE relative to RCBD without driver is only about $74\%$.


```r
MSE_row <- (6.56 + 3*40.98)/4
correction <- function(r)  (r^2 - 3*r+3)*(r^2 - 2*r+4)/((r^2-3*r+5)*(r^2 - 2*r+2))
RE <- (MSE_row / 40.98)*correction(4)
RE
```

```
## [1] 0.7373516
```





