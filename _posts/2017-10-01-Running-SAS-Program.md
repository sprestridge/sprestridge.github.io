---
layout: post
title: "Coursework: Running a SAS Program"
date:   2017-10-01 19:16:00 -0500
tags: training
---

I enrolled in a Coursera course titled '**Data Science + Analytics**'. The second lesson, _Software Setup and Supporting Materials_ requires a blog post for running your first program, so this is my post. See below for the program and output.

## Analysis of results

A random sample of 2,294 adults over the age of 18 were asked the following question, "_On a seven-point scale, do you think that Blacks and other minorities are treated the same as Whites in the criminal justice system (1) or do not receive equal treatment (7)_?" The results are shown in the first table of the Output section below. Of the total number, about 35% chose value 7 which indicates they believe minorities do not receive equal treatment. Overall 48.7% believe strongly that minorities do not recieve equal treatment (values 6 & 7). Just 12.3% believe minorities receive equal treatment (values 1 and 2). Only 2.4% refused to answer (-1).

For the next question, the same sample of adults were asked "_What is your religion_?" The results are displayed in the second table. Just 12.8% answered none or that they did not have one (13) with the majority of 28.9% answering Baptist of any denomination (1). Protestant's (2) came in at 16.8% while Catholic (3) was 14.4% of responses. Other Christian (11) was the other significant response at 15.4%. Only 19 individuals (0.8%) refused to answer (-1).

For the third question, "_On a scale of 1 (strongly agree) to 4 (strongly disagree), should churches or places of worship be involved in political matters_?" The results are displayed in the final table below. The majority of 38.5% believe strongly that churches or places of worship should not be involved in political matters while just 8.1% strongly agree that churches and places of workship should be involved in political matters.

## SAS Program

The code for the program I used to produce the output below follows:

```
LIBNAME mydata "/courses/d1406ae5ba27fe300 " access=readonly;
DATA new; set mydata.oll_pds;
LABEL   W1_K4="Blacks treated same as whites? | 1=Equal treatment | 7=Do not receive equal treatment | -1=No Answer | 9=Not sure"
        W1_M1="Religion? | 1=Baptist | 2=Protestant | 3=Catholic | 4=Mormon | 5=Jewish | 6=Muslim | 13=None"
        W1_M3="Places of worship involved in politics? | 1=Strongly Agree | 4=Strongly Disagree";
PROC SORT; by CASEID;
PROC FREQ; TABLES W1_K4 W1_M1 W1_M3;
RUN;
```

## Output

![SAS Output](/img/2017-10-01 SAS Output.png)

<cite>SAS Program Output, Source: S. Prestridge</cite>
