---
layout: post
title: Server 2016 - Remote Desktop Session Host Start Menu
date: 2017-03-23 23:05
author: Dave Green
comments: true
categories: [Computers]
---
Last week we ran into a small issue with one of our first Server 2016 deployments when using it as a Remote Desktop Session Host (RDSH) that took us a little while to figure out.

## The Problem

When we added the machine to the RDSH collection, the machine would then fail to load any Appx apps (including the start menu (ShellExperienceHost!). We initially had a bit of trouble targeting the cause, as other than adding the machine to the session collection, nothing had changed.

## Some Troubleshooting

We did find some events saying the application couldn't be installed in the application log (for the first login only), but also every time in the TWinUI log. This gave me a good idea of where the problem lay, but we followed through looking at the logs to see if it would give us some more information.

This log is found in the following places:

<pre>Applications and Services Logs\Microsoft\Windows\Apps\Microsoft-Windows-TWinUI/Operational</pre>

or

<pre>%SystemRoot%\System32\Winevt\Logs\Microsoft-Windows-TWinUI4%Operational.evt</pre>

The events looked like the following:

<pre>Log Name: Microsoft-Windows-TWinUI/Operational
Source: Microsoft-Windows-Immersive-Shell
Date: 20/03/2017 14:08:32
Event ID: 5985
Task Category: (5961)
Level: Error
Keywords:
User: DOMAIN\user
Computer: RDSH.domain.local
Description:
ActivateApplicationForContractByAppIdAsUserWithHost of the app Microsoft.Windows.ShellExperienceHost_cw5n1h2txyewy!App for the Windows.Launch contract failed with Install failed. Please contact your software vendor..</pre>

We also tried trusty [ProcMon](https://technet.microsoft.com/en-us/sysinternals/processmonitor.aspx) (nothing obviously access denied or wrong), all that would happen WerFault.exe would appear when trying to run ShellExperienceHost.exe manually.

<pre>Log Name: Application
Source: Application Error
Date: 20/03/2017 13:55:19
Event ID: 1000
Task Category: (100)
Level: Error
Keywords: Classic
User: N/A
Computer: RDSH.domain.local
Description:
Faulting application name: ShellExperienceHost.exe, version: 10.0.14393.447, time stamp: 0x5819bf85
Faulting module name: Windows.UI.Xaml.dll, version: 10.0.14393.953, time stamp: 0x58ba5c3d
Exception code: 0xc0000409
Fault offset: 0x00000000000521d0
Faulting process id: 0xbe0
Faulting application start time: 0x01d2a1819c994d20
Faulting application path: C:\Windows\SystemApps\ShellExperienceHost_cw5n1h2txyewy\ShellExperienceHost.exe
Faulting module path: C:\Windows\System32\Windows.UI.Xaml.dll
Report Id: 4c97f524-b5e8-44c0-b03a-2b939c6924d2
Faulting package full name:
Faulting package-relative application ID:</pre>

## A Clue!

We then looked in the next place we usually look when this kind of thing goes on, even though there were no changes to the machine, other than adding the machine to the session collection.

You guessed it, the AppLocker logs! We saw a few events like this:

<pre>Log Name: Microsoft-Windows-AppLocker/Packaged app-Deployment
Source: Microsoft-Windows-AppLocker
Date: 20/03/2017 14:08:22
Event ID: 8026
Task Category: None
Level: Error
Keywords:
User: DOMAIN\user
Computer: RDSH.domain.local
Description:
No packaged apps can be executed while Exe rules are being enforced and no Packaged app rules have been configured.</pre>

<pre>Log Name: Microsoft-Windows-AppLocker/Packaged app-Execution
Source: Microsoft-Windows-AppLocker
Date: 17/03/2017 17:13:35
Event ID: 8027
Task Category: None
Level: Error
Keywords:
User: DOMAIN\user
Computer: RDSH.domain.local
Description:
No packaged apps can be executed while Exe rules are being enforced and no Packaged app rules have been configured.</pre>

Turns out the issue was Applocker blocking the install and the run of the app, which is odd since it wasn't affecting us before. This problem doesn't seem to affect administrative users that log on before the machine is added to the session collection (which I believe is the expected behaviour). The unexpected behaviour is that AppLocker policies appear to be enforced for all users when the server is in a session host collection, which in our case prevented the start menu from working.

## The Solution

A simple fix in the end.

We added the default Appx signed apps allowed rule to our Applocker Policy, however you could make sure just the required applications (like ShellExperienceHost) are allowed.

I'm glad we got to the bottom of it reasonably quickly and I hope that this post might help some others with the same issues.
