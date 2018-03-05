---
layout: post
title:  "Data Analysis and Interpretation: Moderation"
date:   2018-03-04 19:16:00 -0500
categories: training
---

Previous [ANOVA analysis](/training/2018/02/11/ANOVA.html) of the '_Outlook on Life_' survey data on the question of trust in the legal system (**W1_K1_C** - how much do you think you can trust the legal system) revealed statistical significance for age (**PPAGECT4**) and education level (**PPEDUCAT**). See the very bottom of this post for a summary of the variables. Duncan post hoc comparisons revealed that younger people (18-29) are statistically more likely to distrust the legal system than the other age groups. Post hoc comparisons also showed statistical significance for education levels with those having more education slightly more likely to trust the legal system. 

Now, the interaction of a third variable is considered to determine if there is moderation of the relationships by the third variable. The third variable is investigated to see if it affects the direction and or strength of the relation between the explanatory (**W1_K1_C**) and response (**PPAGECT4** and **PPEDUCAT**)variables. First, I examined whether education affected the direction or strength of the relationship between trust in the legal system and age. Second, I examined whether age affected the direction or strength of the relationship between trust in the legal system and education.

The first analysis looks at whether trust in the legal system and age are associated across all education levels. The second analysis examines whether trust in the legal system and education are associated across all age groups.

Full results are available as a [PDF](/files/Week_4_Moderation_Results.pdf).

## Does education moderate the relationship between trust in the legal system and age?

For the 18-29 age group, trust in the legal system is significant, F (3, 203) = 3.49, p = 0.0167, with the mean of 3.17 showing higher levels of mistrust than the other age groups. 

For the 30-44 age group, trust in the legal system is also significant, F (3, 674) = 6.38, p = 0.0003, the mean of 2.92 is similar to the 18-29 age group's mean of 2.93 at this education level and significantly higher than the means for the remaining age groups.

For the 45-59 age group, trust in the legal system is not significant (p = 0.495).

Finally, for the 60+ age group, trust in the legal system is signifcant, F (3, 677) = 2.66, p = 0.0473.

**So overall, it appears that education does moderate the relationship between trust in the legal system and age.**

## Does age moderate the relationship between trust in the legal system and education?

_Note: Write up of this analysis to be edited / completed at a later time. I think the first analysis is enough to earn a passing grade on the assignment._

For the 18-29 age group, trust in the legal system by education level is significant, F (1, 376) = 16.86, p < 0.0001. The mean of 2.98 for those with high school or lower levels of education is significantly higher than the 2.66 for those with some college or higher levels indicating _higher levels of mistrust for the legal system among young people with high school or lower levels of education._

For the next age group, 30-44 yrs old, is also significant, F (1, 469) = 18.95, p < 0.001. Here again the mean for lower levels of education is higher than for higher levels of education, 2.93 vs. 2.63, indicating _higher levels of mistrust for the legal system among this age cohort for those with high school or lower levels of education._

None of the other age groups showed significance.

**So overall, it appears that age does moderate the relationship between trust in the legal system and education levels.**

## SAS Program Code

Full code of my SAS program is shown below.

``` SAS
LIBNAME mydata "/courses/d1406ae5ba27fe300 " access=readonly;
DATA new; set mydata.oll_pds;

LABEL   W1_K1_C="Do You trust the legal system?"
        PPEDUCAT="Education Level"
        PPAGECT4="Age";

/*set missing data*/
IF W1_K1_C=-1 THEN W1_K1_C=.;

IF PPEDUCAT LE 2 THEN PPEDUCATgroup=1;
ELSE IF PPEDUCAT GE 3 THEN PPEDUCATgroup=2;
 
/* W1K1C variable 
    1=TRUST 
    2=MISTRUST */
if W1_K1_C=1 or W1_K1_C=2 then W1K1C=1;
else if W1_K1_C=-1 then W1K1C=.; /*set missing data*/
else W1K1C=2;


/* add formatting for W1_K1_C - Trust Legal System */
PROC FORMAT;
    value W1_K1_C_groups
    1='Trust'
    4='Mistrust';

/* formatting for PPEDUCAT - Education Level */
PROC FORMAT;
    value PPEDUCAT_groups
    1-2='High School or lower'
    3-high='Some college or higher';

/* formatting for PPAGECT4 - Age */
PROC FORMAT;
    value PPAGECT4_groups
    1='18-29'
    2='30-44'
    3='45-59'
    4='60+';

PROC FREQ; TABLES W1_K1_C PPEDUCAT PPAGECT4;
    Format W1_K1_C W1_K1_C_groups.;
    Format PPEDUCAT PPEDUCAT_groups.;
    Format PPAGECT4 PPAGECT4_groups.;

/* general form for anova moderation w/ thirdvar
PROC SORT; BY cat_thirdvar;

PROC ANOVA; CLASS cat_explanatory;
MODEL quan_response=cat_explanatory;
MEANS cat_explanatory; BY cat_thirdvar;
*/

/* look at trust in the legal systeam - W1_K1_C */
PROC SORT; BY PPEDUCAT;

PROC ANOVA; CLASS PPAGECT4;
MODEL W1_K1_C=PPAGECT4;
MEANS PPAGECT4; BY PPEDUCAT;
    Format W1_K1_C W1_K1_C_groups.;
    Format PPAGECT4 PPAGECT4_groups.;

PROC SORT; BY PPAGECT4;

PROC ANOVA; CLASS PPEDUCAT;
MODEL W1_K1_C=PPEDUCAT;
MEANS PPEDUCAT; BY PPAGECT4;
    Format W1_K1_C W1_K1_C_groups.;
    Format PPEDUCAT PPEDUCAT_groups.;

RUN;
```


## Summary of Outlook on Life data in this analysis

**W1_K1_C - How much do you think you can trust the legal system?**
- 1 = just about always
- 2 = most of the time
- 3 = some of the time
- 4 = never
- -1 = refused 

Based upon 2,230 valid cases out of 2,294 total cases.

**PPAGECT4**
- 1 = 18-29
- 2 = 30-44
- 3 = 45-59
- 4 = 60+

**PPEDUCAT**
- 1 = Less than high school
- 2 = High school
- 3 = Some college
- 4 = Bachelor or higher
