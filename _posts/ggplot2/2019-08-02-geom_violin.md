---
name: geom_violin
permalink: ggplot2/geom_violin/
description: How to make a density map using geom_violin. Includes explanations on flipping axes and facetting.
layout: base
thumbnail: thumbnail/geom_violin.jpg
language: ggplot2
page_type: example_index
has_thumbnail: true
display_as: statistical
order: 8
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
## [1] '4.9.0.9000'
```

### Basic violin plot
A basic violin plot showing how Democratic vote share in the 2018 elections to the US House of Representatives varied by level of density. A horizontal bar is added, to divide candidates who lost from those who won.

Source: [Dave Wassermann and Ally Flinn](https://docs.google.com/spreadsheets/d/1WxDaxD5az6kdOjJncmGph37z0BPNhV1fNAH_g7IkpC0/htmlview?sle=true#gid=0) for the election results and CityLab for its [Congressional Density Index](https://github.com/theatlantic/citylab-data/tree/master/citylab-congress). Regional classifications are according to the Census Bureau.


```r
library(plotly)
district_density <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/district_density.csv", stringsAsFactors = FALSE)
district_density$cluster <- factor(district_density$cluster, levels=c("Pure urban", "Urban-suburban mix", "Dense suburban", "Sparse suburban", "Rural-suburban mix", "Pure rural"))
district_density$region <- factor(district_density$region, levels=c("West", "South", "Midwest", "Northeast"))

p <- ggplot(district_density,aes(x=cluster, y=dem_margin, fill=cluster)) +
  geom_violin(colour=NA) +
  geom_hline(yintercept=0, alpha=0.5) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index\nfrom CityLab",
       y = "Margin of Victory/Defeat")
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_violin/basic-graph")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5785.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Flipping the Axes
With geom\_violin(), the y-axis must always be the continuous variable, and the x-axis the categorical variable. To create horizontal violin graphs, keep the x- and y-variables as is and add coord\_flip().


```r
library(plotly)
district_density <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/district_density.csv", stringsAsFactors = FALSE)
district_density$cluster <- factor(district_density$cluster, levels=c("Pure urban", "Urban-suburban mix", "Dense suburban", "Sparse suburban", "Rural-suburban mix", "Pure rural"))
district_density$region <- factor(district_density$region, levels=c("West", "South", "Midwest", "Northeast"))

p <- ggplot(district_density,aes(x=cluster, y=dem_margin, fill=cluster)) +
  geom_violin(colour=NA) +
  geom_hline(yintercept=0, alpha=0.5) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index\nfrom CityLab",
       y = "Margin of Victory/Defeat") +
  coord_flip()
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_violin/flip-axes")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5787.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Add facetting
Including facetting by region.


```r
library(plotly)
district_density <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/district_density.csv", stringsAsFactors = FALSE)
district_density$cluster <- factor(district_density$cluster, levels=c("Pure urban", "Urban-suburban mix", "Dense suburban", "Sparse suburban", "Rural-suburban mix", "Pure rural"))
district_density$region <- factor(district_density$region, levels=c("West", "South", "Midwest", "Northeast"))

p <- ggplot(district_density,aes(x=cluster, y=dem_margin, fill=cluster)) +
  geom_violin(colour=NA) +
  geom_hline(yintercept=0, alpha=0.5) +
  facet_wrap(~region) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index\nfrom CityLab",
       y = "Margin of Victory/Defeat") +
  coord_flip()
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_violin/add-facet")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5789.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Customized Appearance
Add colour to the facet titles, centre-align the title, rotate the y-axis title, change the font, and get rid of the unnecessary legend. Note that coord_flip() flips the axes for the variables and the titles, but does not flip theme() elements.


```r
library(plotly)
district_density <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/district_density.csv", stringsAsFactors = FALSE)
district_density$cluster <- factor(district_density$cluster, levels=c("Pure urban", "Urban-suburban mix", "Dense suburban", "Sparse suburban", "Rural-suburban mix", "Pure rural"))
district_density$region <- factor(district_density$region, levels=c("West", "South", "Midwest", "Northeast"))

p <- ggplot(district_density,aes(x=cluster, y=dem_margin, fill=cluster)) +
  geom_violin(colour=NA) +
  geom_hline(yintercept=0, alpha=0.5) +
  facet_wrap(~region) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index\nfrom CityLab",
       y = "Margin of Victory/Defeat") +
  coord_flip() +
  theme(axis.title.y = element_text(angle = 0, vjust=0.5),
        plot.title = element_text(hjust = 0.5),
        strip.background = element_rect(fill="lightblue"),
        text = element_text(family = 'Fira Sans'),
        legend.position = "none")
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_violin/customize-theme")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5791.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Rotated Axis Text
Rotated the x-axis text 45 degrees, and used facet\_grid to create a 4x1 facet (compared to facet\_wrap, which defaults to 2x2).


```r
library(plotly)
district_density <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/district_density.csv", stringsAsFactors = FALSE)
district_density$cluster <- factor(district_density$cluster, levels=c("Pure urban", "Urban-suburban mix", "Dense suburban", "Sparse suburban", "Rural-suburban mix", "Pure rural"))
district_density$region <- factor(district_density$region, levels=c("West", "South", "Midwest", "Northeast"))

p <- ggplot(district_density,aes(x=cluster, y=dem_margin, fill=cluster)) +
  geom_violin(colour=NA) +
  geom_hline(yintercept=0, alpha=0.5) +
  facet_grid(.~region) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index\nfrom CityLab",
       y = "Margin of Victory/Defeat") +
  theme(axis.text.x = element_text(angle = -45),
        plot.title = element_text(hjust = 0.5),
        strip.background = element_rect(fill="lightblue"),
        text = element_text(family = 'Fira Sans'),
        legend.position = "none")
ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_violin/rotated-text")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5793.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

