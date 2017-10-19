---
layout: post
title: SCOM 2012 - Firewall state PowerShell monitor script
date: 2016-02-25 21:12
author: Dave Green
comments: true
categories: [Computers]
---
Hmm, long time no post...

This script requires the excellent [Wizard to Create Powershell-based Monitors Management Pack](https://gallery.technet.microsoft.com/Sample-Management-Pack-17b76379) (or a similar Management Pack to run PowerShell scripts from SCOM).

It generates a property bag of whether you have each of the three Windows Firewall profiles enabled/disabled and (depending on how you've configured the monitor), alerts on them.

[FirewallState](https://gist.github.com/davegreen/5eabc188f78b2576c70e)

It's a very useful little script to ferret out those little servers with the firewall turned off!
