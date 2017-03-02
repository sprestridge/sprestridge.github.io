---
layout: post
title:  "Dice Words, Excel, and Secure Passphrases"
date:   2015-04-06 06:30:00 -0500
categories: excel
---
I am not entirely sure how I first heard about developing secure passphrases using random numbers and a table of words, it was likely a [blog post](http://blog.agilebits.com/2011/06/21/toward-better-master-passwords/) from the makers of 1Password, but in any event I stumbled upon [this Dr. Drang post](http://leancrew.com/all-this/2015/04/passphrases-via-shell-pipeline/) over the weekend that revisits the topic in a uniquely nerdy fashion. As I am not as command-line-competent as Dr. Drang, I had already developed my own solution using Excel when I first read about this method of developing passphrases. See my solution in [this Excel file](LINK TO FILE) and feel free to use it as you see fit. I think Dr. Drang's post and the posts he links to cover the theory so if you are interested, please read up there, I'll wait.

In my Excel implementation, I have stored the dice words in column B and the corresponding dice numbers in column A (and named the ranges). Every time the file recalculates cells E7:J12 produce the required set of six, five-digit random numbers. A simple Index / Match formula in K7:K12 grabs the corresponding dice words from the table and cell K5 mashes them together for the final randomly generated passphrase. If you come across a particular combination that you like you can paste the six, five-digit random numbers as values into N7:N12 or N16:N21 for temporary storage until you can commit the six words to memory and have some practice typing them.

To demonstrate how you might use this file let's assume I need to change my My Space password and I want a nice 29 character long passphrase that is randomly generated. I would simply keep calculating the workbook until I got a passphrase that was the right length and that I generally liked the six words calculated. In the file you can see that saved passphrase #1 contains the words brassy, verb, 61st, gulf, bisque, and tapis which combine to make my new passphrase (no, not really): brassyverb61stgulfbisquetapis. Once I change my My Space passphrase I would delete these six numbers and move on. Please note, NEVER save the file with the digits stored in one of the temporary locations for a passphrase you chose to use.

Getting back to what I really liked about Dr. Drang's post is that his solution allows for the use of a custom corpus. I love the idea of pulling my latest passphrase from "Origin of Species" or some other novel or well-known text. I might have to learn enough about his method to be able to update my Excel file with the list of unique words from "Origin" or to be able to execute his solution from the command line, but those are projects for another day.

### Links

[1] 1Password post: [http://blog.agilebits.com/2011/06/21/toward-better-master-passwords/](http://blog.agilebits.com/2011/06/21/toward-better-master-passwords/)

[2] Dr. Drang post: [http://leancrew.com/all-this/2015/04/passphrases-via-shell-pipeline/](http://leancrew.com/all-this/2015/04/passphrases-via-shell-pipeline/)

[3] Excel Dice Words: (LINK TO FILE)