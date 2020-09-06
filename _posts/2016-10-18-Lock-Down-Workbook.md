---
layout: post
title:  "Lock Down an Excel Workbook"
date:   2016-10-18 16:00:00 -0500
categories: excel
excerpt_separator: <!--more-->
---

Sometimes we are asked by clients to set something up where after a certain date access to the file or tool is limited. Maybe data in the tool becomes outdated and they would like the opportunity to update it and lock users out from using it past the expiration or maybe they just don't want end users to have the full functionality of the tool beyond a certain date. This example workbook provides a method for giving a client just that. I have to credit Dan Schriever for putting the workbook together and the macros – I believe the majority of it was his work which he later showed me and I modified in order to put this example and post together.

<!--more-->

Hope it is useful and something can be learned from it. As with everything in Excel and VBA, I recognize there are plenty of other ways to accomplish the same thing; this represents one solution to a rather specific client request. 

There are several pieces that all work together in this example to allow the user to set an expiration date for a workbook such that past the expiration the workbook opens and only displays certain worksheets with a message to the user that the workbook has expired. The expiration message can be customized as can the worksheets that are displayed or not.

Get the workbook [here](/files/Lockdown.xlsm)


## Worksheets

1.  **Index** - this is the main worksheet of the tool and always visible to the end user even past the expiration date.

2.  **Test Sheet A** - this is a "sensitive" worksheet that is only available and visible to the user prior to the expiration date.

3.  **Test Sheet B** - this is a "sensitive" worksheet that is only available and visible to the user prior to the expiration date.

4.  **P99-Control** - this is an administrative worksheet where the controls are set (Sent to, Date Sent, Duration, Contact Name and Information) via named ranges and a combination of administrative entry (blue italicized text) and formulas.


## Macros

1. `cm_Unhide_Control_Sheet()` - this custom macro (cm) is the only way to unhide the control sheet because the P99-Control worksheet is set to xlVeryHidden by the "Hide All Sheets" macro. Very Hidden worksheets are not visible to the user when selecting a worksheet and right-clicking to bring up the context menu to Unhide/Hide worksheets. If you are familiar with the VBA editor you can also manually change the property to xlSheetVisible. For ease of use to the developer / admin, a shortcut of Ctrl-Shift-A has been assigned to run this macro.

2. `cm_Unhide_All_Sheets()` - custom macro (cm) to unhide all worksheets. Note that this will not unhide the control workhseet.

3. `cm_Hide_All_Sheets()` - custom macro (cm) that hides all worksheets except the Index worksheet and makes them xlVeryHidden. **xlVeryHidden** is a special property that makes it difficult for the user to determine if there are hidden worksheets in a workbook.

4. `Check_Expiration()` - checks to see if the expiration date has passed and if it has sends a message box to the user with custom specified POC information for the tool. If it has not the macro calls the 'unhide all sheets' macro.


## Events

1. `Workbook_BeforeClose` - an event that is active for the workbook (ThisWorkbook) that calls the hide all worksheets macro before the file is closed.

2. `Workbook_Open` - this event runs on open and checks the expiration date by calling that macro.