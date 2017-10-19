---
layout: post
title: PowerShell Argument Completers
date: 2017-04-19 23:02
author: Dave Green
comments: true
categories: [Computers]
---
Something I do a lot of the time to make people's experience with PowerShell easier, especially when they are new, or reluctant to learn is to write argument completers. A lot of the time, I write them for advanced functions and cmdlets I build, but I also find the odd occasion where I build them for existing commands.

I can't believe I haven't talked about this before, but I started doing this after [watching this awesome video](https://www.youtube.com/watch?v=_GgpwjsFQXc&amp;index=11&amp;list=PLfeA8kIs7Coc1Jn5hC4e_XgbFUaS5jY2i) from the [PowerShell Global summit](https://powershell.org/summit/) by the awesome [Rohn Edwards](https://rohnspowershellblog.wordpress.com/).

I always make sure to ship these even if 99% of the time the script is running completely automated because they are easy to write and, when you find you need them, the convenience is just awesome. Since these are PS v5 commands, you can always make sure you skip these when running on lower versions if you don't want to require v5 as the minimum shell version, since they are entirely optional.

Probably the biggest time saver argument completers I've ever written are for the MSOnline Office 365 PowerShell module, as being able to tab through UPNs (and sometimes tenant IDs, once you start to recognise them :S) is absurdly useful.

These particular argument completers take a dependency on a little function to test whether you're connected to Office 365, but there are other ways to do this as well. This function is also included below.

Anyways, because they've saved me a lot of time, here they are. Hopefully they will save you some time too!:

[Completer 1](https://gist.github.com/davegreen/10006271bd0856cb0c68fb98b3956cbe)

[Completer 2](https://gist.github.com/davegreen/02487224a8371918520db05235033de7)