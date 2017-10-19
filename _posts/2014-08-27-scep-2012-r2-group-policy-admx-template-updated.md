---
layout: post
title: SCEP 2012 R2 – Group Policy ADMX Template Updated
date: 2014-08-27 22:00
author: Dave Green
comments: true
categories: [Computers]
---
A quick followup to my earlier post on the SCEP 2012 ADMX template, I was working today with our Citrix environment and needed to remove visibility of the SCEP client interface.

Fortunately, there is an option in the SCCM Endpoint Protection policies, so I know the functionality to do this is there. This doesn't seem to have been an option in Forefront Endpoint Protection (FEP) 2010, otherwise it would have been in the original policy template.

The value for this is "UILockdown" and is found next to the other UX configuration settings for SCEP:

    HKLM\Software\Policies\Microsoft\Microsoft Antimalware\UX Configuration\UILockdown

    (1 for disabled, 0 for enabled)

I've updated the ADMX template to make this new setting visible, the changes I've made to the template are [here for the ADMX](https://github.com/davegreen/miscellaneous/commit/8f77ebd4fd70dfe8a410d9a1a4699bd126f77097) and [here for the ADML](https://github.com/davegreen/miscellaneous/commit/1420cb6ab81a574b7115a98c531b7ad13c7132b7).

The files can be [downloaded here](https://github.com/davegreen/miscellaneous/tree/master/CustomADMX). I'll also continue to update the template as I find other settings that weren't present in FEP 2010.
