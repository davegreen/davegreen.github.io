---
layout: post
title: SCCM Cache size and client content management
date: 2018-08-09 20:00
author: Dave Green
comments: true
categories: [Computers]
---

Managing client content in SCCM is *usually* a zero touch affair. You set the content size and the data usually manages itself as it ages out over time and makes way for newer content. **However**, sometimes an application come along to break all those assumptions and make you cringe.

I recently had to deploy an application that required me to do just that. Two halves of an application, with the larger piece being a svelte 20GB of install content, even when compressed!

This required me to do two things to ensure the SCCM clients wouldn't run into too many problems, especially if they'd been recently deployed.

One thing was to force clear the client cache before install, to use the minimal amount of space and free up space that may have been used for deployments that day, especially if the machine was freshly deployed, whee this is usually an issue. The second thing was to temporarily increase the cache size for the install with a silent required application, then reset/leave it to the SCCM policy afterward, since these apps are very rare.

For both of these things, I used my trusty friend PowerShell to load the SCCM client management COM object, then expand or clear the client content. Big thanks to both [OzThe2](https://fearthemonkey.co.uk/how-to-change-the-ccmcache-size-using-powershell/) and [kittiah](https://www.reddit.com/r/SCCM/comments/4mx9h9/clean_ccmcache_on_a_regular_schedule/d3z8px0/?context=3) on reddit for the info, near enough just altered things into functions to make my deployment tidier.

Here's the clean cache script merged into the function. For the cache size script, see [here](https://github.com/ozthe2/Powershell/blob/master/SCCM/SetCCMCacheSize):

```powershell
#Requires -RunAsAdministrator

Param (
    [Parameter()]
    [int]
    $OlderThan = 30
)

function Clear-CCMCache {
    <#
        .Synopsis
            Clears the content from the CCMCache.

        .DESCRIPTION
            Clears the content from the CCMCache.
            If no parameters are supplied then the cache is completely cleaned.

            To use this in Configuration Manager, on the Programs tab on the Deployment Type, set the 'Program' to be:

            Powershell -ExecutionPolicy Bypass -File .\Clear-CCMCache.ps1 -OlderThan 30

            The number after -OlderThan reflects the age of the CCM cache content to clear.
            If you leave out the -OlderThan parameter then all cache will be cleared.

        .EXAMPLE
            Clear-CCMCache -OlderThan 2

            Clears the cache data older than 2 days

        .NOTES
            Author: David Green
    #>

    [CmdletBinding()]
    Param (
        [Parameter()]
        [int]
        $OlderThan
    )

    if ($OlderThan) {
        $TargetDate = (Get-Date).AddDays($OlderThan * -1)
    }
    else {
        $TargetDate = Get-Date
    }

    # Create Cache COM object
    $CCM = New-Object -ComObject UIResource.UIResourceMgr
    $CCMCache = $CCM.GetCacheInfo()

    # Enumerate all cache items, remove any not referenced since $targetDate from the CCM Cache
    $CCMCache.GetCacheElements() | ForEach-Object {
        if ($_.LastReferenceTime -lt $TargetDate) {
            $CCMCache.DeleteCacheElement($_.CacheElementID)
        }
    }

    Write-Verbose -Message 'Updating registry.'

    # Create the registry key we will use for checking last run.
    $RegistryKey = 'HKLM:\System\CCMCache'
    New-Item -Path $RegistryKey -Force

    # Add size that CCMCache was changed to in MB
    $Item = @{
        Path         = $RegistryKey
        Name         = 'LastCleared'
        Value        = (Get-Date -Format 'dd/MM/yyyy HH:mm:ss').ToString()
        PropertyType = 'String'
        Force        = $true
    }

    New-ItemProperty @Item

    Write-Verbose -Message 'Done.'
}

# Script entry point
Clear-CCMCache -OlderThan $OlderThan
```