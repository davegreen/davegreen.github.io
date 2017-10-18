---
layout: post
title: SCCM 2012 R2 - Problems with SCEP 2012 R2 and Group Policy Results
date: 2014-10-30 12:32
author: dave
comments: true
categories: [Computers]
---
We ran into an issue yesterday with Group Policy Results when using SCEP 2012 R2, the problem exactly follows this particular issue on the <a href="https://social.technet.microsoft.com/Forums/en-US/651e9006-adb8-4eb4-ac65-141765d235f0/system-center-endpoint-and-group-policy-results?forum=configmanagergeneral">TechNet Forums</a>, but appears to affect a couple of extra registry keys.

It's really unfortunate that this still isn't fixed in SCCM 2012 R2, having also been an issue in 2012, as the post describes.

To resolve the problem, I used my existing <a href="https://github.com/davegreen/miscellaneous/tree/master/CustomADMX">SCEP Group Policy ADMX template</a> in creating a GPO to replicate the default settings pushed out to clients with SCCM, which solves the problem, as the exceptions and settings we push out to standard clients aren't any different from the Microsoft recommended settings.

In the GPO I had to specify the exclusion settings and also the default threat actions, which are specified in the registry here:
<pre>HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Microsoft Antimalware\Threats\ThreatSeverityDefaultAction</pre>
Previously these settings were defined as REG_DWORD, but need to be REG_SZ, as shown below:

<a href="http://tookitaway.co.uk/wp-content/uploads/2014/10/ThreatSeverityDefaultAction.png"><img class="alignnone size-full wp-image-1401" src="http://tookitaway.co.uk/wp-content/uploads/2014/10/ThreatSeverityDefaultAction.png" alt="ThreatSeverityDefaultAction" width="748" height="252" /></a>
