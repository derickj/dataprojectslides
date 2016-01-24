---
title       : "Developing Data Products - Peer Assessment Project"
subtitle    : "Visualising Enrollments by Race in South African Universities"
author      : derickj
job         : Coursera student by night, project leader by day
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
--- .class #id 

## Data Used



- Source of data:  Center for Higher Education Transformation
- South African Higher Education Open Data Set
- Available at: http://chet.org.za/data/sahe-open-data
- Data was transformed to CSV and hosted on google docs for the app to read it directly from global.R

```r
url <- "https://docs.google.com/spreadsheets/d/14EbD832LcfKEI2y4QIEJq8mvmkiVtFWNNPF3AEJap0Q/pub?output=csv"
urlData <- getURLContent(url)
cname <- read.table (textConnection(urlData), sep = ',', skip = 7, 
                     nrow = 1, stringsAsFactors = FALSE)
```


--- .class #id 

## User Interface

- ui.R provides selectors for two types of graphs:
- A year (selected from a drop-down) and one or more university names (abbreviated) for a comparison between those universities for a given year
- A single university name selected from a drop-down to view the enrollment trends over the period of the dataset
- The ui creates a tabset panel with the plots on one and instructions about the app on another 
- Plots are generated in the server app and displayed as soon as data selections are changed
- github repo: https://github.com/derickj/dataprojects/tree/gh-pages

--- .class #id 

## Server App

- The server app uses the selected data to subset the data, .e.g

```r
inputunis <- c("UP","UJ","WSU")
inputyear <- "2012"
thedata <- unidata[unidata$univ %in% inputunis,]
thedata <- thedata[thedata$year == inputyear,]
```
- Rcharts  library is used to generate the charts as illustrated on next slide

- The app is hosted at: https://derickj.shinyapps.io/dataprojects/

--- .class #id 

## Generating Plots

- Example code for comparative chart

```r
library (knitr)
library (rCharts)
library (markdown)
options(RCHART_LIB = 'polycharts')
r1 <- rPlot(perc ~ univ, color = 'race', data = thedata, type = 'bar')
r1$guides(x = list(title = "Universities"))
r1$guides(y = list(title = "Percentage enrollment"))
r1$addParams(height = 300, 
               dom = 'chart1', 
               title = paste("Percentage of Enrollment By Race Group",inputyear))
r1
```

```
## <iframe src=' assets/fig/chart-1.html ' scrolling='no' frameBorder='0' seamless class='rChart polycharts ' id=iframe- chart1 ></iframe> <style>iframe.rChart{ width: 100%; height: 400px;}</style>
```
