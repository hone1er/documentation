---
name: geom_count
permalink: ggplot2/geom_count/
description: How to make a 2-dimensional frequency graph in ggplot2 using geom_count Examples of coloured and facetted graphs.
layout: base
thumbnail: thumbnail/geom_count.jpg
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
## [1] '4.8.0.9000'
```

### Basic geom\_count Plot
geom\_count is a way to plot two variables that are not continuous. Here's a modified version of the nycflights13 dataset that comes with R; it shows 2013 domestic flights leaving New York's three airports. This graph maps two categorical variables: which of America's major airports it was headed to, and which major carrier was operating it.

It's good to show the ful airport names for destinations, rather than just the airport codes. You can use aes(group = ), which doesn't modify the graph in any way but adds information to the labels.


```r
library(plotly)
flightdata <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/flightdata.csv", stringsAsFactors = FALSE)

p <- ggplot(flightdata, aes(y=airline, x=dest, colour = dest, group=airport)) +
  geom_count(alpha=0.5) +
  labs(title = "Flights from New York to major domestic destinations",
       x = "Origin and destination",
       y = "Airline",
       size = "")
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_count/basic-plot")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5819.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Adding a Third Variable
By using facets, we can add a third variable: which of New York's three airports it departed from. We can also colour-code by this variable.


```r
library(plotly)
flightdata <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/flightdata.csv", stringsAsFactors = FALSE)

p <- ggplot(flightdata, aes(y=airline, x=origin, colour=origin, group=airport)) +
  geom_count(alpha=0.5) +
  facet_grid(. ~ dest) +
  labs(title = "Flights from New York to major domestic destinations",
       x = "Origin and destination",
       y = "Airline",
       size = "")
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_count/three-variables")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5821.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Customized appearance
The airport labels at the bottom aren't very visible and aren't very important, since there's a colour key to the side; we can get rid of the text and ticks using theme() options. Let's also use the LaCroixColoR package to give this geom\_count chart a new colour scheme.


```r
library(plotly)
library(LaCroixColoR)
flightdata <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/flightdata.csv", stringsAsFactors = FALSE)

p <- ggplot(flightdata, aes(y=airline, x=origin, colour=origin, group=airport)) +
  geom_count(alpha=0.5) +
  facet_grid(. ~ dest) +
  scale_colour_manual(values = lacroix_palette("PassionFruit", n=3)) +
  theme(axis.text.x = element_blank(),
        axis.ticks.x = element_blank()) +
  labs(title = "Flights from New York to major domestic destinations",
       x = "Origin and destination",
       y = "Airline",
       size = "")
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_count/customize-theme")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5823.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### geom\_count vs geom\_point
Here's a comparison of geom\_count and geom\_point on the same dataset (rounded for geom\_count). Geom\_point has the advantage of allowing multiple colours on the same graph, as well as a label for each point. But even with a low alpha, there are too many overlapping points to understand what the actual distribution looks like, only a general impression.


```r
library(plotly)
library(dplyr)
beers <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/beers.csv", stringsAsFactors = FALSE)

df <- beers %>%
  mutate(abv = round(abv*100),
         ibu = round(ibu/10)*10) %>%
  filter(!is.na(style2))

p <- ggplot(df, aes(x=abv, y=ibu, colour=style2)) +
  geom_count(alpha=0.5) +
  theme(legend.position = "none") +
  facet_wrap(~style2)
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_count/compare-count")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5825.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>


```r
library(plotly)
library(dplyr)
beers <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/beers.csv", stringsAsFactors = FALSE)

df <- filter(beers, !is.na(style2))

p <- ggplot(df, aes(x=abv, y=ibu, colour=style2)) +
  geom_point(alpha=0.2, aes(text = label)) +
  theme(legend.position = "none") +
  facet_wrap(~style2) +
  labs(y = "bitterness (IBU)",
       x = "alcohol volume (ABV)",
       title = "Craft beers from American breweries")
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_count/compare-point")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5827.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>
