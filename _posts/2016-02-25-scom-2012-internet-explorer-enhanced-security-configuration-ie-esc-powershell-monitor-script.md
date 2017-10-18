---
layout: post
title: SCOM 2012 - Internet Explorer Enhanced Security Configuration (IE-ESC) PowerShell monitor script
date: 2016-02-25 21:17
author: dave
comments: true
categories: [Computers]
---
This script requires the excellent <a href="https://gallery.technet.microsoft.com/Sample-Management-Pack-17b76379">Wizard to Create Powershell-based Monitors Management Pack</a> (or a similar Management Pack to run PowerShell scripts from SCOM).

It generates a property bag of whether you have each of the Internet Explorer Enhanced Security Configuration (IE-ESC) profiles enabled or disabled and (depending on how you've configured the monitor), alerts on them.

https://gist.github.com/davegreen/622c9e417a1e200a5c8e

It's just another useful little script for maintaining a good security posture for servers in your organisation.
