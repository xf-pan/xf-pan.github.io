---
layout:     post                     # 使用的布局（不需要改）
title:      从最简单开始               # 标题 
subtitle:   multinomial logit model  #副标题
date:       2019-11-16               # 时间
author:     PAN                      # 作者
header-img: img/post-bg-2015.jpg     #这篇文章标题背景图片
catalog: true                        # 是否归档
tags:                                #标签
    - MNL, dicmo, mlogit
---

运行环境
--------

-   R: 3.6.1
-   dicmo: 0.1.0
-   dplyr: 0.8.3
-   mlogit: 1.0-1

packages loading
----------------

    library(dicmo)

    ## dicmo 0.1.0 is still BETA sofrware. Please report any bugs.

    ## URL: https://xf-pan.github.io/dicmo/

    ## BugReports: https://github.com/xf-pan/dicmo/issues

    ## E-mail: X.PAN <evan_cools@sina.com>

    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    library(mlogit)

    ## Loading required package: Formula

    ## Loading required package: zoo

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

    ## Loading required package: lmtest

    data("Heating", package = "mlogit")

MNL using dicmo
---------------

    H1 <- as_tibble(Heating)
    H1 <- rename(H1,
                 'ic:gc' = ic.gc, 'ic:gr' = ic.gr, 'ic:ec' = ic.ec,
                 'ic:er' = ic.er, 'ic:hp' = ic.hp,
                 'oc:gc' = oc.gc, 'oc:gr' = oc.gr, 'oc:ec' = oc.ec,
                 'oc:er' = oc.er, 'oc:hp' = oc.hp)
    attrs <- list(attrs_alts = tibble(ic = c(1,1,1,1,1),
                                      oc = c(1,1,1,1,1)),
                  asc        = NULL,
                  context    = NULL)
    alts <- c('gc', 'gr', 'ec', 'er', 'hp')
    M1 <- X.logit(H1, choice = 'depvar', alts = alts, attrs = attrs)

    ## 2019-11-16 12:12:47 - model estimation starts
    ## 2019-11-16 12:12:47 - model estimation ends

    summary(M1)

    ## --------------------------------------------------------- 
    ## Model name: logit 
    ## Model estimation starts at: 2019-11-16 12:12:47 
    ## Model estimation ends at: 2019-11-16 12:12:47 
    ## Model estimation method: BFGS maximization with numeric Hessian matrix 
    ## Model diagnosis: successful convergence  
    ## --------------------------------------------------------- 
    ## Initial log-likelihood: -1448.494 
    ## Convergent log-likelihood: -1095.237 
    ## Rho sqaured: 0.2439 
    ## Adjusted Rho sqaured: 0.2425 
    ## AIC: 2194.474 
    ## BIC: 2204.079 
    ## Sample size: 900 
    ## Number of estimated parameters: 2 
    ## --------------------------------------------------------- 
    ## Estimates: 
    ##      estimate  std. error   t-value  p-value     
    ## ic  -0.006232    0.000353  -17.6666        0  ***
    ## oc  -0.004580    0.000322  -14.2182        0  ***
    ## ------ 
    ## Signif. codes: *** p < 0.01, ** p < 0.05, * p < 0.1 
    ## ---------------------------------------------------------

MNL using dicmo
---------------

    H2 <- mlogit.data(Heating, shape = "wide", choice = "depvar", varying = c(3:12))
    M2 <- mlogit(depvar ~ ic + oc | 0, H2)
    summary(M2)

    ## 
    ## Call:
    ## mlogit(formula = depvar ~ ic + oc | 0, data = H2, method = "nr")
    ## 
    ## Frequencies of alternatives:
    ##       ec       er       gc       gr       hp 
    ## 0.071111 0.093333 0.636667 0.143333 0.055556 
    ## 
    ## nr method
    ## 4 iterations, 0h:0m:0s 
    ## g'(-H)^-1g = 1.56E-07 
    ## gradient close to zero 
    ## 
    ## Coefficients :
    ##       Estimate  Std. Error z-value  Pr(>|z|)    
    ## ic -0.00623187  0.00035277 -17.665 < 2.2e-16 ***
    ## oc -0.00458008  0.00032216 -14.217 < 2.2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Log-Likelihood: -1095.2



