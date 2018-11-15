---
layout: post
title: SCCM 2012 R2 - Operating System deployment when on mains power only
date: 2014-08-02 08:20
author: Dave Green
comments: true
categories: [Computers]
---
It's a quick little script I just had to write, after my testing today ran into the minor issue of a flat battery... halfway through the Operating System Deployment (OSD) process.

It's a little PowerShell script, very similar to [the last one](https://tookitaway.co.uk/sccm-2012-r2-validating-and-setting-osdcomputername-with-powershell) that just pops up a box asking you to plug-in the laptop if you are running on battery. I haven't put much in the way of validation that the device is actually a laptop with a battery, but since my OSD task sequences have a laptop/desktop divide, it's not too much of a problem!

[Here's the script](https://github.com/davegreen/miscellaneous/tree/master/PowerShell/SCCM/OSDBatteryStatus), the setup instructions are similar to [the earlier script](https://tookitaway.co.uk/sccm-2012-r2-validating-and-setting-osdcomputername-with-powershell), with a different name and a call to a batch file instead of the ServiceUI command.

The batch script contains the command to run ServiceUI, after deciding which copy to run the PowerShell script with, based on the boot image architecture (x86/x64). I've put this in at the beginning of the task sequence, once the laptop has booted to the boot image, so we can get the user input or error states dealt with up front.
