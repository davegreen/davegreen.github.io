---
layout: post
title: SCEP 2012 R2 – Updated ADMX Template for the February Update - KB3041687
date: 2015-03-03 19:00
author: Dave Green
comments: true
categories: [Computers]
---
The revised February update for Microsoft Endpoint Protection products is out ([KB3041687](http://support.microsoft.com/kb/3041687)) and brings with it a couple of changes to registry keys introduced in the first February update.

This update deprecates the <b>DisableGenericReports</b> subkey and adds a new DWORD called <strong>SubmitSamplesConsent </strong>to the following place:
<blockquote>HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft Antimalware\SpyNet</blockquote>
This new key will allow configuration of sample submissions to Microsoft for analysis.

I have added these updates to my ADMX template for SCEP 2012 R2, which can be [downloaded from GitHub](https://github.com/davegreen/miscellaneous/tree/master/CustomADMX).
<h2>Notes from [KB3036437](http://support.microsoft.com/kb/3036437)</h2>
Endpoint Protection may request file samples to be sent to Microsoft for further analysis. By default, Endpoint Protection will always prompt before it sends such samples. There is an option available to send samples automatically. To opt in to automatic sample submission, open the Endpoint Protection UI, click the <b>Settings</b> tab, select the <b>Advanced</b> section, and then click <strong class="uiterm">Send file samples automatically when further analysis is required</strong>.

Administrators can manage automatic sample submission with additional configuration options through WMI, PowerShell, and Group Policy by using the following registry subkeys:
<h3><b>MAPS Configuration</b></h3>
<b>Registry location:</b>
<blockquote>
<div class="reg_path">HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft Antimalware\SpyNet</div></blockquote>
<b>DWORD name:</b> SpyNetReporting
<b>DWORD values:</b>
<ul>
	<li>0 - Off</li>
	<li>1 - Basic Membership</li>
	<li>2 - Advanced Membership</li>
</ul>
<h3><b>Sample Submission</b></h3>
<b>Registry location:</b>
<blockquote>
<div class="reg_path">HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft Antimalware\SpyNet</div></blockquote>
<b>DWORD name:</b> SubmitSamplesConsent
<b>DWORD values:</b>
<ul>
	<li>0 (default) – Automatic sample submission disabled. End-users will always be prompted for samples.</li>
	<li>1 – Most samples will be sent automatically. Files that are likely to contain personal information will still prompt and require additional confirmation.</li>
	<li>2 – All sample submission disabled. Samples will never be sent and end-users will never be prompted.</li>
	<li>3 – All samples will be sent automatically. All files determined to require further analysis will be sent automatically without prompting.</li>
</ul>
