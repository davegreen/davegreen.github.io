---
layout: post
title: PowerShell - Timezone Module
date: 2015-09-26 18:29
author: dave
comments: true
categories: [Computers]
---
I've been working on a few different things recently and I've got into a few nice things in PowerShell that I haven't been using before. The magic of Pester is one of them! I needed to write a couple of functions to get and set the timezone on a computer. It's not hugely complicated, but it does the job. I wrote up some tests first to get an idea on the objects I would return and how the user would interact with the cmdlets.

Also, instead of using a large ValidateSet array that would require maintaining for the Set-Timezone function, the function uses a ValidateScript that gets the valid inputs for <a href="https://technet.microsoft.com/en-us/library/hh875624.aspx">tzutil</a> dynamically, which is nice!

I'm still trying to figure out how to handle the _dstoff option properly, I'll keep on it and update the module once I get it sorted.

The <a href="https://github.com/davegreen/miscellaneous/tree/master/PowerShell/Modules/Timezone">Timezone module can be found on my GitHub</a>.
