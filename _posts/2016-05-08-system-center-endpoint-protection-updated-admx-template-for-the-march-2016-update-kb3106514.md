---
layout: post
title: System Center Endpoint Protection – Updated ADMX Template for the March 2016 Update – KB3106514
date: 2016-05-08 01:56
author: Dave Green
comments: true
categories: [Computers]
---
The new update has been out for a little while now ([KB3106514](https://support.microsoft.com/en-us/kb/3106514)) and brings with it three new settings.

<blockquote>HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Microsoft Antimalware\MpEngine
<span class="text-base">DWORD name:</span> MpEnablePUS</blockquote>

This setting enables detection and removal of Potentially Unwanted Applications (PUA) downloaded through IE, Firefox or Chrome. One thing about this is that it will only apply to new detections going forward. This setting will not cause existing PUAs to be detected and removed.

<blockquote>
<div class="reg_path">HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Microsoft Antimalware\UX Configuration</div>
<span class="text-base">DWORD name:</span> SuppressRebootNotification</blockquote>

This is a setting to suppress the reboot notification from the client if it detects that a reboot is required to finish the clean-up of any malware. This is useful in shared environments (RDS, etc.), where a this kind of thing would not be fun.

<blockquote>
<div class="reg_path">HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Microsoft Antimalware</div>
<span class="text-base">DWORD name:</span> ThreatFileHashLogging</blockquote>

This setting records an event with ID 1120 to the log file containing the SHA-1 hash of the affected file for more research and correlation with other infections or threats.

There's also a link from the knowledge base page to [a script on the PowerShell gallery](https://www.powershellgallery.com/packages/SignatureDownloadCustomTask) for setting up anti-malware client updates on a UNC share, which is quite nice for new deployments, without using something like System Center Configuration Manager (SCCM) or Windows Server Update Services (WSUS).

I have added these updates to my ADMX template for System Center Endpoint Protection, which can be [downloaded from GitHub](https://github.com/davegreen/miscellaneous/tree/master/CustomADMX). Note that from this update on, the file names and data drop the 2012R2 version number from the file name, which makes more sense going forward. The old files are still there for reference.

The direct links to the files are:

<a href="https://github.com/davegreen/miscellaneous/blob/master/CustomADMX/SystemCenterEndpointProtection.admx">SystemCenterEndpointProtection.admx
</a>[SystemCenterEndpointProtection.adml](https://github.com/davegreen/miscellaneous/blob/master/CustomADMX/en-US/SystemCenterEndpointProtection.adml)

<a id="kb-link-3" href="https://www.powershellgallery.com/packages/SignatureDownloadCustomTask" target="_self"></a>It's been just over a year since the last policy template settings change from Microsoft for their Endpoint Protection products and still no sign of an official file! I'll keep on with the updates for this until Microsoft sort it out.

UPDATE:

I've made a couple more changes to add two new policy options that I had previously overlooked, these are:

<blockquote>
<div class="reg_path">HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Microsoft Antimalware\Real-Time Protection</div>
<span class="text-base">DWORD name:</span> DisableScriptScanning</blockquote>

This setting provides an admin override to disable script scanning.

<blockquote>
<div class="reg_path">HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Microsoft Antimalware\Real-Time Protection</div>
<span class="text-base">DWORD name:</span> LocalSettingOverrideDisableScriptScanning</blockquote>

This setting allows the local client setting for script scanning to take precedence over a group policy setting.
