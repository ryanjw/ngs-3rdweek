## Multivariate tests with NGS data

The tests I will present here can be used universally across multivariate data.

First, either download the data or get it directly from the datasets directory using a function from the RCurl package.

Let's download and install the package first
```
install.packages("RCurl")
library(RCurl)
```
Using a combination of functions from R, we will create an object called dataset that holds all of the fly data
```
dataset<-read.table(textConnection(getURL(URL)),header=T,check.names=F,sep="\t")
```
Let's look at the first few rows and columns of the dataset.
```
head(dataset[,1:10])
```
