## Multivariate Visualizations with NGS data

We previously explored multivarate tests with the fly data [(lesson here)](https://github.com/ryanjw/ngs-3rdweek/blob/master/multivariate-tests/tests.md), and now we are going to work on different ways of visualizing the data. 

Here we are going to use some multivariate visualizations with a particular goal in mind.  We assume that in addition to the fly data we had previously, 20 more fly transcriptomes were sequenced.  Unfortunately, someone forgot to label the samples.  We know that two different flies (combination of `fly` and `type`) were sequenced.  

Let's download the new data
```R
URL<-("https://raw.githubusercontent.com/ryanjw/ngs-3rdweek/master/multivariate-viz/fly_data_with_unknowns.txt")
dataset<-read.table(textConnection(getURL(URL)),header=T,check.names=F,sep="\t")
```
If you can't use `getURL` modify the URL in the code [here](https://github.com/ryanjw/ngs-3rdweek/blob/master/multivariate-tests/alternative-download.md)