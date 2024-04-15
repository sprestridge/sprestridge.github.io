---
layout: post
title: "TIL: SORT, UNIQUE, VSTACK"
date:  2024-04-03 12:00:00 -0500
categories: excel
excerpt_separator: <!--more-->
---

## How to use three formulas to combine and sort the unique values from two different lists (arrays)

Imagine two very long lists of unique codes (names, id numbers, any unique identifier). You need a single list of the unique codes. There are several approaches but I learned about `VSTACK` recently, have wanted to use it, and had to look it up again to apply it, so I am writing this as a TIL - today I learned.

Use the two lists to combine (`VSTACK`) them into a single list of unique values (`UNIQUE`) that is sorted (`SORT`).

<!--more-->

In the screen shot, the array formula in cell D3 combines these three Excel functions to produce the sorted list of unique alpha codes. Two adjacent Boolean columns give a 1 or a 0 depending on whether the alpha code is from list one or two. `ISNUMBER` and `MATCH` are used with double unary characters to return the 1's and 0's. A value of 1 indicates it was from that list; a value of 0 indicates it was not.

![](/img/2024-04-03_SORT-UNIQUE-VSTACK.png "Screenshot showing the two lists and the formula combining SORT, UNIQUE, and VSTACK described in this post.")

The final column, **Source**, uses an `IF` formula and the Boolean columns to indicate where the alpha code appeared--list 1, list 2, or both lists. Careful readers will note that **EVER appears in both lists** and the `IF` formula correctly identifies that in column G.


### Formulas

**D3**: `= SORT( UNIQUE( VSTACK(A3:A7, B3:B8)))`

**E3**: `= --ISNUMBER( MATCH( D3#, $A$3:$A$7, 0))`

**F3**: `= --ISNUMBER( MATCH( D3#, $B$3:$B$8, 0))`

**G3**: `=IF(AND(E3, F3), "Both", IF(E3, "List 1", "List 2"))`


### Functions

[VSTACK](https://support.microsoft.com/en-us/office/vstack-function-a4b86897-be0f-48fc-adca-fcc10d795a9c) - Appends arrays vertically and in sequence to return a larger array.

[SORT](https://support.microsoft.com/en-us/office/sort-function-22f63bd0-ccc8-492f-953d-c20e8e44b86c) - The SORT function sorts the contents of a range or array. 

[UNIQUE](https://support.microsoft.com/en-us/office/unique-function-c5ab87fd-30a3-4ce9-9d1a-40204fb85e1e) - The UNIQUE function returns a list of unique values in a list or range. 

[ISNUMBER](https://support.microsoft.com/en-us/office/is-functions-0f2d7971-6019-40a0-a171-f2d869135665) - checks whether a specified value is a number (TRUE) or not (FALSE)

[MATCH](https://support.microsoft.com/en-us/office/match-function-e8dffd45-c762-47d6-bf89-533f4a37673a) - The MATCH function searches for a specified item in a range of cells, and then returns the relative position of that item in the range.

[IF](https://support.microsoft.com/en-us/office/if-function-69aed7c9-4e8a-4755-a9bc-aa8bbff73be2) - makes logical comparisons and can have two results, one if the comparison is TRUE and the other if the comparison is FALSE.
