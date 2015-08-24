## Multivariate tests with NGS data

The tests I will present here can be used universally across multivariate data.

First, either download the data or get it directly from the datasets directory using a function from the RCurl package.

Let's download and install the package first
```R
install.packages("RCurl")
library(RCurl)
```
Using a combination of functions from R, we will create an object called dataset that holds all of the fly data
```R
dataset<-read.table(textConnection(getURL(URL)),header=T,check.names=F,sep="\t")
```
If you are unable to do download RCurl, [run this code instead](https://github.com/ryanjw/ngs-3rdweek/blob/master/multivariate-tests/alternative-download.md)

Let's look at the first few rows and columns of the dataset.
```R
head(dataset[,1:10])
```
The output should look like this
```R
  fly type                           file FBgn0000003 FBgn0000008 FBgn0000014 FBgn0000015 FBgn0000017 FBgn0000018 FBgn0000022
1 HYB sdE3 HYB_sdE3_rep1_htseq_counts.txt           0           0           0           0           0           0         200
2 HYB sdE3 HYB_sdE3_rep2_htseq_counts.txt           0           0           0           0           0           0         319
3 HYB   wt   HYB_wt_rep1_htseq_counts.txt           0           0           0           0           0           0         509
4 HYB   wt   HYB_wt_rep2_htseq_counts.txt           0           0           0           0           0           0         331
5 ORE sdE3 ORE_sdE3_rep1_htseq_counts.txt           0           0           0           0           0           0         385
6 ORE sdE3 ORE_sdE3_rep2_htseq_counts.txt           0           0           0           0           0           0         312
```
We can see several columns describing what fly strain, mutant type, and other metadata regarding these samples.  Starting in column three, we can see the counts of specific contigs from this RNAseq experiment.

### Multivariate analysis of variance

There are several options to determine whether or not experimental designs produce significantly different results.  A classic way of doing this is using multivariate analysis of variance or MANOVA.  A major assumption of MANOVA is that variables within each treatment level come from a multivariate normal distribution.

To run a MANOVA, we set up a linear model first and then nest a few functions together.  Here we demonstrate a Wilk's lambda, which is common in the literature.  Note that there are several options; run `?summary.manova` to explore them.  **This code may be too intense for your laptop, consider using EC2**. 
```R
test<-lm(as.matrix(dataset[,-c(1:3)])~dataset$fly)
summary(manova(test),test="Wilks")
``` 
If you run this code, it may not work...see output below
```R
Error in qr.default(D %*% ss[[nt]] %*% D, tol = tol) :
  NA/NaN/Inf in foreign function call (arg 1)
 ```
This code ends up failing, but why may this be?  Is our data violating some assumptions?  Let's grab a few random variables from the dataset and see how they are distributed.
```R
library(ggplot2)
ggplot(dataset)+geom_density(aes(x=FBgn0000022,fill=fly,alpha=0.5))
```
(https://raw.githubusercontent.com/ryanjw/ngs-3rdweek/master/multivariate-tests/fly-density-plot.jpg)
##Challenge
*Try looking at another variable by replacing `FBgn0000022` with another variable name and coloring by `type` instead of `fly`


