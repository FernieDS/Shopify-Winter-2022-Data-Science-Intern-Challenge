---
title: "Shopify Winter 2022 Data Science Intern Challenge"
author: "Fernando Crupi"
date: "15/9/2021"
output: html_document
---

### Question 1
Given some sample data, write a program to answer the following: [click here to access the required data set](https://docs.google.com/spreadsheets/d/16i38oonuX1y1g7C_UAmiK9GkY7cS-64DfiDMNiR41LM/edit#gid=0)

#### Introduction
On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 
What metric would you report for this dataset?
What is its value?

To answer these questions we first load the data in R.
```{r setup, include=FALSE}

library(depmixS4)
library(tidyverse)
library(readxl)

knitr::opts_chunk$set(echo = TRUE, warning=FALSE, message=FALSE, fig.align = 'center',out.width = "75%")
```

```{r question1, echo=TRUE, eval = TRUE}
data = read.csv('2019 Winter Data Science Intern Challenge Data Set - Sheet1.csv', header = TRUE)

# To avoid naming each variable we use attach
attach(data)
summary(order_amount)
```
From this summary of order amount, we can see that the naive measure that was calculated was just a mean of the total order amounts. 

In particular, we can find more insights on why the average order value is so high, if we calculate an average order value per shop.
```{r groups}
shop_aov <- data %>%
        group_by(shop_id) %>%
        summarize(aov_per_shop = mean(order_amount)) %>%
        arrange(desc(aov_per_shop))

head(shop_aov, 10)

ggplot(shop_aov) +
  aes(x = "", y = shop_aov$aov_per_shop) +
  geom_boxplot() +
  coord_trans(y = "log10") +
  scale_y_continuous(breaks=c(50,100, 150, 200, 350, 500, 20000)) +
  ylab("AOV per shop") +
  xlab("") +
  theme_bw()

```
