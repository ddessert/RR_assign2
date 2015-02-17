---
title: "Unhealthy Weather Events"
author: "David Dessert"
date: "Tuesday, February 17, 2015"
output: html_document
keep_md: true
---

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

## Load necessary packages to execute
```r
library(ggplot2)
library(plyr)
```

## R Software environment
```r
sessionInfo()
```

## Download the source file
```r
inFile <- "./data/repdata_data_StormData.csv.bz2"
download.file("https://d396qusza40orc.cloudfront.net/repdata_data_StormData.csv.bz2", 
              inFile)
```

## Load and preprocessing the data
The input data file is stored in the ./data subdirectory and
is in a comma-separated variable format (.csv extension) within 
a zipped file (.bz2 extension). Extracting and loading takes a while.
```r
data <- read.csv(bzfile(inFile))
df <- data[,c("EVTYPE", "FATALITIES", "INJURIES", "PROPDMG", "PROPDMGEXP")]
summary(df)
```
## Combine EVTYPE's that are identical (ignore case difference)
```r
levels(df$EVTYPE) <- toupper(levels(df$EVTYPE))
```

## Isolate important EVTYPE's by non-zero FATALITIES, INJURIES, or PROPDMG
```r
ddply(df, .(EVTYPE), summarize, imp_events = (FATALITIES>0) || (INJURIES>0) || (PROPDMG>0))
```

You can also embed plots, for example:

```{r, echo=FALSE}
plot(cars)
```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
