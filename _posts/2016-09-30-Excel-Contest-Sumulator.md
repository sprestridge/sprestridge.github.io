---
layout: post
title: "Simple Excel Based Contest Simulator"
date:  2016-09-30 17:00:00 -0500
categories: [excel, vba]
excerpt_separator: <!--more-->
---

Occasionally as a modeler with strong Excel and VBA skills one gets asked to create something outside of the realm of regular client work. These requests can often be fun and I find myself learning things with every solution developed. I was recently asked to help select a random winner of a contest for a clothing brand's Instagram account. I have seen many Instagram posts where the winner is determined by a lot of random scrolling up and down on the contest post until the selector decides to place their finger on the screen and "select" the winner. Being Excel and VBA minded, I wanted something a little more robust, random, repeatable, easy to implement, and accomplished with every data analyst's favorite tool (_he said somewhat sarcastically_), Excel! 

<!--more-->

![](/img/2016-09-30-Excel-Contest-Simulator.png)

You can see my final solution in the Excel file (Random Contest Winner Simulator 1.0.xlsm - _instead of providing the file, I am including the above screenshot_). Here's why I like it and what you might find useful in your everyday work:

- **Formatted Excel Table**: This makes adding and deleting rows relatively simple and carries with it standard formatting and formulas that will remain even if the user deletes all the rows of the table. Yes - go ahead and try it. Delete all the rows (you might want to save the list of names somewhere first). Paste a new list of names (doesn't matter how many) into the first column and you'll see the table automatically expand to the correct size with the formulas populated and formatting applied. Run a contest. Boom, winner and nicely formatted table.

- **Data Validation** to guide the user on entry: In cell `D4` which has the range name of `rng_NumSims` you will see a tool tip displayed (Excel actually calls this an Input Message) if you put the cell selector in this cell. This is achieved by using '_Data Validation_' on the **Data Tools** panel of the **Data** ribbon. In this tool I have it set so that the user can enter any whole number from 1,000 to 50,000 and I have set an Input Message to display indicating that.

- **A Button to Run the Macro**: To keep things simple for end users that may not know how to run a macro from the Developer ribbon (and may not even have that ribbon exposed for that matter) I have added a simple text box to the worksheet with the words 'Run Contest'. I right clicked on the text box, selected 'Assign Macro' and chose the `RunContest()` macro contained in the VBA code of this workbook.

- **VBA Code for automation**: As modelers we are often asked to automate repetitive tasks and there is none more repetitive than running a model (contest / simulation), recording the result, and running the model again, etc. A few lines of straightforward VBA code and we have an automated process to run as many simulations as necessary and record and increment the results. Full code provided below.

- **Conditional Formatting**: There is conditional formatting on the table rows such that the ultimate winner winds up being formatted as bold, red text. Simple but effective visual indicator as to the winner. 

That's about all I have to say on this one. Ultimately, the client wound up with a relatively simple tool that can be used over and over for any size contest with just a list of names and the backing of Excel's random number generator and some VBA code. Hope you can find something useful here.

```
Sub RunContest()

Dim i As Long

Dim j As Long

'To see the macro in action, comment out the ScreenUpdating line below

Application.ScreenUpdating = False

'first clear the results of any previous contests

Sheet1.Range("tbl_entries[Total Wins]").ClearContents

'read the number of total simulations to run

j = Sheet1.Range("rng_NumSims").Value

'loop to run simulation, record result, and repeat

For i = 1 To j

Randomize

Calculate

Sheet1.Range("tbl_entries[Total Wins]").Value = Sheet1.Range("tbl_entries[Helper Wins]").Value

Next i

Application.ScreenUpdating = True

End Sub
```
