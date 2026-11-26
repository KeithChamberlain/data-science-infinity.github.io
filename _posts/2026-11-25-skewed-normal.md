---
layout: post
title: "Skewed"
author: "Keith Chamberlain"
date: "2026-11-25"
image: "/posts/skewed.jpg"
tags: [R, skew, distribution, truncated]
---


# Hangs in the Balance

Maybe we make too much out of distributions of data, yet maybe not. When
confronted with a skewed up distribution there may be more questions
than answers. When using a definition of model error based on its
square, outliers pull at the seams. When using absolute error thinking,
when counting our chickens, or when data gets bound up, our senses
dwindle when data no longer takes the usual shape. There are different
traditions that guide when an analysis hangs in the balance.

In libraries we obtain the key literature with methods to contain
asymmetries. Error makes no mistake either way, rather, the facts we use
to decide when a model fits right for the job. The R package: Generative
Additive Models for Location Scale and Shape, `{gamlss}` R. A. Rigby and
D. M. Stasinopoulos (2005), a frequently flown location, weighs the
shape of things to come. Some situations of data are typically
non-usual. Consider when communicating data boundaries. Reproducing
normal distributions with… skew… via pirates (R) instead of snakes
(python) this time.

# Real Life Situations

1.  Near boundaries
2.  Multiplicative error
3.  Curved transform from the youge (from normal)
4.  Multiple (less prominent) groups
5.  Wait times/time to
6.  When variance increases with the mean

# Boundary Conditions

Boundaries show up for a few reasons. Transformations can introduce
bounds, transformations may be used for responding to some bounds, the
measurement scale mins or maxes out at some value and the data has
frequencies that wont cross: distance, age, quantity are always zero or
positive, yard-sticks don’t go below 0 and people don’t get younger.
Beyond a certain condition highly correlated with age, people don’t get
any older either; lives are punctuated at both extremes.

## Extended Family

`{gamlss}` extends the base family of distributions in R. As a result,
we have a normal family with bounds, which can introduce skew, as
confusing as that sounds.

``` r
library(gamlss.tr)
```

    ## Loading required package: gamlss.dist

    ## Loading required package: gamlss

    ## Loading required package: splines

    ## Loading required package: gamlss.data

    ## 
    ## Attaching package: 'gamlss.data'

    ## The following object is masked from 'package:datasets':
    ## 
    ##     sleep

    ## Loading required package: nlme

    ## Loading required package: parallel

    ##  **********   GAMLSS Version 5.5-0  **********

    ## For more on GAMLSS look at https://www.gamlss.com/

    ## Type gamlssNews() to see new features/changes/bug fixes.

``` r
# Create a normal family based ten trunctated distribution to the left
gen.trun(par=c(10), family="NO", name="ten", type="left")
```

    ## A truncated family of distributions from NO has been generated 
    ##  and saved under the names:  
    ##  dNOten pNOten qNOten rNOten NOten 
    ## The type of truncation is left 
    ##  and the truncation parameter is 10

``` r
set.seed(1023)
dat <- rNOten(n=10000, mu=15, sigma=7)
hist(dat)
```

![](../img/skewed_normal.png)<!-- -->

## Built

Couldn’t help but simplify from the basics: pfun set to `pnrom()`,
called from within `runif()` called within qfun set to `qnorm()`. The
result is identical to the `rNOten()` call created using `gen.trun()`.

``` r
truncate <- function(n=10000, left=-Inf, right=Inf, ..., log.p=FALSE,
                     pfun=pnorm, qfun=qnorm){
    '
    truncate returns a distribution with optional left/right truncation for
    pfun & qfun with mean/sd or shape/scale or mu/sigma passed in `...`.
    '
    qfun(runif(
        n=n,
        min=if(left==-Inf) 0 else pfun(q=left, ..., log.p=log.p),
        max=if(right==Inf) 1 else pfun(q=right, ..., log.p=log.p)),
        ...)
}

set.seed(1023)
dat2 <- truncate(n=10000, left=10, mean=15, sd=7, right=Inf)
print(paste("Dat and dat2 are exactly equal: ", all.equal(dat, dat2)))
```

    ## [1] "Dat and dat2 are exactly equal:  TRUE"

``` r
hist(dat2)
```

![](../img/skewed_replicated.png)<!-- -->

# Conclusion

The skewed, so called truncated `normal` distribution. Consider this and
other creations for exploratory data analysis in my R_EDA repository,
including a multi-modal based version of the modified-z-transform on my
[https://www.github.com/keithchamberlain/R_EDA/](github).

# References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-gamlss2005" class="csl-entry">

R. A. Rigby, and D. M. Stasinopoulos. 2005. “Generalized Additive Models
for Location, Scale and Shape,(with Discussion).” *Applied Statistics*
54.3: 507–54.

</div>

</div>
