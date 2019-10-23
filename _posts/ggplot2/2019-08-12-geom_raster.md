---
name: geom_raster
permalink: ggplot2/geom_raster/
description: How to make a 2-dimensional heatmap in ggplot2 using geom_raster.
layout: base
thumbnail: thumbnail/geom_raster.jpg
language: ggplot2
page_type: example_index
has_thumbnail: true
display_as: basic
order: 7
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

### Basic 2d Heatmap
geom\_raster creates a coloured heatmap, with two variables acting as the x- and y-coordinates and a third variable mapping onto a colour. (It is coded similarly to geom\_tile and is generated more quickly.) This uses the volcano dataset that comes pre-loaded with R.


```r
library(reshape2)
library(plotly)

df <- melt(volcano)

p <- ggplot(df, aes(Var1, Var2)) +
  geom_raster(aes(fill=value)) +
  labs(x="West to East",
       y="North to South",
       title = "Elevation map of Maunga Whau")
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_raster/basic-chart")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5850.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Customized 2d Heatmap
This uses the Spectral palette from [ColorBrewer](https://ggplot2.tidyverse.org/reference/scale_brewer.html); a full list of palettes is here.


```r
library(reshape2)
library(plotly)

df <- melt(volcano)

p <- ggplot(df, aes(Var1, Var2)) +
  geom_raster(aes(fill=value)) +
  scale_fill_distiller(palette = "Spectral", direction = -1) +
  labs(x="West to East",
       y="North to South",
       title = "Elevation map of Maunga Whau",
       fill = "Elevation") +
  theme(text = element_text(family = 'Fira Sans'),
        plot.title = element_text(hjust = 0.5))
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_raster/colour-scales")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5852.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

