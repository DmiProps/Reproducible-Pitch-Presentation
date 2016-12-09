---
title: "Mtcars k-means clustering"
author: "Dmitriy Yakovlev"
date: "09-Dec-2016"
output:
  html_document:
    number_sections: yes
    toc: yes
job: 09-Dec-2016
knit: slidify::knit2slides
mode: selfcontained
subtitle: different variables of Mtcars & clustering analisys
framework: io2012
widgets: bootstrap
---



## Introduction

This assignment has two parts:

1. First, you will create a Shiny application and deploy it on Rstudio's servers.
2. Second, you will use Slidify or Rstudio Presenter to prepare a reproducible pitch presentation about your application.

## Data


```r
library(datasets)
head(mtcars, 3)
```

```
##                mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4     21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710    22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
```

---

## ui.R


```r
titlePanel("Mtcars k-means clustering"),

sidebarLayout(
  sidebarPanel(
      selectInput('xcol', 'X Variable', names(mtcars)),
      selectInput('ycol', 'Y Variable', names(mtcars),
                  selected=names(mtcars)[[2]]),
      numericInput('clusters', 'Cluster count', 3,
                   min = 1, max = 9)
  ),
  mainPanel(
      plotOutput('plot1')
  )
)
```

---

## server.R


```r
selectedData <- reactive({mtcars[, c(input$xcol, input$ycol)]})

clusters <- reactive({
    kmeans(selectedData(), input$clusters)
})

output$plot1 <- renderPlot({
    palette(c("#E41A1C", "#377EB8", "#4DAF4A", "#984EA3",
              "#FF7F00", "#FFFF33", "#A65628", "#F781BF", "#999999"))

    par(mar = c(5.1, 4.1, 0, 1))
    plot(selectedData(),
         col = clusters()$cluster,
         pch = 20, cex = 3)
    points(clusters()$centers, pch = 4, cex = 4, lwd = 4)
})
```

---

## Example plotting

![Title](https://github.com/DmiProps/ReproduciblePitchPresentation/blob/master/img1.png?raw=true)
