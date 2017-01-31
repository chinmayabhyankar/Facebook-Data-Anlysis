Import Data
-----------

``` r
fd = read.csv('pseudo_facebook.tsv',sep ='\t')
```

Histogram of Birthday
---------------------

31st of of every month has low count which makes sense as not every month has 31 days 1st day of month had a huge peak which seemed weird.

``` r
# install.packages('ggplot2')
library(ggplot2)
qplot(x= dob_day, data = fd, binwidth = .5, color = 'black', fill = '#F79420') + 
  scale_x_continuous(breaks = 1:31)
```

![](FacebookDataAnalysis_files/figure-markdown_github/unnamed-chunk-2-1.png)

Later I tried to dive in further to get more insights. I saw that most of the peak is for 1st January which could be a default setting for the users who did not add their birthdays.

``` r
qplot(x= dob_day, data = fd, binwidth = .5, color = 'black', fill = '#F79420') + 
  scale_x_continuous(breaks = 1:31) +
  facet_wrap(~dob_month)
```

![](FacebookDataAnalysis_files/figure-markdown_github/unnamed-chunk-3-1.png)

Histogram of Friendcount
------------------------

Its a right skewed data with most entries in the range of 0 to 1000

``` r
qplot(x= friend_count, data = fd, binwidth = 3, color = 'black', fill = '#F79420')
```

![](FacebookDataAnalysis_files/figure-markdown_github/unnamed-chunk-4-1.png)

Setting a limit on axis

``` r
qplot(x= friend_count, data = fd, binwidth = 10, color = 'black', fill = '#F79420') + 
  scale_x_continuous(limits = c(0,1000))
```

![](FacebookDataAnalysis_files/figure-markdown_github/unnamed-chunk-5-1.png)

Analyzing friend count by gender ![](FacebookDataAnalysis_files/figure-markdown_github/unnamed-chunk-6-1.png) \#statistics by gender solution

``` r
by(fd$friend_count, fd$gender, summary)
```

    ## fd$gender: female
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       0      37      96     242     244    4923 
    ## -------------------------------------------------------- 
    ## fd$gender: male
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       0      27      74     165     182    4917

log transformation works better on skewed data
==============================================

``` r
p = qplot(x= friend_count, data = fd, fill = '#F79420')
p1 = qplot(x= log10(friend_count + 1), data = fd, fill = '#F79420')
#install.packages('gridExtra')
library(gridExtra)
grid.arrange(p,p1, ncol = 1)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](FacebookDataAnalysis_files/figure-markdown_github/unnamed-chunk-8-1.png) \#Frequency Polygon to comapare counts histograms by gender

``` r
ggplot(aes(x = friend_count, y = ..count../sum(..count..)), data = subset(fd, !is.na(gender))) + 
  geom_freqpoly(aes(color = gender), binwidth=10) + 
  scale_x_continuous(limits = c(0, 1000), breaks = seq(0, 1000, 50)) + 
  xlab('Friend Count') + 
  ylab('Percentage of users with that friend count')
```

![](FacebookDataAnalysis_files/figure-markdown_github/unnamed-chunk-9-1.png)

Analyzing likes by gender
=========================

![](FacebookDataAnalysis_files/figure-markdown_github/unnamed-chunk-10-1.png)

Analysis to see who initiates a friend request on an average
============================================================

![](FacebookDataAnalysis_files/figure-markdown_github/unnamed-chunk-11-1.png)

checking if facebook should concerntrate on their mobile app
============================================================

For this we can check what percent of people actually use facebook on their mobile This can be found out using mobile\_like attribute in the dataset.

    ## 
    ##     0     1 
    ## 35056 63947

This tells that more than 50 percent people use facebook on mobile
