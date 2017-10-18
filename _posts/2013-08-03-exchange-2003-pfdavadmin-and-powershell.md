---
layout: post
title: Exchange 2003, PFDAVAdmin and PowerShell
date: 2013-08-03 20:42
author: dave
comments: true
categories: [Computers]
---
I've been working with Exchange 2003 a bit recently, as part of a migration to Office 365. Most of the work has been around getting permission data, mailbox sizes and some other related data.

As part of that, I've written a couple of PowerShell scripts to grab some information we needed for the migration. The first is a script to get data from Exchange and AD in a 2003 domain using WMI. The second is a script to parse and output permission data from a PFDAVAdmin export in a more usable format.

I've put the two scripts on GitHub in my <a href="https://github.com/davegreen/miscellaneous.git">miscellaneous repository</a>, under the PowerShell folder. Hopefully they come in useful to anyone else needing to make more sense of PFDAVAdmin exports, or pull mailbox data from Exchange 2003.
