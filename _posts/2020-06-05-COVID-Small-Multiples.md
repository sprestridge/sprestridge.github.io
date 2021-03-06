---
layout: post
title:  "Producing Small Multiples of COVID-19 case rates by state"
date:   2020-06-05 17:00:00 -0500
categories: [datavis, python, COVID-19]
excerpt_separator: <!--more-->
---

Inspired by my love for small multiples, Horace Dediu's [recent work](http://www.asymco.com/wp-content/uploads/2020/05/Screen-Shot-2020-05-11-at-7.59.26-AM.jpg) on Asymco, and the pandemic the world is experiencing I decided to put my Python skills to test and produce a small multiples plot of case rates for the United States.

<!--more-->

I'm pleased with the result but already see things I would like to incorprate for v2.0.

If you'd like to use the script yourself I have made it available [online](https://gist.github.com/sprestridge/01a63f45db28854db39c6420f0179be1). The plots are all COVID-19 case rates starting once a state reaches 30 cases/day. All data is 7-day average and the source is the [NY Times COVID data from GitHub](https://github.com/nytimes/covid-19-data).

What trends do you see?

![](/img/2020-06-04-covid-case-rate-small-multiples-US.png "Small Multiples - COVID-19 U.S. case rates by state")
