---
layout: post
title:  "Data Analysis and Interpretation: Chi-Squared Analysis"
date:   2018-02-18 19:16:00 -0500
categories: training
---

I am using the 'Outlook on Life' survey data to investigate if age or education play a role in whether people think _blacks and other minorities are treated the same as whites in the criminal justice system_ (`W1_K4` in the codebook) or in whether they have _trust in the legal system_ (`W1_K1_C` in the codebook). Since both of these categorical response variables have multiple levels, as do the explanatory variables, they need to be collapsed into fewer groups. `W1_K4` (belief in equal treatment in criminal justice system) will be collapsed from 7 levels to 3 levels - Recieve equal treatment, No opinion, and Do not receive equal treatment. `W1_K1_C` (trust in legal system) will be collapsed from 4 levels to 2 levels - Trust and Mistrust.

## Do you think that blacks and other minorities are treated the same in the criminal justice system?

The p-value of 0.1573 indicates that belief in whether or not blacks and other minorities are treated the same in the criminal justice system is not associated with age. So we cannot reject the null hypothesis - that there is no difference across age categories in belief categories for whether blacks and other minorities are treated the same. (Chi-Squared = 9.3016, DF=6, p-value=0.1573)

The p-value of less than 0.0001 indicates that belief in whether or not blacks and other minorities are treated the same in the criminal justice system is associated with education levels. So we accept the alternate hypothesis - that not all education categories are equal across belief categories for whether blacks and other minorities are treated the same. We know all are not equal but we do not know which are different and which are not. (Chi-Squared = 34.3372, DF=2, p-value<0.0001)

Adjusted Bonferroni p-value for Education = $p / c = 0.05 / 6 = 0.008$

In order to protect against type 1 error in the context of the Chi-Square test it is appropriate to use the post hoc approach known as the Bonferroni Adjustment. Post hoc comparisons of education levels revealed that there is a statistical difference between those with high school or lower levels of education and those with college or greater levels of education.


## Do you trust the legal system?

The p-value of 0.3152 indicates that trust in the legal system is not associated with age. The null hypothesis cannot be rejected - there is not enough evidence to say trust in the legal system is different across age groups. (Chi-Squared = 3.5431, DF=3, p-value=0.3152)

The p-value of less than 0.0001 clearly indicates that trust in the legal system is associated with education levels. So we accept the alternate hypothesis - that not all education categories are equal across trust in the legal system. We know all are not equal but we do not know which are different and which are not.

Adjusted Bonferroni p-value = $p / c = 0.05 / 4 = 0.0125$

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

/* W1K1C variable 
    1=TRUST 
    2=MISTRUST */
if W1_K1_C=1 or W1_K1_C=2 then W1K1C=1;
else W1K1C=2;

/* W1K4 variable 
    1=Recieve equal treatment 
    2=no opinion
    3=Do not receive equal treatment */
if W1_K4<3 then W1K4=1;
else if W1_K4=4 then W1K4=2;
else W1K4=3;


/* add formatting for W1_K1_C - Trust Legal System */
PROC FORMAT;
    value W1_K1_C_groups
    1-2='Trust'
    3-high='Mistrust';

/* add formatting for W1K1C - Trust Legal System */
PROC FORMAT;
    value W1K1C_groups
    1='Trust'
    2-high='Mistrust';

/* formatting for W1_K4 - Treated Same as Whites */
PROC FORMAT;
    value W1_K4_groups
    low-1='Receive equal treatment'
    7='Do not receive equal treatment'
    9-high='Not sure';

/* formatting for W1K4 - Treated Same as Whites */
PROC FORMAT;
    value W1K4_groups
    low-1='Receive equal treatment'
    2='No opinion'
    3='Do not receive equal treatment';

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
PROC FREQ; TABLES W1K4*PPAGECT4/CHISQ;
    Format W1K4 W1K4_groups.;
    Format PPAGECT4 PPAGECT4_groups.;

PROC FREQ; TABLES W1K4*PPEDUCAT/CHISQ;
    Format W1K4 W1K4_groups.;
    Format PPEDUCAT PPEDUCAT_groups.;

/* look at trust in the legal systeam - W1_K1_C */
PROC FREQ; TABLES W1K1C*PPAGECT4/CHISQ;
    Format W1K1C W1K1C_groups.;
    Format PPAGECT4 PPAGECT4_groups.;

PROC FREQ; TABLES W1K1C*PPEDUCAT/CHISQ;
    Format W1K1C W1K1C_groups.;
    Format PPEDUCAT PPEDUCAT_groups.;

RUN;

```