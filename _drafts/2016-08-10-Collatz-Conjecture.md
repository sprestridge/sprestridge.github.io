---
layout: post
title:  "Fun with the Collatz Conjecture"
date:   2016-08-10 16:00:00 -0500
categories: excel
---
I had heard about this mathematical tidbit in grad school and forgotten all about it. For some reason it has been making the rounds on the Internet and social media recently and I stumbled upon it again. I think it was this YouTube video I saw most recently: 

[https://www.youtube.com/watch?v=5mFpVDpKX70](https://www.youtube.com/watch?v=5mFpVDpKX70)

Here is the basic principle. Take any positive integer n. If n is even, divide it by 2 (n/2). If n is odd, multiply it by 3 and add 1 (3n + 1). Repeat the process on the result (which has been called "_Half Or Triple Plus One_", or HOTPO) indefinitely. The **Collatz Conjecture** is that _no matter what positive integer you begin with the result will eventually reach 1_. 

I put together an Excel workbook with some simple VBA code to run the math and to play with the results. [Collatz Conjecture.xlsm](https://boozallen-my.sharepoint.com/personal/520280_bah_com/Documents/Shared%20with%20Everyone/Collatz%20Conjecture.xlsm?web=1). (let me know if that link does not work) The file uses tables, a worksheet change event to trigger the macro to run when the user changes the starting integer, and a formula in place of the Mod VBA function. The use of the formula in place of Mod is interesting and amounts to the fact that in VBA, Mod takes a Double but converts it to an Integer before calculating the result and thus cannot handle very large numbers [1]. 

[1]: https://support.microsoft.com/en-us/kb/205053 

It is interesting to see how certain numbers go down to 1 rather quickly (see 16, 1,024, and 24 for example) and others bounce around and even get significantly larger than the starting value before finally going to 1 (see 22, 120,000, and 120,021 for example). In the screen shot below, you can see the graph for starting with 120,021 – it takes 180 steps to reach 1 and the maximum integer is 360,064. Play around with the file.

![Collatz Graph 120,021](/img/CollatzConjectureGraph.png)

From a VBA perspective, the interesting items are the replacement of the Mod function, the use of the worksheet change event, and the Do While Loop. Originally I was going to prompt the user to enter the starting integer with an input box but decided to just go with direct entry in the worksheet and have VBA code via the worksheet change event trigger the main subroutine to run. I commented the input box code out but left it in the procedure for anyone to experiment.
