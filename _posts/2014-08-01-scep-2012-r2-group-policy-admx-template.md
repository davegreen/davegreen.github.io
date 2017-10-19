---
layout: post
title: SCEP 2012 R2 - Group Policy ADMX Template
date: 2014-08-01 08:00
author: Dave Green
comments: true
categories: [Computers]
---
I've been working a lot with System Center Configuration Manager (SCCM) and System Center Endpoint Protection (SCEP) recently and as part of the work, I'll be migrating servers over to using SCEP. We have decided on a two part move, firstly moving antivirus to SCEP with Group Policy management, then later adding the SCCM client if we feel it would be better. Obviously reporting, status, definition updates and other useful features are nice, but we have System Center Operations Manager (SCOM) for monitoring, so we will certainly write some monitors for this!

Initially theres a roadblock, as the only ADMX provided is as part of the [Forefront Endpoint Protection 2010 tools](http://www.microsoft.com/en-gb/download/details.aspx?id=13088) and it's not obvious it applies to SCEP 2012. Fortunately Microsoft haven't changed anything since then, so it's essentially a drop in solution. However, if you want to be nice and use the right names for things, you need to do a little bit of work to alter the friendly names, which I've done [here](https://github.com/davegreen/miscellaneous/tree/master/CustomADMX). Copy the files into the "%systemroot%\PolicyDefinitions" folder locally to test, then into the [policy central store](http://support.microsoft.com/kb/929841) for your domain when it makes sense.

[SystemCenterEndpointProtection2012R2.admx](https://github.com/davegreen/miscellaneous/blob/master/CustomADMX/SystemCenterEndpointProtection2012R2.admx)

[SystemCenterEndpointProtection2012R2.adml](https://github.com/davegreen/miscellaneous/blob/master/CustomADMX/en-US/SystemCenterEndpointProtection2012R2.adml)

EDIT: Latest versions are:

[SystemCenterEndpointProtection.admx](https://github.com/davegreen/miscellaneous/blob/master/CustomADMX/SystemCenterEndpointProtection.admx)

[SystemCenterEndpointProtection.adml](https://github.com/davegreen/miscellaneous/blob/master/CustomADMX/en-US/SystemCenterEndpointProtection.adml)