---
layout: post
title:  "Regex - Don't Fear It"
date:   2019-01-31 09:16:00 -0500
categories: [data, movies]
---

Bottom line up front: _I've seen 55% of the top movies from 2000 - 2018_.

I seem to be running into posts about text munging or data scrubbing a lot recently. First it was Jason Snell's [recent article](https://sixcolors.com/post/2019/01/using-bbedit-and-excel-to-revive-a-dead-podcast-feed/) on reviving an old podcast feed in which he describes using the power of Grep pattern-matching, BBEdit, and Excel to reconstruct a podcast RSS feed. Then I saw the good doctor's post - '[Don't fear the regex](https://leancrew.com/all-this/2019/01/dont-fear-the-regex/)' - in which Dr. Drang discusses regular expressions, their power, and the great _"Searching with Grep"_ chapter in the [BBEdit User Manual](https://s3.amazonaws.com/BBSW-download/BBEdit_12.5.2_User_Manual.pdf#page182). Special hat tip to the doctor for the Blue Oyster Cult [reference](https://itunes.apple.com/us/album/dont-fear-the-reaper/217555955?i=217556132).

Thanks to the BBEdit User Manual, regular expressions / Grep, and [Sublime Text](https://www.sublimetext.com) I was able to rapidly extract the text I needed from a recent post to determine my personal 'seen it rate' for top movies - 55%.

In my ['Movies I haven't seen' post](movies/2019/01/11/popular-movies-i-havent-seen.html), I went through the 2000 - 2018 top movies and identified those I had seen and those I had not. Just below each year's heading I had a summary that took a familiar format:

(**9 seen** out of 14)

for example. I knew I could use Sublime and the power of regular expressions to search the whole post and select every line at once that matched that format and copy it to another text file or Excel for further processing. My ultimate goal was to sum the column of numbers for _'seen it'_ and _'out of'_ to calculate the overall _'seen it'_ percentage (total movies seen over total movies).

Two challenges I encountered were that the raw text was Markdown so I had to deal with the double asterisks (e.g. \*\*9 seen\*\*) used for **bold text** by Markdown and some numbers were a single digit (9 for example) but others were two digits (14 for example).

The regular expression I used in Sublime:

`\(\*\*\d+ seen\*\* out of \d+\)`

did the trick. The first backslash is an escape so that I can search for the opening parens `(`, likewise asterisks need to be escaped as well because they are special characters in Grep. The next trick involves matching the numbers and since I could have a single or double digit number the solution is to match one or more digits using the `+` symbol which is also a special character. Long story short, I ran that regular expression in Sublime Text with a `Find All` and regular expressions, copied the results, and pasted them into a new text file. I did a bit more clean-up and used the final results in Excel for a quick `Text-to-Columns`, sum column manipulation, and then I had my answer. I've seen 55% of the top movies from 2000 - 2018.