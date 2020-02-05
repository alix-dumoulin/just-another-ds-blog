---
title: Hacking Disinformation with the EU
author: ~
date: '2020-02-05'
slug: hacking-disinformation-with-the-eu
categories: [Short Projects]
tags: [shiny, R, visualisation, politics]
images: [disinfo.png]
authors: [Alix Dumoulin]
---

I recently took part in the "Disinformation Ahead: Responding to Future Threats" Hackathon organised by the East StratCom Task Force. We created a nice shiny dashboard to raise awareness of disinformation.
<!--more-->

## Disinformation Ahead: Responding to Future Threats

The conference "Disinformation Ahead: Responding to Future Threats" was taking place in Brussels on 29-30th January - in parallel, the East StratCom Task Force (ESTF) invited programmers, data scientists, communications professionals, and thinkers from across the EU to help them in their task to fight disinformation spread from the Russian Federation.

The taskforce is guided by the Action Plan against Disinformation of the European External Action serive which aims to tackle
> “deliberate, large-scale, and systematic spreading of disinformation”

Learn more [here.](https://eeas.europa.eu/headquarters/headquarters-homepage/54866/action-plan-against-disinformation_en)

## The EU vs Disinfo Database

Over the last 5 years, the ESTF did an amazing job collecting, analyzing, labelling and debunking fake news articles from sources like Sputnik or Russia Today. You can explore the debunked articles on the new website: https://euvsdisinfo.eu. 

The database contains the following: 

* **Claims** made that are considered disinformation

* The **original news article** they come from

* The **keywords** from the article (e.g. weapons, daesh etc.)

* The source **organisation** of the claim

* The **language** and **country** targeted by the article

* A **reviews** of the claim by the ESTF

This data was collected and labeled by communications professionals and is of very high quality and is now available via an API, here: https://api.veedoo.io. You can call the API in R, for instance to collect all the claim reviews: 

{{< highlight r >}}
library(jsonlite)
claim_reviews <- list()

for(i in 1:241) {
  url  <- "https://api.veedoo.io"
  path1 <- paste0("claim_reviews?page=", i)
  path_full <- paste0(path1, "&perPage=30")
  raw.result <- GET(url = url, path = path_full)
  names(raw.result)
  this.raw.content <- rawToChar(raw.result$content)
  this.content <- fromJSON(this.raw.content)
  claim_reviews[[i]] <- this.content$`hydra:member`
}
{{< /highlight >}}

THe API is not yet super easy to use with quite a lot of `merge()` to do for instance to get the keywords used by an organisation. In hindsight, creating an SQLite database would have made it much easier! But the API is free and relatively easy to access so don't hesitate to use it for you own project!


## Gaining new insights

The goal behind this hackathon was for the participants to use this data to create new insights, new tools, and generally help the ESTF better understand the data and use it towards their mission of raising awareness and fighting the disinformation campaigns from Russia.


## Our solution

In a team of 3, we created a Shiny dashboard (R) which allows users to explore the database and have an overview of its content. 

![shiny-page](/images/shiny.png)

Fow now, the dashboard has 4 tabs:


1) A home tab gives some explanations and guidlines about the dashboard.

2) The first tab includes a map of countries most targeted by disinformation, ranking of most active organisations, and the number of fake claims over time. 

3) On the second tab, you have the possibility to dive into a specific organisation and see what languages they spread their news in, what topics they write about most and how many claims they made. 

4) The last tab (for now) lets you explore the claims: you can explore the most popular themes by country, explore a mapping of the claims based on word embeddings which plots the claims in two dimensions according to how similar they are - interesting to see clustering of topics and how they changed in the last few years - as well the the evolution of topic popularity over time.

You can access the shiny dashboard here: http://alixdumoulin.shinyapps.io/disinformation_hackathon/

And the Github page here: https://github.com/alix-dumoulin/eu_disinformation/

It is the result of a 2-day hackathon which means there is much more we can do, suggestions are welcome, you can contact me at: a.dumoulin@lse.ac.uk.