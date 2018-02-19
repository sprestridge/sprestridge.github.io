---
layout: post
title:  "Data Analysis and Interpretation: Chi-Squared Analysis"
date:   2018-02-18 19:16:00 -0500
categories: training
---

I am using the 'Outlook on Life' survey data to investigate if age or education play a role in whether people think blacks and other minorities are treated the same as whites in the criminal justice system (W1_K4 in the codebook) or in whether they have trust in the legal system (W1_K1_C in the codebook).

## Do you think that blacks and other minorities are treated the same in the criminal justice system?

Our p-value of less than 0.0001 clearly tells us that belief in whether or not blacks and other minorities are treated the same in the criminal justice system is associated with age. So we accept the alternate hypothesis - that not all age categories are equal across belief categories for whether blacks and other minorities are treated the same. We know all are not equal but we do not know which are different and which are not.

The p-value of 0.0105 also tells us that belief in whether or not blacks and other minorities are treated the same in the criminal justice system is associated with education levels as well. So we accept the alternate hypothesis - that not all education categories are equal across belief categories for whether blacks and other minorities are treated the same. We know all are not equal but we do not know which are different and which are not.

Adjusted Bonferroni p-value = p / c = 0.05 / 18 = 0.003

In order to protect against type 1 error in the context of the Chi-Square test it is appropriate to use the post hoc approach known as the Bonferroni Adjustment. Post hoc comparisons of age categories revealed that there were statistical differences between groups aged 18-29yrs and 45-59yrs (1-3) and 18-29yrs and 60+yrs (1-4) as well as 30-44yrs and 60+yrs (2-4).

Post hoc comparisons of education levels revealed that there is a statistical difference between those with high school or lower levels of education and those with college or greater levels of education.

Post hoc comparisons of rates of nicotine dependence by pairs of cigarettes per day categories revealed that higher rates of nicotine dependence were seen among those smoking more cigarettes, up to 11 to 15 cigarettes per day. In comparison, prevalence of nicotine dependence was statistically similar among those groups smoking 10 to 15, 16 to 20, and > 20 cigarettes per day.

## Do you trust the legal system?

The p-value of less than 0.0001 clearly tells us that trust in the legal system is associated with both age and education levels. So we accept the alternate hypothesis - that not all education and age categories are equal across trust in the legal system. We know all are not equal but we do not know which are different and which are not.

Adjusted Bonferroni p-value = p / c = 0.05 / 9 = 0.006

Post hoc comparisons of age categories revealed that there were statistical differences between groups aged 18-29yrs and 45-59yrs (1-3) and 18-29yrs and 60+yrs (1-4) as well as 30-44yrs and 60+yrs (2-4).

Post hoc comparisons of education levels revealed that there is a statistical difference between those with high school or lower levels of education and those with college or greater levels of education.

Full results available as a [PDF](/files/Week_2_Chi-Squared_Results.pdf) and the Bonferroni adjusted post hoc results [here](/files/Week_2_Chi-Squared_Bonferroni_Results.pdf).

## SAS Program Code

Full code of my SAS program is shown below.

``` SAS

LIBNAME mydata "/courses/d1406ae5ba27fe300 " access=readonly;
DATA new; set mydata.oll_pds;

LABEL   W1_K4="Do you think that blacks & other minorities are treated the same as whites in the criminal justice system?"
        W1_K1_C="Do You trust the legal system?"
        PPEDUCAT="Education Level"
        PPAGECT4="Age";

/*set missing data*/
IF W1_K4=-1 THEN W1_K4=.; /* treated equal */
IF W1_K4=9 THEN W1_K4=.; /* if they don't know, also blank */
IF W1_K1_C=-1 THEN W1_K1_C=.;

PROC SORT; by CASEID;

/* add formatting for W1_K1_C - Trust Legal System */
PROC FORMAT;
    value W1_K1_C_groups
    1-2='Trust'
    3-high='Mistrust';

/* formatting for W1_K4 - Treated Same as Whites table */
PROC FORMAT;
    value W1_K4_groups
    low-1='Receive equal treatment'
    7='Do not receive equal treatment'
    9-high='Not sure';

/* formatting for PPEDUCAT - Education Level - explanatory variable */
PROC FORMAT;
    value PPEDUCAT_groups
    1-2='High School or lower'
    3-high='Some college or higher';

/* formatting for PPAGECT4 - Age - explanatory variable */
PROC FORMAT;
    value PPAGECT4_groups
    1='18-29'
    2='30-44'
    3='45-59'
    4='60+';


/* general form for Chi-squared
 *
 * PROC FREQ; TABLES Categorical_Response_Variable * Categorical_Explanatory_Variable/CHISQ;
 * 
*/
/* look at blacks & minorities treated same - W1_K4 */
/* PROC FREQ; TABLES W1_K4*PPAGECT4/CHISQ; */

DATA COMPARISON1; SET NEW;
IF PPAGECT4=1 OR PPAGECT4=2;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPAGECT4/CHISQ;
RUN;

DATA COMPARISON2; SET NEW;
IF PPAGECT4=1 OR PPAGECT4=3;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPAGECT4/CHISQ;
RUN;

DATA COMPARISON3; SET NEW;
IF PPAGECT4=1 OR PPAGECT4=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPAGECT4/CHISQ;
RUN;

DATA COMPARISON4; SET NEW;
IF PPAGECT4=2 OR PPAGECT4=3;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPAGECT4/CHISQ;
RUN;

DATA COMPARISON5; SET NEW;
IF PPAGECT4=2 OR PPAGECT4=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPAGECT4/CHISQ;
RUN;

DATA COMPARISON6; SET NEW;
IF PPAGECT4=3 OR PPAGECT4=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPAGECT4/CHISQ;
RUN;


/* PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ; */

DATA COMPARISON7; SET NEW;
IF PPEDUCAT=1 OR PPEDUCAT=2;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;

PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;
DATA COMPARISON8; SET NEW;
IF PPEDUCAT=1 OR PPEDUCAT=3;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;

PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;
DATA COMPARISON9; SET NEW;
IF PPEDUCAT=1 OR PPEDUCAT=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;

PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;
DATA COMPARISON10; SET NEW;
IF PPEDUCAT=2 OR PPEDUCAT=3;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;

PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;
DATA COMPARISON11; SET NEW;
IF PPEDUCAT=2 OR PPEDUCAT=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;

PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;
DATA COMPARISON12; SET NEW;
IF PPEDUCAT=3 OR PPEDUCAT=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K4*PPEDUCAT/CHISQ;

/* look at trust in the legal systeam - W1_K1_C */
/* PROC FREQ; TABLES W1_K1_C*PPAGECT4/CHISQ; */

DATA COMPARISON13; SET NEW;
IF PPAGECT4=1 OR PPAGECT=2;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPAGECT4/CHISQ;

DATA COMPARISON14; SET NEW;
IF PPAGECT4=1 OR PPAGECT=3;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPAGECT4/CHISQ;

DATA COMPARISON15; SET NEW;
IF PPAGECT4=1 OR PPAGECT=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPAGECT4/CHISQ;

DATA COMPARISON16; SET NEW;
IF PPAGECT4=2 OR PPAGECT=3;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPAGECT4/CHISQ;

DATA COMPARISON17; SET NEW;
IF PPAGECT4=2 OR PPAGECT=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPAGECT4/CHISQ;

DATA COMPARISON18; SET NEW;
IF PPAGECT4=3 OR PPAGECT=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPAGECT4/CHISQ;

/* PROC FREQ; TABLES W1_K1_C*PPEDUCAT/CHISQ; */
DATA COMPARISON19; SET NEW;
IF PPEDUCAT=1 OR PPEDUCAT=2;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPEDUCAT/CHISQ;

DATA COMPARISON20; SET NEW;
IF PPEDUCAT=1 OR PPEDUCAT=3;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPEDUCAT/CHISQ;

DATA COMPARISON21; SET NEW;
IF PPEDUCAT=1 OR PPEDUCAT=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPEDUCAT/CHISQ;

DATA COMPARISON22; SET NEW;
IF PPEDUCAT=2 OR PPEDUCAT=3;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPEDUCAT/CHISQ;

DATA COMPARISON23; SET NEW;
IF PPEDUCAT=2 OR PPEDUCAT=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPEDUCAT/CHISQ;

DATA COMPARISON24; SET NEW;
IF PPEDUCAT=3 OR PPEDUCAT=4;
PROC SORT; BY CASEID;
PROC FREQ; TABLES W1_K1_C*PPEDUCAT/CHISQ;

```