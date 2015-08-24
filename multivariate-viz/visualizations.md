## Multivariate Visualizations with NGS data

We previously explored multivarate tests with the fly data [(lesson here)](https://github.com/ryanjw/ngs-3rdweek/blob/master/multivariate-tests/tests.md), and now we are going to work on different ways of visualizing the data. 

Here we are going to use some multivariate visualizations with a particular goal in mind.  We assume that in addition to the fly data we had previously, 40 more fly transcriptomes were sequenced.  Unfortunately, someone forgot to label the samples.  We know that two different flies (combination of `fly` and `type`) were sequenced.  

Let's download the new data
```R
URL<-("https://raw.githubusercontent.com/ryanjw/ngs-3rdweek/master/multivariate-viz/fly_data_with_unknowns.txt")
dataset<-read.table(textConnection(getURL(URL)),header=T,check.names=F,sep="\t")
```
If you can't use `getURL` modify the URL in the code [here](https://github.com/ryanjw/ngs-3rdweek/blob/master/multivariate-tests/alternative-download.md)

Let's look at the first and last few rows of the data

```R
head(dataset[,1:10])
tail(dataset[,1:10])
```

We can see that we have the addition of 40 unknown flies.  [In reality, these data were simulated from the previous samples, and the rough code can be seen here.](https://github.com/ryanjw/ngs-3rdweek/blob/master/multivariate-viz/data-sim.md)

The first visualization we will perform is based on nonmetric multidimensional scaling.  In short, this algorithmic approach samples are placed in N-dimensional space based on a distance matrix.  We then view a 2D representation of these differences and are given a statistic called *stress* which, when minimized, the visualization is easiest to interpret (Smaller Stress, Better Visualization)

Let's generate an NMDS using the `metaMDS` function from the `vegan` package.  We will make an object called `sc` that has the scores (i.e. coordinates in space) for each sample along with metadata from our samples
```R
 mds<-metaMDS(dataset[,-c(1:4)],distance="bray",autotransform=F,k=3 )
 sc<-data.frame(scores(mds),dataset[,1:3])
 head(sc)
 ```

 Let's visualize these relationships via a scatterplot in ggplot
 ```R
 ggplot(sc)+geom_point(aes(x=NMDS1,y=NMDS2))
 ```
 ![alt text](https://raw.githubusercontent.com/ryanjw/ngs-3rdweek/master/multivariate-viz/plain-nmds.jpg)   