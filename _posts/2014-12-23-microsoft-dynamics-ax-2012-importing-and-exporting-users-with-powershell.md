---
layout: post
title: Microsoft Dynamics AX 2012 - Importing and exporting users with PowerShell
date: 2014-12-23 20:38
author: Dave Green
comments: true
categories: [Computers]
---
Over the past couple of months I've occasionally been working with Microsoft Dynamics AX 2012 and moving model and data sets between different environments.

Once of the downsides to this is overwriting the user and role memberships at the destination. To combat this, I had two options, the AX Data Import/eXport Framework (DIXF), or the AX PowerShell module. I think you can guess which one I went for!

Unfortunately the AX module need to be loaded in a wonderfully manual way and there are a lot of features not provided. The cmdlets even have a horrible lack of support for the very things that make PowerShell so great, like pipelining!

Even lacking these things, it's still PowerShell, so I soldiered on. It took a little while and some features still have to be implemented, but the two scripts allow users to be imported and exported to/from a CSV, which is good enough to release for a first version.

The two scripts can be [downloaded from GitHub](https://github.com/davegreen/miscellaneous/tree/master/PowerShell/DynamicsAX).

- [Import-AXUser.ps1](https://github.com/davegreen/miscellaneous/tree/master/PowerShell/DynamicsAX/Import-AXUser.ps1)
- [Export-AXUser.ps1](https://github.com/davegreen/miscellaneous/tree/master/PowerShell/DynamicsAX/Export-AXUser.ps1)

There are also some features I'll implement if I get the time, such as complete overwrite on import and support for importing/exporting user data from remote servers.