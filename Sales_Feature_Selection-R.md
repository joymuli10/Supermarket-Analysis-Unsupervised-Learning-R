``` r
df_feature <- read.csv(file = 'http://bit.ly/CarreFourDataset')
head(df_feature)
```

    ##    Invoice.ID Branch Customer.type Gender           Product.line Unit.price
    ## 1 750-67-8428      A        Member Female      Health and beauty      74.69
    ## 2 226-31-3081      C        Normal Female Electronic accessories      15.28
    ## 3 631-41-3108      A        Normal   Male     Home and lifestyle      46.33
    ## 4 123-19-1176      A        Member   Male      Health and beauty      58.22
    ## 5 373-73-7910      A        Normal   Male      Sports and travel      86.31
    ## 6 699-14-3026      C        Normal   Male Electronic accessories      85.39
    ##   Quantity     Tax      Date  Time     Payment   cogs gross.margin.percentage
    ## 1        7 26.1415  1/5/2019 13:08     Ewallet 522.83                4.761905
    ## 2        5  3.8200  3/8/2019 10:29        Cash  76.40                4.761905
    ## 3        7 16.2155  3/3/2019 13:23 Credit card 324.31                4.761905
    ## 4        8 23.2880 1/27/2019 20:33     Ewallet 465.76                4.761905
    ## 5        7 30.2085  2/8/2019 10:37     Ewallet 604.17                4.761905
    ## 6        7 29.8865 3/25/2019 18:30     Ewallet 597.73                4.761905
    ##   gross.income Rating    Total
    ## 1      26.1415    9.1 548.9715
    ## 2       3.8200    9.6  80.2200
    ## 3      16.2155    7.4 340.5255
    ## 4      23.2880    8.4 489.0480
    ## 5      30.2085    5.3 634.3785
    ## 6      29.8865    4.1 627.6165

Feature Ranking
===============

We will use the FSelector Package. This is a package containing functions for selecting attributes from a given dataset.
========================================================================================================================

``` r
suppressWarnings(
        suppressMessages(if
                         (!require(FSelector, quietly=TRUE))
                install.packages("FSelector")))
library(FSelector)
```

``` r
num_cols <- unlist(lapply(df_feature, is.numeric))
num_cols
```

    ##              Invoice.ID                  Branch           Customer.type 
    ##                   FALSE                   FALSE                   FALSE 
    ##                  Gender            Product.line              Unit.price 
    ##                   FALSE                   FALSE                    TRUE 
    ##                Quantity                     Tax                    Date 
    ##                    TRUE                    TRUE                   FALSE 
    ##                    Time                 Payment                    cogs 
    ##                   FALSE                   FALSE                    TRUE 
    ## gross.margin.percentage            gross.income                  Rating 
    ##                    TRUE                    TRUE                    TRUE 
    ##                   Total 
    ##                    TRUE

``` r
# Subset numeric columns
data_num <- df_feature[ ,num_cols]
```

From the FSelector package, we use the correlation coefficient as a unit of valuation.
======================================================================================

This would be one of the several algorithms contained
=====================================================

in the FSelector package that can be used rank the variables.
=============================================================

``` r
Scores <- linear.correlation(Total~., data_num)
Scores
```

    ##                         attr_importance
    ## Unit.price                    0.6339621
    ## Quantity                      0.7055102
    ## Tax                           1.0000000
    ## cogs                          1.0000000
    ## gross.margin.percentage              NA
    ## gross.income                  1.0000000
    ## Rating                        0.0364417

Getting the top 5 most representative variables
===============================================

``` r
Subset <- cutoff.k(Scores, 5)
as.data.frame(Subset)
```

    ##         Subset
    ## 1          Tax
    ## 2         cogs
    ## 3 gross.income
    ## 4     Quantity
    ## 5   Unit.price

Getting the best variables
==========================

``` r
Subset1 <-cutoff.k.percent(Scores, 0.4)
as.data.frame(Subset1)
```

    ##        Subset1
    ## 1          Tax
    ## 2         cogs
    ## 3 gross.income

``` r
Subset1 <-cutoff.k.percent(Scores, 0.6)
as.data.frame(Subset1)
```

    ##        Subset1
    ## 1          Tax
    ## 2         cogs
    ## 3 gross.income
    ## 4     Quantity

``` r
Subset1 <-cutoff.k.percent(Scores, 0.8)
as.data.frame(Subset1)
```

    ##        Subset1
    ## 1          Tax
    ## 2         cogs
    ## 3 gross.income
    ## 4     Quantity
    ## 5   Unit.price
    ## 6       Rating

The most important features in descending order are ‘Tax’, ‘cogs’,
‘gross.income’, ‘Quantity’, ‘Unit.price’, ‘Rating’
