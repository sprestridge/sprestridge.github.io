---
layout: post
title: "Write Better Code"
date: 2025-01-04 12:00:00 -0500
categories: ai, data science
excerpt_separator: <!--more-->
---

Did you know that continually asking an LLM to "_write better code_" works?

In [Can LLMs write better code if you keep asking them to “write better code”?](https://minimaxir.com/2025/01/write-better-code/) Max Woolf investigates and kicks-off with a brief review of the short-lived meme where users asked a Stable Diffusion model to "_make it more bro_" (really, go look at the hero image on the blog post).

Ultimately, Max showed that LLMs can indeed make better code just by being asked.

[--more--]({{ site.baseurl }}{% link _posts/2025-01-04-Write-Better-Code.md %})

<!--more-->

> In all, asking an LLM to "write code better" does indeed make the code better, depending on your definition of better. Through the use of the generic iterative prompts, the code did objectively improve from the base examples, both in terms of additional features and speed. Prompt engineering improved the performance of the code much more rapidly and consistently, but was more likely to introduce subtle bugs as LLMs are not optimized to generate high-performance code. As with any use of LLMs, your mileage may vary, and in the end it requires a human touch to fix the inevitable issues no matter how often AI hypesters cite LLMs as magic.
> 
> There are a few optimizations that I am very surprised Claude 3.5 Sonnet did not identify and implement during either experiment. Namely, it doesn't explore the statistical angle: since we are generating 1,000,000 numbers uniformly from a range of 1 to 100,000, there will be a significant amount of duplicate numbers that will never need to be analyzed. The LLM did not attempt to dedupe, such as casting the list of numbers into a Python `set()` or using numpy's `unique()`. I was also expecting an implementation that involves sorting the list of 1,000,000 numbers ascending: that way the algorithm could search the list from the start to the end for the minimum (or the end to the start for the maximum) without checking every number, although sorting is slow and a vectorized approach is indeed more pragmatic.

The best part is that Max's code is available on GitHub including the full, unedited conversation threads for the two methods of prompting he covers:

- [Casual prompting](https://github.com/minimaxir/llm-write-better-code/blob/main/python_30_casual_use.md)

- [Prompt engineering](https://github.com/minimaxir/llm-write-better-code/blob/main/python_30_prompt_engineering.md)

After an initial bout of hard work maybe someday the LLMs will just say "[I would prefer not to](https://en.wikipedia.org/wiki/Bartleby%2C_the_Scrivener)".
