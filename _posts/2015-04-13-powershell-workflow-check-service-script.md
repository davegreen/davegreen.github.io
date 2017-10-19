---
layout: post
title: PowerShell Workflow - Check-Service script
date: 2015-04-13 21:52
author: Dave Green
comments: true
categories: [Computers]
---
I've recently needed to work with some services in a group of computers where only one service can be active at a time. This particular service I needed to work with is a printing service responsible for printing labels in a shared directory. If more than one service is enabled at a time, we get duplicate labels! (It led to a lot of confusion during user testing when a server was rebooted).

To automate the work required to make sure only one service is running when maintenance or other work occurs, I decided to write a script.

[The script can be downloaded from here.](https://github.com/davegreen/miscellaneous/blob/master/PowerShell/Workflows/Check-Service.ps1)

I used PowerShell workflow, as this seemed like a good bet for something that would benefit from the parallelism benefits that a workflow provides. The idea for me is to use it as part of a System Center Orchestrator Runbook to run the script on an alert from Operations Manager.

I worked on making the script take parameters for the computers to run against, the service to work on and how many services should be running at any one time.

Hopefully this also helps some people working to learn some of the basics of workflow, I'll make sure to add some comments in the script to explain parts of it. Some things certainly confused me for a little while till I got things working!
