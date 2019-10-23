---
name: geom_jitter
permalink: ggplot2/geom_jitter/
description: How to make a graph using geom_jitter.
layout: base
thumbnail: thumbnail/jitter.png
language: ggplot2
page_type: example_index
has_thumbnail: true
display_as: basic
order: 4
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

### Basic Jitter Plot
You can use the "text=" option to control what pops when you hover over each point. (Note: you might get an error message when running this function; ggplot does not recognize it but the plotly function does.)


```r
library(plotly)
district_density <- read.csv("https://raw.githubusercontent.com/plotly/datasets/master/district_density.csv",
                             stringsAsFactors = FALSE)
district_density$cluster <- factor(district_density$cluster, levels=c("Pure urban", "Urban-suburban mix", "Dense suburban", "Sparse suburban", "Rural-suburban mix", "Pure rural"))
district_density$region <- factor(district_density$region, levels=c("West", "South", "Midwest", "Northeast"))

p <- ggplot(district_density,aes(x=cluster, y=dem_margin, colour=region)) +
  geom_jitter(aes(text=paste("district: ", cd_code)), width=0.25, alpha=0.5, ) +
  geom_hline(yintercept=0) +
  theme(axis.text.x = element_text(angle = -30, hjust = 0.1)) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index from CityLab",
       y = "Democratic Margin of Victory/Defeat")
p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_jitter/basic-plot")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5799.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Add Boxplot


```r
library(plotly)

p <- ggplot(district_density,aes(x=cluster, y=dem_margin)) +
  geom_boxplot(fill=NA, alpha=0.5) +
  geom_jitter(aes(colour=region, text=paste("district: ", cd_code)), width=0.25, alpha=0.5) +
  geom_hline(yintercept=0) +
  theme(axis.text.x = element_text(angle = -30, hjust = 0.1)) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index from CityLab",
       y = "Democratic Margin of Victory/Defeat")
p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_jitter/with-boxplot")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5801.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Facetting


```r
library(plotly)

p <- ggplot(district_density,aes(x=region, y=dem_margin, colour=region)) +
  geom_jitter(aes(text=paste("district: ", cd_code)), width=0.25, alpha=0.5, ) +
  geom_hline(yintercept=0) +
  facet_grid(.~cluster) +
  theme(axis.text.x = element_text(angle = -30, hjust = 0.1)) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index from CityLab",
       y = "Democratic Margin of Victory/Defeat")
p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_jitter/facets")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5803.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Adding Text
Tabulating the number of observations in each category, and adding these numbers to the graph.


```r
library(plotly)
library(dplyr)

density_sum <- district_density %>%
  group_by(cluster, region) %>%
  summarise(count = n())

p <- ggplot(district_density,aes(x=region, y=dem_margin, colour=region)) +
  geom_jitter(aes(text=paste("district: ", cd_code)), width=0.25, alpha=0.5, ) +
  geom_hline(yintercept=0) +
  facet_grid(.~cluster) +
  theme(axis.text.x = element_text(angle = -30, hjust = 0.1)) +
  geom_text(data = density_sum, aes(label = count,
                                    x = region, y = -90)) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index from CityLab",
       y = "Democratic Margin of Victory/Defeat")
p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_jitter/add-text")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5805.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Customized Appearance
Centre the title, add colours to the facet label, rotate the labels, change the font, and remove the unnecessary legend.


```r
library(plotly)

p <- ggplot(district_density,aes(x=region, y=dem_margin, colour=region)) +
  geom_jitter(aes(text=paste("district: ", cd_code)), width=0.25, alpha=0.5, ) +
  geom_hline(yintercept=0) +
  facet_grid(.~cluster) +
  geom_text(data = density_sum, aes(label = count,
                                    x = region, y = -90)) +
  theme(axis.text.x = element_text(angle = -30, hjust = 0.1),
        plot.title = element_text(hjust = 0.5),
        strip.background = element_rect(fill="lightblue")) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index from CityLab",
       y = "Democratic Margin of Victory/Defeat") +
  theme(text = element_text(family = 'Fira Sans'),
        legend.position = "none")
p <- ggplotly(p)

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_jitter/customized")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5795.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>

### Position Jitterdodge
Up to this point, we've subdivided points by making one category the x-axis, and facetting by the other. Another way is to make one category the x-axis, then use "position = dodge" so that the points are distinct rather than overlapping. Since we want points to be jittered and dodged, we can use geom\_point with position\_jitterdodge().

Make sure to specify the "group" variable: this graph specifies three potential grouping variables (cluster, region, cd_code), and position\_jitterdodge can't tell which two to use unless specified. Further, you can use the ggplotly() function to specify what shows up on the tooltip.


```r
library(plotly)

p <- ggplot(district_density,aes(x=cluster, y=dem_margin, colour=region, 
                                 district=cd_code, group=paste(cluster, region))) +
  geom_point(position=position_jitterdodge(), alpha=0.5) +
  geom_hline(yintercept=0) +
  theme(axis.text.x = element_text(angle = -30, hjust = 0.1),
        plot.title = element_text(hjust = 0.5)) +
  labs(title = "Democratic performance in the 2018 House elections, by region and density",
       x = "Density Index from CityLab",
       y = "Democratic Margin of Victory/Defeat") +
  theme(text = element_text(family = 'Fira Sans'))
p <- ggplotly(p, tooltip=c("district","y"))

# Create a shareable link to your chart
# Set up API credentials: https://plot.ly/r/getting-started
chart_link = api_create(p, filename="geom_jitter/jitterdodge")
chart_link
```

<iframe src="https://plot.ly/~RPlotBot/5797.embed" width="800" height="600" id="igraph" scrolling="no" seamless="seamless" frameBorder="0"> </iframe>
