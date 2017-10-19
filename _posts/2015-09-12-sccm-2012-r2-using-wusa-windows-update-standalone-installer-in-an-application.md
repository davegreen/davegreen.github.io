---
layout: post
title: SCCM 2012 R2 - Using WUSA (Windows Update Standalone Installer) in an Application
date: 2015-09-12 01:21
author: Dave Green
comments: true
categories: [Computers]
---
It's been a little while since my last post, I've not long started a new job so things to write about got put on the back burner for a little while.

I needed to install IE11 on some Windows 7 machines that didn't necessarily have the prerequisite updates. The main thing was the update package couldn't be downloading all its prerequisites from the internet, because that's just not professional!

This makes things a little more complicated from an SCCM perspective, since I can't just go and install IE11 directly, I have to make sure the prerequisites get installed. I had a play around with the IEAK 11 (Internet Explorer Administration Kit), but it didn't handle the prerequisites particularly well, I had a few failures, then decided to sort the dependencies myself.

This is where WUSA comes in! I built the IE11 application as normal, just calling the IE offline installer executable directly with the '/quiet', '/update-no', '/norestart' and '/closeprograms' switches, then gave it some [Windows updates that required as prerequisites as dependencies](https://support.microsoft.com/en-gb/kb/2847882) for the deployment.

This went well, till I ran into some odd failures with some computers. I looked at the logs and WUSA was returning some odd return codes. I went on a bit of a search and [found a list of return codes ](https://support.microsoft.com/en-us/kb/938205)that applied, but none of them matched which annoyed me for a second till I realized that WUSA was returning the codes, but the SCCM AppEnforce.log was logging them as decimal, not the hexadecimal values shown in the knowledge base article!

A quick bit of converting from decimal to hex and I was there. I added the return codes to the deployments and all was well! Here's the two main ones I ended up using:
<p style="padding-left: 30px;">2359301 (0x00240005) Success (Installed but the system must be restarted to complete installation of the update).</p>
<p style="padding-left: 30px;">2359302 (0x00240006) Failure (The update to be installed is already installed on the system). This error highlights that you might have a bad detection rule in place.</p>
<p style="padding-left: 30px;">2145124330 (0x80240016) Failure (Operation tried to install while another installation was in progress or the system was pending a mandatory restart).</p>
There's loads more I could have used, but I like to only put the extra return codes in the application when I need to, keep it simple!
