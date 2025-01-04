---
layout: post
title: "Study of a binge listener"
date:  2019-03-20 18:00:00 -0500
categories: [datavis]
excerpt_separator: <!--more-->
---

## Exploring the last decade of my music listening

This spring I took a General Assembly course, _Data Visualization Fundamentals_, that immerses students in advanced analytics and visualization practices. The underlying theory behind good data visualization and best practices was studied. We learned to identify and frame the right questions, then conduct and communicate analyses to answer business questions, and to tell stories with data via hands-on labs and a semester long capstone project. The extract-transform-load (ETL) cycle and exploratory data analysis (EDA) were covered along with skills like data wrangling, exploring, statistical modeling, and communicating insights using Excel, SQL, Tableau, and Python. 

This post is documentation of my capstone project – **Study of a binge listener**, _Exploring the last decade of my music listening_. My final presentation and project were well received and I scored highest marks – meaning my performance was above and beyond the requirements for the final project.

<!--more-->

I have long been fascinated with the quantified self movement and annual reports like those produced by Nicholas Felton ([https://feltron.com/info.html](https://feltron.com/info.html)). He has been producing reports in distinctive visual styles for over 10 years – see 2014 ([http://feltron.com/FAR14.html](http://feltron.com/FAR14.html)), 2013 ([http://feltron.com/FAR13.html](http://feltron.com/FAR13.html)), and 2012 ([http://feltron.com/FAR12.html](http://feltron.com/FAR12.html)) as examples.

I have close to 13-years of music listening history thanks to [last.fm](https://www.last.fm) and Apple's recent decision to make the [GDPR data and privacy web portal](https://privacy.apple.com/) available worldwide (see References, below).

There are a wide variety of questions that can be explored:

1. What times of day are most popular for listening to music?

2. Are different days of the week or months of the year more popular and why?

3. Which artists are most popular overall and which have gained or lost popularity over time?

4. Which songs have been most popular by year and overall?

5. How much time has been spent listening to music?

## Profile of a shuffling, binge listener

I am a shuffling, binge listener that has transitioned from Rock to Alternative / Adult Alternative over the 10-years of listening.

### When did I start and stop listening to artists?

![](/img/2019-03-20_DataVis_Capstone.png "When did I start and stop listening to artists graph")

This was the gem of my capstone project. The graph shows when I **first** and **last** listened to a band/artist in two different charts – a bubble chart and a line chart. The X-axis of both charts is years. The Y-axis of the both charts is the number of listens for a given artist. For the bubble chart, the size of the bubble encodes the total number of listens – a larger bubble means more lifetime listens. The color of the bubbles encodes the lifetime of listening to that artist – the number of days between the first listen to that artist and the last listen.

Bubbles can be selected and the corresponding time series line chart for that selected artist is plotted on the inlaid line chart. It is possible to select multiple bubbles and the line chart will show the corresponding lines. In the screenshot above, Bruce Springsteen, Coldplay, Barenaked Ladies, The Black Keys, Big Head Todd and the Monsters, and A Fine Frenzy are selected.



## Data cleaning and EDA

As with any data science project, a significant amount of data wrangling and scrubbing was necessary before, during, and after joining the two datasets. Including:

- Looked for duplicate date / time collisions. Obviously cannot be listening to two different things at the same time. There were only 4. Deleted the duplicates.

- Rodrigo y Gabriela's album **11:11** does not behave well as a data element. Some conversion was necessary to ensure it was interpreted as an album name (string) as opposed to a time or something else.

- Several videos snuck through. They are not songs, so they were deleted.

- Grizmatik: the artist should be Gramatik and the album is Grizmatik. Meta data corrected. Probably a remnant of file sharing or poor tagging on my part when importing music from other sources.

- Blank album – For some reason, I do not think the Apple Music data had album information. I used an INDEX / MATCH and a sorted dataset to fill in blank / missing albums. This could have been done in Python or SQL but was done in Excel in the interest of time.

### Apple Music

One of the major challenges encountered was the volume of records in Apple Music and the realization that the records are not "_plays_" or "_listens_" but **interactions with the service** (play, pause, skip, scrub, display lyrics, view album, view artist info, etc.). With some research and digging, I found Pat Murray's Github repo for his Apple Music visualization tool and he walks through the computations in his Javascript code to filter records to just plays. Specifically, Pat's [Computation.js (https://github.com/PatMurrayDEV/apple-music-history/blob/master/src/components/Computation.js)](https://github.com/PatMurrayDEV/apple-music-history/blob/master/src/components/Computation.js) was immensely helpful to my project. 

### Source: last.fm, late 2006 through mid 2016, (n= 81,461 x 6 columns)

- 3,609 days | 9 years, 10 months, 17 days | 515 weeks, 4 days | 9.88 years

- Two user accounts that I combined

- Data downloaded via someone's web app that takes your last.fm data and exports it to a CSV file

- Data scrubbing: two accounts, some overlap; ~5k records with corrupt date; 11:11 album, 5-0 song, etc.; blank albums, no genre

### Source: Apple Music, mid 2015 to current (n=41,562 x 24 columns)

- 1,281 days | 3 years, 6 months, 1 day | 42 months, 1 day | 183 weeks | 3.51 years

- Every interaction with the service, not just listens (play, pause, skip, scrub, view lyrics, etc.)

- Data downloaded via request at privacy.apple.com (thanks GDPR!)

### Final Data: JOIN last.fm to Apple Music on Date

Seems easy but...

- `2015-07-10 18:19` for last.fm

- `2015-07-10T18:19:29.218Z` for Apple Music

Final DataFrame for Tableau: **n = 116,350 x 28 columns**


## Lessons Learned

- I am a shuffling, binge listener that has transitioned from Rock to Alternative ove time.

- EDA, EDA, EDA there's always data cleaning, merging, and understanding to be done. And it will always take longer than anything else in a project.

- Python can plot and create decent data visualization graphics but there's a lot of documentation and code to figure out to get what you want

- Tableau is an incredibly POWERFUL tool...like Excel, if you know it really well, it is way more than just a spreadsheet.


## Next steps

If I had a time-turner, I would take this all further	:

- Learn how to use Tableau's _Story_ feature.

- Attempt to build a few of the more advanced visualizations that I have seen.

- Recreate the Genre graphs in Tableau.

- Add Spotify Top 200 or Billboard 100 data to see how my listening overlaps.

- NLP analysis on the lyrics.


## References

1. [Pat Murrayʼs Apple Music History](https://github.com/PatMurrayDEV/apple-music-history) – a project that was an inspiration for my project.

2. [LastFM to CSV](https://benjaminbenben.com/lastfm-to-csv/) – will convert your last.fm profileʼs listening data to a CSV file.

3. [Apple Data and Privacy web portal](https://privacy.apple.com/) – source for obtaining your Apple Music listening data.

4. [Fetlron](http://feltron.com/) – quality annual reports that were an inspiration for this project.

5. [When did I start and stop listening to bands?](https://public.tableau.com/app/profile/bridget/viz/TableauFitVizGames2AndyCotgreavelastfm2015/Alt1) – I'm pretty sure that Bridget Winds Cogley's alternatives to Andy Cotgreave's visualization were the inspiration for my favorite graph in my capstone project. See [Music listening trends: 2015. The Death of the album?](https://gravyanecdote.com/tableau/music-listening-trends-2015-the-death-of-the-album/) by Andy Cotgreave for the original. 
