---
title: geom_boxplot | Examples | Plotly
name: geom_boxplot
permalink: ggplot2/box-plots/
description: How to make a box plot in ggplot2. Examples of box plots in R that are grouped, colored, and display the underlying data distribution.
layout: base
thumbnail: thumbnail/box.jpg
language: ggplot2
page_type: example_index
has_thumbnail: true
display_as: statistical
order: 2
output:
  html_document:
    keep_md: true
---



### New to Plotly?

Plotly's R library is free and open source!<br>
[Get started](https://plot.ly/r/getting-started/) by downloading the client and [reading the primer](https://plot.ly/r/getting-started/).<br>
You can set up Plotly to work in [online](https://plot.ly/r/getting-started/#hosting-graphs-in-your-online-plotly-account) or [offline](https://plot.ly/r/offline/) mode.<br>
We also have a quick-reference [cheatsheet](https://images.plot.ly/plotly-documentation/images/r_cheat_sheet.pdf) (new!) to help you get started!

### Version Check

Version 4 of Plotly's R package is now [available](https://plot.ly/r/getting-started/#installation)!<br>
Check out [this post](http://moderndata.plot.ly/upgrading-to-plotly-4-0-and-above/) for more information on breaking changes and new features available in this version.


```r
library(plotly)
packageVersion('plotly')
```

```
## [1] '4.5.6.9000'
```

### Basic Boxplot


```r
library(plotly)

set.seed(1234)
dat <- data.frame(cond = factor(rep(c("A","B"), each=200)), rating = c(rnorm(200),rnorm(200, mean=.8)))

p <- ggplot(dat, aes(x=cond, y=rating)) + geom_boxplot()

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_boxplot/basic")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4035.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Colored Boxplot


```r
library(plotly)

set.seed(1234)
dat <- data.frame(cond = factor(rep(c("A","B"), each=200)), rating = c(rnorm(200),rnorm(200, mean=.8)))

p <- ggplot(dat, aes(x=cond, y=rating, fill=cond)) + geom_boxplot()

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_boxplot/colored")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4037.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Flipped Boxplot


```r
library(plotly)

set.seed(1234)
dat <- data.frame(cond = factor(rep(c("A","B"), each=200)), rating = c(rnorm(200),rnorm(200, mean=.8)))

p <- ggplot(dat, aes(x=cond, y=rating, fill=cond)) + geom_boxplot() +
    guides(fill=FALSE) + coord_flip()

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_boxplot/flipped")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4039.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Boxplot w/ Stats


```r
library(plotly)

set.seed(1234)
dat <- data.frame(cond = factor(rep(c("A","B"), each=200)), rating = c(rnorm(200),rnorm(200, mean=.8)))

p <- ggplot(dat, aes(x=cond, y=rating)) + geom_boxplot() +
    stat_summary(fun.y=mean, geom="point", shape=5, size=4)

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_boxplot/stats")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4041.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Boxplot Facets


```r
library(plyr)
library(reshape2)
library(plotly)

set.seed(1234)
x<- rnorm(100)
y.1<-rnorm(100)
y.2<-rnorm(100)
y.3<-rnorm(100)
y.4<-rnorm(100)

df<- (as.data.frame(cbind(x,y.1,y.2,y.3,y.4)))

dfmelt<-melt(df, measure.vars = 2:5)

p <- ggplot(dfmelt, aes(x=factor(round_any(x,0.5)), y=value,fill=variable))+
    geom_boxplot()+
    facet_grid(.~variable)+
    labs(x="X (binned)")+
    theme(axis.text.x=element_text(angle=-90, vjust=0.4,hjust=1))

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_boxplot/facets")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4043.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Time Series Facets


```r
library(foreign)
library(MASS)
library(Hmisc)
```

```
## Error in library(Hmisc): there is no package called 'Hmisc'
```

```r
library(reshape2)
library(plotly)

dat <- read.dta("http://www.ats.ucla.edu/stat/data/ologit.dta")
lapply(dat[, c("apply", "pared", "public")], table)
ftable(xtabs(~ public + apply + pared, data = dat))

p <- ggplot(dat, aes(x = apply, y = gpa)) +
    geom_boxplot(size = .75) +
    facet_grid(pared ~ public, margins = TRUE)

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_boxplot/time-series")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4045.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Outliers


```r
library(plotly)
set.seed(123)

df <- diamonds[sample(1:nrow(diamonds), size = 1000),]

p <- ggplot(df, aes(cut, price, fill = cut)) + 
  geom_boxplot(outlier.shape = NA) + 
  ggtitle("Ignore outliers in ggplot2")

# Need to modify the plotly object and make outlier points have opacity equal to 0
p <- plotly_build(p)

p$data <- lapply(p$data, FUN = function(x){
  x$marker = list(opacity = 0)
  return(x)
})


# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_boxplot/outliers")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4047.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Linewidth


```r
library(plotly)
set.seed(123)

df <- diamonds[sample(1:nrow(diamonds), size = 1000),]

p <- ggplot(df, aes(cut, price, fill = cut)) + 
  geom_boxplot(size = 1) + 
  ggtitle("Adjust line width of boxplot in ggplot2")

# Need to modify the plotly object to make sure line width is larger than default
p <- plotly_build(p)

p$data <- lapply(p$data, FUN = function(x){
  x$line = list(width = 10)
  return(x)
})


# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_boxplot/linewidth")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4049.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Whiskers


```r
library(plotly)
set.seed(123)

df <- diamonds[sample(1:nrow(diamonds), size = 1000),]

# This is how it needs to be done in ggplot
p <- ggplot(df, aes(color, price)) +
  stat_boxplot(geom ='errorbar') + 
  geom_boxplot()+
  ggtitle("Add horizontal lines to whiskers using ggplot2")

# Note that plotly will automatically add horozontal lines to the whiskers
p <- ggplot(df, aes(cut, price, fill = cut)) +
  geom_boxplot()+
  ggtitle("Add horizontal lines to whiskers using ggplot2")

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_boxplot/whiskers")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4051.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

These example were inspired by <a href="http://www.cookbook-r.com/Graphs/Plotting_distributions_(ggplot2)/">Cookbook for R</a>.
