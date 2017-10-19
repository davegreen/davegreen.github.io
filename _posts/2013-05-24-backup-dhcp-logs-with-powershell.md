---
layout: post
title: Backup DHCP Logs with Powershell
date: 2013-05-24 10:40
author: Dave Green
comments: true
categories: [Computers]
---
I've uploaded another little Powershell script to GitHub, as a module this time!

It's called [backupDhcpLogs](https://github.com/davegreen/backupDhcpLogs.git) and it tries to do what it says on the tin. Of course, if you couldn't read the tin, it's a module to back up DHCP logs.

Currently it backs up the previous days of logs to a destination you specify and will also remove logs from the destination that are older than a number of days you specify. There is also the option to back up the DHCP server configuration if you so wish.

However, I've got some more work to do on it. I'd like to offer an option to back up all the logs that exist, while managing them in the same way. Additionally, I would also like to offer a slightly more modern backup method using 'Export-DhcpServer' or 'Backup-DhcpServer', and not 'netsh'.

I also had to remove a check for the DHCP Server Windows feature from the module, as it was stopping it from running when using the 'Run whether the user is logged in or not' option in the task scheduler. It's something I'll figure out eventually, then put the functionality back in.
