---
layout: post
title: "2026 Monte Carlo Final Four"
date: 2026-05-22 10:00:00 -0500
categories: [data science, python, lacrosse]
excerpt_separator: <!--more-->

---
### Predicting the championship from the final four field

I am revisiting last year's Monte Carlo posts on predicting the outcomes of lacrosse games ([here]({{ site.baseurl }}{% link _posts/2025-05-18-Monte-Carlo-Lacrosse.md %}) and [here]({{ site.baseurl }}{% link _posts/2025-05-23-Monte-Carlo-Redux.md %})). Heading into Memorial Day weekend 2026, the [UNC Women's Lacrosse](https://goheels.com/sports/womens-lacrosse) team has to be feeling confident. Defending champions and favored to win with 5 in 10 chances to take this year's championship. 

On the Men's side, Notre Dame has 4 in 10 chances to win the championship with Princeton having 2.8 chances in 10.

Read on to learn more.

<!--more-->

Ratings are sourced from [LaxPower2](https://laxmath.com/laxpower/wom/list_prr001.php) and are [based on a margin-of-victory](https://laxmath.com/laxpower/wom/ex_pr.php) calculation. The difference between two ratings is the number of goals by which the higher rated team is expected to win. Monte Carlo models are used to generate the odds of each of the four teams advancing and winning the championship. If 1,000 tournaments were played, UNC would win 502 championships, Northwestern 270, Maryland 124, and [JHU](https://hopkinssports.com/sports/womens-lacrosse/) 104 championships.

**Women's D1**

| Team         | Record | Rating   | AGD  | GF  | GA  | Odds  |
|:-------------|-------:|---------:|-----:|----:|----:|------:|
| UNC          | 18-1   | 99.9000  |10.31 | 341 | 145 | 50.2% |
| Northwestern | 17-3   | 98.2876  | 5.90 | 295 | 177 | 27.0% |
| Maryland     | 18-3   | 97.2975  | 3.14 | 272 | 206 | 12.4% |
| JHU          | 17-4   | 96.8323  | 4.90 | 319 | 216 | 10.4% |

In the semifinal against [Maryland](https://umterps.com/sports/womens-lacrosse), UNC is expected to win **724 games out of 1,000** and if [Northwestern](https://nusports.com/sports/womens-lacrosse) were to advance to the final, UNC would be expected to win the championship game over Northwestern **659 times out of 1,000**.

**Women's D3**

Because we have friends that have a daughter playing for a D3 program, I ran the numbers for that bracket as well.

| Team         | Record | Rating   | AGD  | GF  | GA  | Odds  |
|:-------------|-------:|---------:|-----:|----:|----:|------:|
| Middlebury   | 21-0   | 99.9000  | 9.47 | 303 | 104 | 38.0% |
| Wesleyan     | 18-3   | 98.2876  | 5.66 | 245 | 126 | 25.7% |
| Tufts        | 17-3   | 97.2975  | 7.75 | 313 | 158 | 19.6% |
| Salisbury    | 20-0   | 96.8323  | 7.60 | 295 | 143 | 16.8% |

**Middlebury** is expected to win the championship **380 times out of 1,000** tournaments. In the semifinal against Tufts, Middlebury is expected to win 606 times out of 1,000 games; Middlebury would be expected to win the championship over Wesleyan **612 times out of 1,000 finals**. 

Wesleyan has a loss and a win with Tufts and two losses to Middlebury on their record.

Tufts has previously lost to Middlebury and Wesleyan but also has a win over Wesleyan.

Salisbury has the worst odds, winning just **168 championships out of 1,000** simulated tournaments. Salisbury is expected to get past Wesleyan just 243 times out of 1,000 and could defeat Middlebury in a championship game 360 times.

**Men's D1**

| Team         | Record | Rating   | AGD  | GF  | GA  | Odds  |
|:-------------|-------:|---------:|-----:|----:|----:|------:|
| Notre Dame   | 12-2   | 99.9000  | 4.71 | 186 | 120 | 40.2% |
| Princeton    | 15-2   | 99.2868  | 4.94 | 252 | 168 | 27.5% |
| Duke         | 11-4   | 98.8380  | 6.00 | 221 | 131 | 18.9% |
| Syracuse     | 13-5   | 98.3347  | 2.44 | 235 | 191 | 13.4% |

Princeton faces Duke in the first semifinal and Notre Dame faces Syracuse in the second. According to the model, **Notre Dame should win 402 of 1,000 championship** weekends. Syracuse has the longest odds, winning just 134 of 1,000 championship weekends. Head-to-head, Notre Dame should defeat Syracuse 662 of 1,000 semifinals. In a championship against Princeton, Notre Dame is expected to be victorious 665 of 1,000 times. 

For posterity, the code used for the Men's final four:

```zsh
% python3.10 mc_final_four.py 100000 24.5 99.90 98.3347 99.2868 98.8380
Running 100000 tournament simulations...

Tournament Win Probabilities:
Team  1: 40.18% - Notre Dame
Team  2: 13.38% - Syracuse
Team  3: 27.53% - Princeton
Team  4: 18.91% - Duke
```

