# Gauss-Markov Theory

Recall the Gauss-Markov model defines a linear, stochastic relationship between an $n\times 1$ vector response $Y$ and an $n\times p$ matrix of covariates $X$ by
\[Y = X\beta + \epsilon\]
where $\epsilon_i \stackrel{iid}{\sim} N(0,\sigma^2)$.<br><br>

Gauss-Markov theory is concerned with the following questions:<br>
 - Which linear functions $c^\top \beta$ are meaningful and may be estimated by the observed $(Y,X)$?
 - How should such functions be estimated?
 - Are hypotheses about such functions testable using the observed(Y,X)?
 - How may such testable hypotheses be tested? (What are the test statistics and their corresponding sampling distributions?)
 
## Estimability

In general, beyond Gauss-Markov models, whenever we use a probability distribution to model a population there is a notion of *identifiability* with respect to the parameter of that model distribution.  Say $F_\theta$ is a distribution function used to model a population where $\theta$ is the parameter indexing the family of distributions.  Then, we say $\theta$ is identifiable if and only if $\theta\mapsto F_\theta$ one-to-one.  In other words, two different parameters $\theta$ and $\theta'$ do not correspond to the same distribution function.  <br><br>

In the context of Gauss-Markov models the notion of identifiability boils down to:
\[\beta\text{ is identifiable if }X\beta_1\ne X\beta_2\iff \beta_1\ne \beta_2.\]
And, from the point of view of linear algebra this means that $\beta$ is estimable if and only if $X$ has full column rank---$Xa = 0 \iff a \equiv 0$.<br><br>

A linear function $c^\top \beta$ is *estimable* if it can be written
\[c^\top \beta = AX\beta = AE(Y)\]
for some matrix $A$.  Statistically, we understand estimability to mean that the function in question can be represented as one or more linear combinations of mean responses.  From a linear algebra point of view, a linear function $c^\top \beta$ is estimable if $c^\top = AX$ for some $A$, which means $c$ is in the column space of $X^\top$ (row space of X).  

### Characterizing estimable functions in one-way ANOVA

Consider a one-way ANOVA where $n=12$ with 3 groups of 4 replicates:
\[Y_{ij} = \mu + \alpha_i + \epsilon_{ij}.\]
The design matrix of the equivalent Gauss-Markov model $Y = X\beta+\epsilon$ is
\[X = \begin{bmatrix}
1 & 1 & 0 & 0 \\
1 & 1 & 0 & 0 \\
1 & 1 & 0 & 0 \\
1 & 1 & 0 & 0 \\
1 & 0 & 1 & 0 \\
1 & 0 & 1 & 0 \\
1 & 0 & 1 & 0 \\
1 & 0 & 1 & 0 \\
1 & 0 & 0 & 1 \\
1 & 0 & 0 & 1 \\
1 & 0 & 0 & 1 \\
1 & 0 & 0 & 1 
\end{bmatrix},\]
and the coefficient vector is $\beta = (\mu,\alpha_1, \alpha_2, \alpha_3)^\top$.  <br><br>
The coefficient vector $\beta$ is **NOT** identifiable or estimable.  To see this, first define $\mu_i$ $i=1,2,3$ to be the population means of the three groups.  Then, let $\beta_1 = (0,\mu_1, \mu_2, \mu_3)^\top$ and $\beta_2 = (\mu_3, \mu_1 - \mu_3, \mu_2 = \mu_3, 0)^\top$.  Then, $\beta_1 \ne \beta_2$, generally; in fact, $\beta_1 - \beta_2 = (-\mu_3, \mu_3, \mu_3, \mu_3)$. But, it is easy to check that $X\beta_1 = X\beta_2 = 4\mu_1+4\mu_2+4\mu_3$. 

<br><br>

The parameter $\mu + \alpha_1$, however, IS estimable. To see this, first write
\[\mu+\alpha_1 = c^\top \beta = (1,1,0,0)\cdot (\mu, \alpha_1, \alpha_2, \alpha_3)^\top.\]
Let $A = (\tfrac14, \tfrac14, \tfrac14, \tfrac14, 0,0,0,0,0,0,0,0)$ and note that
\[E(Y) = X\beta = (\mu+\alpha_1, \mu+\alpha_1, \mu+\alpha_1, \mu+\alpha_1, \mu+\alpha_2,\cdots, \mu+\alpha_4).\]
Therefore, $AE(Y) = \tfrac14\sum_{j=1}^4 E(Y_{1j}) = \mu+\alpha_1$.  Since we were able to find a linear combination of mean responses equal to the parameter that means the function $c^\top \beta$ is estimable.
<br><br>

Last, let's think carefully about how to characterize estimable and non-estimable functions more generally, rather than using ad-hoc methods in consideration of individual functions like those above.  Recall that the definition of estimability says that $c$ is in the row space of $X$, i.e., it is equivalent to a linear combination of rows of $X$.  That means $X^\top y = c$ for some $y$.  If there is a solution $y$ to this inhomogeneous system then $c^\top \beta$ is estimable.  One way to check this condition is to investigate the null space of $X$, which is the set of vectors $u$ such that $Xu = 0$.  If $c$ is orthogonal to any set of basis vectors spanning the null space of $X$---denoted $N(X)$---then $c$ is in the row space of $X$, hence $c^\top \beta$ is estimable.<br><br>

Let's apply this strategy to the one-way ANOVA example above.  By inspection, we can tell that only three of the four columns in $X$ are linearly independent, for example, because the first column equals the sum of the other three.  Another way to confirm this is to produce the reduced row echelon form (RREF) of $X$ via Gaussian elimination; there are only three non-zero rows in the RREF of $X$, hence the rank of $X$ is 3. (See the R code below).  Because the dimension of the row space and null space of $X$ must add to the number of columns of $X$ (the Rank + Nullity Theorem) it follows that the null space of $X$ is one-dimensional.  In other words, the null space is spanned by a single vector.  Using the RREF we can see that a solution to $Xu = 0$ is $u = (1,-1,-1,-1)^\top$. Every function $c^\top \beta$ such that the rows of $c^\top$ are orthogonal to $u$ is estimable.  For example, $(1,1,0,0)\cdot u = 0$; hence, $\mu+\alpha_1$ is estimable. 


```r
library(matlib)
X <- cbind(rep(1,12), c(1,1,1,1,0,0,0,0,0,0,0,0), c(0,0,0,0,1,1,1,1,0,0,0,0), c(0,0,0,0,0,0,0,0,1,1,1,1))
echelon(X)
```

```
##       [,1] [,2] [,3] [,4]
##  [1,]    1    0    0    1
##  [2,]    0    1    0   -1
##  [3,]    0    0    1   -1
##  [4,]    0    0    0    0
##  [5,]    0    0    0    0
##  [6,]    0    0    0    0
##  [7,]    0    0    0    0
##  [8,]    0    0    0    0
##  [9,]    0    0    0    0
## [10,]    0    0    0    0
## [11,]    0    0    0    0
## [12,]    0    0    0    0
```

```r
Solve(X, matrix(0,12,1))
```

```
## x1       + x4  =  0 
##   x2   - 1*x4  =  0 
##     x3 - 1*x4  =  0 
##             0  =  0 
##             0  =  0 
##             0  =  0 
##             0  =  0 
##             0  =  0 
##             0  =  0 
##             0  =  0 
##             0  =  0 
##             0  =  0
```


### Characterizing estimable functions in RCBD using the null space of $X$

A linear model for an RCBD is 
\[Y_{ij} = \mu + \alpha_i + \beta_j + \epsilon_{ij}.\]
Suppose, for example, that there are 12 observations, 4 in each of 3 blocks, each with a different treatment.  Then, the design matrix may be written
\[X = \begin{bmatrix}
1 & 1 & 0 & 0 & 0 & 1 & 0\\
1 & 1 & 0 & 0 & 0 & 0 & 1\\
1 & 0 & 1 & 0 & 0 & 1 & 0\\
1 & 0 & 1 & 0 & 0 & 0 & 1\\
1 & 0 & 0 & 1 & 0 & 1 & 0\\
1 & 0 & 0 & 1 & 0 & 0 & 1\\
1 & 0 & 0 & 0 & 1 & 1 & 0\\
1 & 0 & 0 & 0 & 1 & 0 & 1
\end{bmatrix}.\]
The rank of $X$ is 5, which can be determined by solving the system $Xu = 0$ by Gaussian elimination and observing that the RREF of $X$ contains 5 non-zero rows.  The rank plus nullity theorem implies the null space is two-dimensional.  Two linearly independent solutions to the homogeneous system are
\[u = (-2,1,1,1,1,1,1)^\top \text{ and } v = (-1,1,1,1,1,0,0)^\top\]
which together form a basis for $N(X)$, the null space of $X$.  All estimable linear functions $c^\top \beta$ are such that the rows of $c^\top$ are orthogonal to both $u$ and $v$, and hence orthogonal to the null space, or, in other words, members of the row space of $X$, denoted $R(X)$.  <br><br>

For examples, $\alpha_1$ is not estimable, nor is $\mu + \alpha_1 - \alpha_2$.  But, $\mu + \alpha_1 + \tfrac12(\beta_1 + \beta_2)$ is estimable.  You can check that the corresponding $c$ vectors for these functions are $(0,1,0,0,0,0,0)$, $(1,1,-1,0,0,0,0)$ and $(1,1,0,0,0,\tfrac12,\tfrac12)$, of which only the last is orthogonal to the null space basis found above.
<br><br>

The codes below illustrate how to determine the RREF and rank of the design matrix $X$ using the package matlib in R.  


```r
library(matlib)
X <- matrix(c(1,1,1,1,1,1,1,1,
		  1,1,0,0,0,0,0,0,
		  0,0,1,1,0,0,0,0,
		  0,0,0,0,1,1,0,0,
		  0,0,0,0,0,0,1,1,
		  1,0,1,0,1,0,1,0,
		  0,1,0,1,0,1,0,1),8,7)
X
```

```
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7]
## [1,]    1    1    0    0    0    1    0
## [2,]    1    1    0    0    0    0    1
## [3,]    1    0    1    0    0    1    0
## [4,]    1    0    1    0    0    0    1
## [5,]    1    0    0    1    0    1    0
## [6,]    1    0    0    1    0    0    1
## [7,]    1    0    0    0    1    1    0
## [8,]    1    0    0    0    1    0    1
```

The basis vectors $u$ and $v$ are determined by putting x6=x7=1 and x6=x7=0 in the general solution of the homogeneous system produced by the Solve function. 


```r
echelon(X)
```

```
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7]
## [1,]    1    0    0    0    1    0    1
## [2,]    0    1    0    0   -1    0    0
## [3,]    0    0    1    0   -1    0    0
## [4,]    0    0    0    1   -1    0    0
## [5,]    0    0    0    0    0    1   -1
## [6,]    0    0    0    0    0    0    0
## [7,]    0    0    0    0    0    0    0
## [8,]    0    0    0    0    0    0    0
```

```r
Solve(X,matrix(0,8,1))
```

```
## x1         + x5     + x7  =  0 
##   x2     - 1*x5           =  0 
##     x3   - 1*x5           =  0 
##       x4 - 1*x5           =  0 
##                x6 - 1*x7  =  0 
##                        0  =  0 
##                        0  =  0 
##                        0  =  0
```

By solving $Ax = 0$ where A is the null space basis of $X$ we find that an estimable linear functions $c^\top \beta$ is such that each row of $c^\top$ satisfies $c_1 = c_6+c_7$  and $c_2+c_3+c_4+c_5 = c_6+c_7$. 


```r
null.basis <- t(matrix(c(-2,1,1,1,1,1,1,-1,1,1,1,1,0,0),7,2))
null.basis
```

```
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7]
## [1,]   -2    1    1    1    1    1    1
## [2,]   -1    1    1    1    1    0    0
```

```r
Solve(null.basis, matrix(0,2,1))
```

```
## x1                  - 1*x6 - 1*x7  =  0 
##   x2 + x3 + x4 + x5 - 1*x6 - 1*x7  =  0
```



### Characterizing estimable functions in RCBD by solving an inhomogeneous equation

Recall once more that the definition of estimability says the linear function $c^\top \beta$ is estimable if and only if $c^\top \beta = AX\beta$ which means that $c^\top  = AX$ or, rather that $X^\top A^\top = c$.  Now, $X^\top$ is a $p\times n$ matrix and $c$ is a $p \times k$ matrix so that $A^\top$ is a $n\times k$ matrix.  From a linear algebraic point of view, estimability means that there exists a solution $a_\ell$ to $X^\top a_\ell = c_\ell$ for all $\ell = 1, \ldots, k$ where $a_\ell$ and $c_\ell$ denote the columns of $A^\top$ and $c$, respectively. Inhomogeneous systems may be solved by Gauss-Jordan elimination on the augmented matrix $[X^\top ,\,c]$; and see the R codes below.<br><br>
The first inhomogeneous system below is used to evaluate whether $\mu + \alpha_1 - \alpha_2$ is estimable, corresponding to $c = (1,1,-1,0,0,0,0)$.  It is not estimable, and this is reflected in the inconsistencies "0=1" in the RREF of the augmented matrix $[X, \, c]$.  On the other hand, $\mu + \alpha_1 + \tfrac12(\beta_1 + \beta_2)$ is estimable, as evidenced by the solutions given to its corresponding inhomogeneous system below.  For example, $A = (0.5,0.5,0,0,0,0,0,0)$ is one solution (obtained by putting x8=x7=x6=x5=x4=x3=0), but there are infinite others.  


```r
Solve(t(X),matrix(c(1,1,-1,0,0,0,0),7,1))
```

```
## x1     - 1*x4   - 1*x6   - 1*x8  =   1 
##   x2     + x4     + x6     + x8  =   1 
##     x3   + x4                    =  -1 
##              x5   + x6           =   0 
##                       x7   + x8  =   0 
##                               0  =  -1 
##                               0  =  -1
```

```r
Solve(t(X),matrix(c(1,1,0,0,0,.5,.5),7,1))
```

```
## x1     - 1*x4   - 1*x6   - 1*x8  =  0.5 
##   x2     + x4     + x6     + x8  =  0.5 
##     x3   + x4                    =    0 
##              x5   + x6           =    0 
##                       x7   + x8  =    0 
##                               0  =    0 
##                               0  =    0
```




## Estimation

### Full rank estimation

So far we have discussed *whether or not* a linear function $c^\top \beta$ may be estimated but not *how* to estimate it when possible.  <br><br>

Consider a linear algebraic point of view (forget about statistics momentarily) and recall that if $\beta$ is estimable that means the $p-$dimensional identity matrix is in the row space of $X$, and, equivalently, that $X$ has full rank $p$.  Given $\hat\beta$ is an estimator of $\beta$ (we have not yet defined $\hat\beta$) then $X\hat\beta$ is the natural linear predictor of $Y$.  I say it is the natural predictor because $E(Y) = X\beta$, meaning that $E(Y)$ is in the columns space of $X$, and it is natural to predict $Y$ by an estimate of its mean.  Therefore, a natural predictor of $Y$ is a vector in the columns space of $X$.  But, which one?  Again, proceeding logically, the natural choice is the vector in the columns space of $X$ that is *closest* to the observed $Y$ with respect to some measure of distance.  If we choose to measure distance according to Euclidean (also called $L_2$ distance) then the resulting estimator $\hat\beta$ turns out to be the familiar OLS/least squares estimator (and the MLE).<br><br>

To see this, note that the Euclidean distance between $Y$ and $X\hat\beta$ is given by
\[\|Y - X\hat\beta\|_2 = \left(\sum_{i=1}^n (Y_i - x_i^\top \hat\beta)^2\right)^{1/2} = (Y - X\hat\beta)^\top (Y - X\hat\beta)\]
In order to minimize this quantity over $\hat\beta$ we differentiate w.r.t. $\hat\beta$, set the gradient equal to zero and solve, which produces the *normal equations*
\[X^\top X\hat\beta = X^\top Y.\]
Since $X$ is full rank, so is $X^\top X$, and, therefore, $X^\top X$ being a square matrix is also invertible with inverse $(X^\top X)^{-1}$.  Left multiplying the normal equations by the inverse produces the OLS estimator
\[\hat\beta = (X^\top X)^{-1}X^\top Y.\]
Further, the predictor $\hat Y$ is $X\hat\beta = X(X^\top X)^{-1}X^\top Y = P_x Y$ where $P_x$ is the X-projection matrix, which *projects* $n-$vectors to a p-dimensional linear subspace, that is, $\mathbb{R}^p$, the row space of $X$.<br><br>

Tying back to the definition of estimability, note that it is straightforward to verify any linear function $c^\top \beta$ is estimable when $\hat\beta$ itself is estimable.  First, note an estimator is given by $c^\top \hat\beta = c^\top (X^\top X)^{-1}X^\top Y$.  Next, recall that $\hat\beta$ is unbiased, $E(\hat\beta) = E((X^\top X)^{-1}X^\top Y) = (X^\top X)^{-1}X^\top X\beta = \beta$.  Therefore,
\[E(c^\top \hat\beta) = c^\top \beta = c^\top (X^\top X)^{-1}X^\top E(Y) \]
which reveals that
\[c^\top \beta = AE(Y)\]
where $A = c^\top (X^\top X)^{-1}X^\top$; hence, the definition of estimability is satisfied.

### Less than full rank estimation

When $X$ has rank $r < p$ so does $X^\top X$, and, as a result, the normal equations $X^\top X \beta = X^\top Y$ have infinite solutions.  This means $\beta$ is not estimable.  However, for any two solutions to the normal equations, say $\hat\beta$ and $\tilde \beta$, their predictions are the same:
\[X\hat\beta = X\tilde \beta.\]
In other words, $\hat Y$ is *invariant* to the choice of estimate of $\beta$ so long as it is a solution to the normal equations.  For a brief explanation, note that if $\hat\beta$ and $\tilde \beta$ both solve the normal equations, then $X^\top X \hat\beta=X^\top X \tilde\beta$.  Additionally, the null space of $X^\top X$ is the same as the null space of $X$, as can be seen by the following calculation:
\begin{align*}
&X\beta = 0 \Rightarrow X^\top X\beta = 0 \Rightarrow N(X)\subseteq N(X^\top X)\\
& X^\top X\beta = 0 \Rightarrow \beta^\top X^\top X\beta = 0 \Rightarrow (X\beta)^\top (X\beta)=0 \Rightarrow X\beta = 0 \Rightarrow N(X^\top X)\subseteq N(X).
\end{align*}
Therefore, $X^\top X (\hat\beta - \tilde \beta) = 0$ implies $X\hat\beta = X\tilde \beta$.  
<br><br>
We have seen that predictions are invariant to the choice of estimator (choice of solution to the normal equations).  This means also that functions of the predictions are invariant to the choice of estimator: including the SSE and MSE, and all partial F tests!  <br><br>

### Augmented normal equations

Invariance is a very important property.  But, given there are infinite solutions to the normal equations in the less-than-full-rank case, how do we pick a solution to use in a statistical analysis?  There is a systematic way of doing so, and this method is related to the systems of constraints we considered earlier in the course.<br><br>

Let the $n\times p$ design matrix $X$ have rank $r<p$ so that $X^\top X$ also has rank $r$.  Let $K$ specify a $(p-r)\times p$ matrix of jointly non-estimable functions, i.e. every linear combination in $K\beta$ is not estimable.  $K$ may be found by examining the null space of $X$: each row vector in $K$ is not-orthogonal to every basis vector in a basis for $N(X)$.  The point is that $K$ is making up for the ($p-r$ dimensional space of) functions that are not estimable due to the lack of full rank of the model matrix.  We then augment $X\top X$ by $K$ by row-binding, or stacking, $X^\top X$ on top of $K$ and solve the *augmented normal equations*
\[\begin{bmatrix} X^\top X \\ K\end{bmatrix} \beta = \begin{bmatrix} X^\top Y \\0\end{bmatrix}.\]
The augmented equations specify a set of $p-r$ constraints $K\beta = 0$ which force a unique solution to the inhomogeneous system.  <br><br>

Below is an example of solving the augmented normal equations in the context of one-way ANOVA.  Note that the specific choice of augmented rows corresponds to a baseline constraint.



```r
library(matlib)
# The design matrix X
X <- cbind(rep(1,12), c(1,1,1,1,0,0,0,0,0,0,0,0), c(0,0,0,0,1,1,1,1,0,0,0,0), c(0,0,0,0,0,0,0,0,1,1,1,1))
X
```

```
##       [,1] [,2] [,3] [,4]
##  [1,]    1    1    0    0
##  [2,]    1    1    0    0
##  [3,]    1    1    0    0
##  [4,]    1    1    0    0
##  [5,]    1    0    1    0
##  [6,]    1    0    1    0
##  [7,]    1    0    1    0
##  [8,]    1    0    1    0
##  [9,]    1    0    0    1
## [10,]    1    0    0    1
## [11,]    1    0    0    1
## [12,]    1    0    0    1
```

```r
# X'X
t(X)%*%X
```

```
##      [,1] [,2] [,3] [,4]
## [1,]   12    4    4    4
## [2,]    4    4    0    0
## [3,]    4    0    4    0
## [4,]    4    0    0    4
```

```r
Solve(t(X)%*%X, matrix(c(0,0,0,0),4,1))
```

```
## x1       + x4  =  0 
##   x2   - 1*x4  =  0 
##     x3 - 1*x4  =  0 
##             0  =  0
```

```r
# A basis for the null space is (-1,1,1,1)
# A vector not orthogonal to this basis vector is (0,0,0,1)
# Augmented Matrix W
W <- rbind(t(X)%*%X, c(0,0,0,1))
W
```

```
##      [,1] [,2] [,3] [,4]
## [1,]   12    4    4    4
## [2,]    4    4    0    0
## [3,]    4    0    4    0
## [4,]    4    0    0    4
## [5,]    0    0    0    1
```

```r
# Generate some response vector for illustration
Y <- matrix(rnorm(12,X%*%matrix(c(1,1,2,3),4,1),1),12,1)
Y
```

```
##           [,1]
##  [1,] 1.764003
##  [2,] 1.575480
##  [3,] 1.866202
##  [4,] 1.962823
##  [5,] 2.963767
##  [6,] 2.263546
##  [7,] 2.819047
##  [8,] 3.117973
##  [9,] 3.128628
## [10,] 1.459018
## [11,] 5.188688
## [12,] 4.655557
```

```r
# Denote [X'Y 0]' by b
b <- rbind(t(X)%*%Y,0)
b
```

```
##           [,1]
## [1,] 32.764731
## [2,]  7.168509
## [3,] 11.164332
## [4,] 14.431890
## [5,]  0.000000
```

```r
# Solve the augmented normal equations [X'X K]' beta = [X'Y 0]',
# and note that there is a unique solution and that beta_4 = 0
beta.hat <- Solve(W, b)
```

```
## x1        =   3.60797241 
##   x2      =  -1.81584524 
##     x3    =  -0.81688933 
##       x4  =            0 
##        0  =            0
```

As we have previously shown, $K$ and $K^\top K$ map to the same null space (we showed this w.r.t. the design matrix $X$).  Therefore, an equivalent set of augmented normal equations is given by

\[\begin{bmatrix} X^\top X \\ K^\top K\end{bmatrix} \beta =\begin{bmatrix} X \\ K\end{bmatrix}^\top \begin{bmatrix} X\\ K\end{bmatrix}\beta =\begin{bmatrix} X^\top Y \\0\end{bmatrix}.\]

Since $[X\,\,\, K]^\top$ is full rank, the product is full rank and $(X^\top X + K^\top K)$ has an inverse.  We can alternatively write the augmented normal equations as
\[(X^\top X + K^\top K)\beta = X^\top Y,\]
and we may estimate $\beta$ by *constrained least squares*:
\[\hat\beta = (X^\top X + K^\top K)^{-1}X^\top Y\]

Next we use this constrained least squares approach in the context of an RCBD example. 


```r
library(matlib)
# The design matrix X
X <- matrix(c(1,1,1,1,1,1,1,1,
		  1,1,0,0,0,0,0,0,
		  0,0,1,1,0,0,0,0,
		  0,0,0,0,1,1,0,0,
		  0,0,0,0,0,0,1,1,
		  1,0,1,0,1,0,1,0,
		  0,1,0,1,0,1,0,1),8,7)
X
```

```
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7]
## [1,]    1    1    0    0    0    1    0
## [2,]    1    1    0    0    0    0    1
## [3,]    1    0    1    0    0    1    0
## [4,]    1    0    1    0    0    0    1
## [5,]    1    0    0    1    0    1    0
## [6,]    1    0    0    1    0    0    1
## [7,]    1    0    0    0    1    1    0
## [8,]    1    0    0    0    1    0    1
```

```r
# X'X
t(X)%*%X
```

```
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7]
## [1,]    8    2    2    2    2    4    4
## [2,]    2    2    0    0    0    1    1
## [3,]    2    0    2    0    0    1    1
## [4,]    2    0    0    2    0    1    1
## [5,]    2    0    0    0    2    1    1
## [6,]    4    1    1    1    1    4    0
## [7,]    4    1    1    1    1    0    4
```

```r
# A basis for the two dimensional null space is (-2,1,1,1,1,1,1) and (-1,1,1,1,1,0,0)
# Vectors not simultaneously orthogonal to these include the baseline constraint vectors (0,0,0,0,1,0,0) and (0,0,0,0,0,0,1)
Solve(t(X)%*%X, matrix(c(0,0,0,0,0,0,0),7,1))
```

```
## x1         + x5     + x7  =  0 
##   x2     - 1*x5           =  0 
##     x3   - 1*x5           =  0 
##       x4 - 1*x5           =  0 
##                x6 - 1*x7  =  0 
##                        0  =  0 
##                        0  =  0
```

```r
# Augmented Matrix W
W <- rbind(t(X)%*%X, c(0,0,0,0,1,0,0), c(0,0,0,0,0,0,1))
W
```

```
##       [,1] [,2] [,3] [,4] [,5] [,6] [,7]
##  [1,]    8    2    2    2    2    4    4
##  [2,]    2    2    0    0    0    1    1
##  [3,]    2    0    2    0    0    1    1
##  [4,]    2    0    0    2    0    1    1
##  [5,]    2    0    0    0    2    1    1
##  [6,]    4    1    1    1    1    4    0
##  [7,]    4    1    1    1    1    0    4
##  [8,]    0    0    0    0    1    0    0
##  [9,]    0    0    0    0    0    0    1
```

```r
# Generate some response vector for illustration
Y <- matrix(rnorm(8,X%*%matrix(c(1,1,2,3,1,1,2),7,1),1),8,1)
Y
```

```
##          [,1]
## [1,] 4.761509
## [2,] 4.862557
## [3,] 3.607456
## [4,] 4.915798
## [5,] 5.210830
## [6,] 5.262519
## [7,] 0.790821
## [8,] 4.484973
```

```r
# Denote [X'Y 0]' by b
b <- rbind(t(X)%*%Y,0,0)
b
```

```
##            [,1]
##  [1,] 33.896463
##  [2,]  9.624066
##  [3,]  8.523253
##  [4,] 10.473349
##  [5,]  5.275794
##  [6,] 14.370615
##  [7,] 19.525847
##  [8,]  0.000000
##  [9,]  0.000000
```

```r
# Solve the augmented normal equations [X'X K]' beta = [X'Y 0]',
# and note that there is a unique solution that zeroes out one treatment and one block coefficient
beta.hat <- Solve(W, b)
```

```
## x1              =   3.28230108 
##   x2            =   2.17413592 
##     x3          =   1.62372966 
##       x4        =   2.59877749 
##         x5      =            0 
##           x6    =  -1.28880804 
##             x7  =            0 
##              0  =            0 
##              0  =            0
```

```r
# Alternatively, compute the constrained LS estimator and check it is the same as the solution to the augmented normal equations
K <-  rbind(c(0,0,0,0,1,0,0), c(0,0,0,0,0,0,1))
Z <- t(X)%*%X + t(K)%*%K
inv(Z)%*%t(X)%*%Y
```

```
##           [,1]
## [1,]  3.282301
## [2,]  2.174136
## [3,]  1.623730
## [4,]  2.598777
## [5,]  0.000000
## [6,] -1.288808
## [7,]  0.000000
```




