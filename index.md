---
title       : The Giving History Tool
subtitle    : Making Giving History Reports Easy(er)
author      : Kenneth Rebella  
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---
## What is it?
When compiling federal giving history reports, it can take upwards of 20 minutes for a finance assistant to organize and calculate a donor's contributions.   

This app takes care of that. After downloading a donor's federal giving history from the internet and saving it as a .csv, the Giving History tool does all the calculation for you in seconds. 

## Why do we need an App for this?

Excel doesn't easily have capability to do this, and most people don't know how to use R. This app is easily accessible, easy to use, and fast!  

--- .class #id 

## How does it work?
It runs some pretty straight forward R code, which pulls the needed information, sums the contributions by recipient and year, and finally orders the information in descending order by year and then condtribution level.  

```r
data <- read.csv(inFile$datapath, header=input$header, sep=input$sep, 
                                 quote=input$quote)
                candidate <- as.character(data$recipient_name)
                date <- as.character(data$date)
                date <- as.Date(date, format= "%m/%d/%y")
                year <- (as.POSIXlt(date)$year + 1900)
                amount <- data$amount
                report <- data.frame(year, amount, candidate)
                years <- unique(year)
                d <- data.frame()
```

--- .class #id 

## R Code Cont.

```r
                for (i in 1:length(years)) {
                        report2 <- report[(report$year == years[i]),]
                        test <- aggregate(report2$amount, by= list(
                                candidate = report2$candidate), FUN = sum)
                        final <- cbind(rep(years[i],nrow(test)), test)
                        colnames(final)<- c("Year", "Candidate", "Amount")
                        d <- rbind(d, final)
                        d<- data.frame(d$Year,d$Amount, d$Candidate)
                        colnames(d) <- c("Year", "Amount", "Candidate")
                }
                d[(order(d$Year, d$Amount, decreasing = TRUE)),]
```

This will return a data frame sorted by year and giving amount. There is the option for the user to then download the resulting information. 

---

## To Recap

1. This app will save staff a lot of time 
2. It is very easy to use. 
3. It is accessible 


