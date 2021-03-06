---
title: geom_ribbon | Examples | Plotly
name: geom_ribbon
permalink: ggplot2/geom_ribbon/
description: How to make plots with geom_ribbon in ggplot2 and R.
layout: base
thumbnail: thumbnail/geom_ribbon.jpg
language: ggplot2
page_type: example_index
has_thumbnail: true
display_as: statistical
order: 5
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

### Line & Ribbon 


```r
library(plotly)

set.seed(1)
y <- sin(seq(1, 2*pi, length.out = 100))
x <- 1:100
plotdata <- data.frame(x=x, y=y, lower = (y+runif(100, -1, -0.5)), upper = (y+runif(100, 0.5, 1)))

p <- ggplot(plotdata) + geom_line(aes(y=y, x=x, colour = "sin"))+
    geom_ribbon(aes(ymin=lower, ymax=upper, x=x, fill = "band"), alpha = 0.3)+
    scale_colour_manual("",values="blue")+
    scale_fill_manual("",values="grey12")

p <- ggplotly()

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_ribbon/line-ribbon")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4162.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>
Inspired by <a href="">ggplot2 docs</a>

### Facets


```r
library(plotly)

set.seed(1987)
pkgs <- c("ggplot2", "mgcv", "MASS")
invisible(lapply(pkgs, require, character.only = TRUE))

load(url('http://biostat.mc.vanderbilt.edu/wiki/pub/Main/DataSets/titanic3.sav'))
titanic3 <- na.omit(titanic3[, -c(3,8:14)])
titanic3$class_sex <- apply(titanic3, 1,
                            function(x) paste(x[1], x[3], collapse = "_"))
titanic3$class_sex <- factor(titanic3$class_sex)
train <- titanic3[sample(row.names(titanic3),
                         size = round(nrow(titanic3) / 2)), ]
test <- titanic3[!(row.names(titanic3) %in% row.names(train)), ]

sim.data <- expand.grid(sex = c("male", "female"), sibsp = 0,
                        age = seq(1, 80), pclass = c("1st", "2nd", "3rd"))

glm.fit <- glm(survived ~ poly(age, 2) * sex * pclass + sibsp,
               "binomial", train)

inv.logit <- function(x) exp(x) / (1 + exp(x))
glm.pred <- predict(glm.fit, newdata = test, se.fit = TRUE)
pred <- data.frame(mean = inv.logit(glm.pred$fit),
                   lo = inv.logit(glm.pred$fit - 2 * glm.pred$se.fit),
                   hi = inv.logit(glm.pred$fit + 2 * glm.pred$se.fit),
                   survived = test$survived)
pred <- pred[order(pred$mean), ]
pred$id <- seq_along(pred$mean)
row.names(pred) <- NULL

pred <- predict(glm.fit, newdata = sim.data, se.fit = TRUE)
sim.data$mean <- inv.logit(pred$fit)
sim.data$lo <- inv.logit(pred$fit - 2 * pred$se.fit)
sim.data$hi <- inv.logit(pred$fit + 2 * pred$se.fit)

p <- ggplot(titanic3, aes(x = age, y = survived))
p <- p + geom_point()
p <- p + facet_grid(sex ~ pclass)
p <- p + geom_line(data = sim.data, aes(y = mean))
p <- p + geom_ribbon(data = sim.data, aes(y = mean, ymin = lo, ymax = hi),
                     alpha = .25)
p <- p + labs(x = "Passenger Age", y = "Probability of Survival")

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_ribbon/facets")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4164.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>
Inspired by <a href="http://zmjones.com/titanic/">Zachary Jones</a>

### Facetwrap & Smooth


```r
library(plotly)

set.seed(42)
n <- 100

df <- data.frame(location = rep(LETTERS[1:4], n),
                 score    = sample(45:80, 4*n, replace = TRUE))

df$p    <- inv.logit(0.075 * df$score + rep(c(-4.5, -5, -6, -2.8), n))
df$pass <- sapply(df$p, function(x){rbinom(1, 1, x)})

g <- glm(pass ~ location + score, data = df, family = 'binomial')

new.data <- expand.grid(score    = seq(46, 75, length = n), 
                        location = LETTERS[1:4])

preds <- predict(g, newdata = new.data, type = 'response',se = TRUE)
new.data$pred.full <- preds$fit

new.data$ymin <- new.data$pred.full - 2*preds$se.fit
new.data$ymax <- new.data$pred.full + 2*preds$se.fit

p <- ggplot(df,aes(x = score, y = pass)) +
    facet_wrap(~location) +
    geom_point(size=1) +
    geom_ribbon(data = new.data,aes(y = pred.full, ymin = ymin, ymax = ymax),alpha = 0.25) +
    geom_line(data = new.data,aes(y = pred.full),colour = "blue")

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_ribbon/facetwrap")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4166.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>
Inspired by <a href="http://stackoverflow.com/questions/8662018/ggplot2-stat-smooth-for-logistic-outcomes-with-facet-wrap-returning-full-or">Stack Overflow</a>

### Prediction Bands


```r
library(plotly)

set.seed(42)
x <- rep(0:100,10)
y <- 15 + 2*rnorm(1010,10,4)*x + rnorm(1010,20,100)
id<-rep(1:10,each=101)

dtfr <- data.frame(x=x ,y=y, id=id)

library(nlme)

model.mx <- lme(y~x,random=~1+x|id,data=dtfr)

#create data.frame with new values for predictors
#more than one predictor is possible
new.dat <- data.frame(x=0:100)
#predict response
new.dat$pred <- predict(model.mx, newdata=new.dat,level=0)

#create design matrix
Designmat <- model.matrix(eval(eval(model.mx$call$fixed)[-2]), new.dat[-ncol(new.dat)])

#compute standard error for predictions
predvar <- diag(Designmat %*% model.mx$varFix %*% t(Designmat))
new.dat$SE <- sqrt(predvar) 
new.dat$SE2 <- sqrt(predvar+model.mx$sigma^2)

library(ggplot2) 
p <- ggplot(new.dat,aes(x=x,y=pred)) + 
geom_line() +
geom_ribbon(aes(ymin=pred-2*SE2,ymax=pred+2*SE2),alpha=0.2,fill="red") +
geom_ribbon(aes(ymin=pred-2*SE,ymax=pred+2*SE),alpha=0.2,fill="blue") +
geom_point(data=dtfr,aes(x=x,y=y), size=1) +
scale_y_continuous("y")

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_ribbon/lme")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4168.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>
Inspired by <a href="http://stackoverflow.com/questions/14358811/extract-prediction-band-from-lme-fit/14435982#14435982">Stack Overflow</a>

### Confidence Bands 


```r
library(plotly)

require(nlme)

set.seed(101)
mp <-data.frame(year=1990:2010)
N <- nrow(mp)

mp <- within(mp,
         {
             wav <- rnorm(N)*cos(2*pi*year)+rnorm(N)*sin(2*pi*year)+5
             wow <- rnorm(N)*wav+rnorm(N)*wav^3
         })

m01 <- gls(wow~poly(wav,3), data=mp, correlation = corARMA(p=1))

fit <- predict(m01)

V <- vcov(m01)
X <- model.matrix(~poly(wav,3),data=mp)
se.fit <- sqrt(diag(X %*% V %*% t(X)))

predframe <- with(mp,data.frame(year,wav,
                                wow=fit,lwr=fit-1.96*se.fit,upr=fit+1.96*se.fit))

p <- ggplot(mp, aes(year, wow))+
    geom_point()+
    geom_line(data=predframe)+
    geom_ribbon(data=predframe,aes(ymin=lwr,ymax=upr),alpha=0.3)

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_ribbon/confidence")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4170.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>
Inspired by <a href="http://stackoverflow.com/questions/14033551/r-plotting-confidence-bands-with-ggplot/14033770#14033770">Stack overflow</a>

### Multiple Layers 


```r
library(plotly)

x=seq(1,10,length=100)
data=data.frame(x,dnorm(x,mean=6.5,sd=1))
names(data)=c('x','new.data')
x.ribbon=seq(1,10,length=20)
ribbon=data.frame(x.ribbon,
                  dnorm(x.ribbon,mean=5,sd=1)+.01,
                  dnorm(x.ribbon,mean=5,sd=1)-.01,
                  dnorm(x.ribbon,mean=5,sd=1))
names(ribbon)=c('x.ribbon','max','min','avg')

p <- ggplot()+geom_ribbon(data=ribbon,aes(ymin=min,ymax=max,x=x.ribbon,fill='lightgreen'))+
    geom_line(data=ribbon,aes(x=x.ribbon,y=avg,color='black'))+
    geom_line(data=data,aes(x=x,y=new.data,color='red'))+
    xlab('x')+ylab('density') + 
    scale_fill_identity() +
    scale_colour_manual(name = 'the colour', 
         values =c('black'='black','red'='red'))

p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = plotly_POST(p, filename="geom_ribbon/layers")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/4172.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>
Inspired by <a href="http://stackoverflow.com/questions/18394391/r-custom-legend-for-multiple-layer-ggplot/18395012#18395012">Stack Overflow</a>
