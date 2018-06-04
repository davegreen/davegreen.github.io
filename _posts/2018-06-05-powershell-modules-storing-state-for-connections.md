---
layout: post
title: PowerShell Modules - Storing state for connections
date: 2018-06-05 20:00
author: Dave Green
comments: true
categories: [Computers]
---

Following on from my recent module writing work, i've been using a little trick in some of my PowerShell modules to store a small amount of state that persists as long as the module is loaded. I've been primarily using this strategy as a way to manage connections for SharePoint and REST services.

My example here is a small one derived loosely from a SharePoint Client-Side Object Model (CSOM) module I use and maintain. Essentially, in the module .psm1 file the variable is defined and then used by a couple of functions to set, get and clear the connection data using the `$script:variable` scoping, as shown below:

```powershell
# SharePoint connection states
[System.Diagnostics.CodeAnalysis.SuppressMessage('PSUseDeclaredVarsMoreThanAssigments', '')]
$Connection = New-Object System.Collections.ArrayList

Function Connect-SharePoint {
    [CmdletBinding()]
    Param (
        [Parameter(Mandatory)]
        [PSCredential]
        $Credential,

        [Parameter(Mandatory)]
        [string]
        $Uri,

        [parameter()]
        [switch]
        $PassThru
    )

    # Login
    $SPOContext = New-Object Microsoft.SharePoint.Client.ClientContext($AdminUri)
    $SPOContext.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials(
        $Credential.UserName,
        $Credential.Password
    )

    try {
        $SPOContext.ExecuteQuery()
        $null = $Script:Connection.Add($SPOContext)

        if ($PassThru) {
            $SPOContext
        }
    }
    catch {
        Write-Error "Error connecting to SharePoint."
    }
}

Function Get-SharePointConnection {
    [CmdletBinding()]
    Param (
        [parameter()]
        [string]
        $Site
    )

    if ($Site) {
        $Script:Connection | Where-Object -Property URL -eq -Value $Site
    }
    else {
        $Script:Connection
    }
}

Function Disconnct-SharePoint {
    [CmdletBinding(DefaultParameterSetName = 'Site')]
    Param (
        [Parameter(
            Mandatory,
            ParameterSetName = 'Site'
        )]
        [string]
        $Site,

        [Parameter(
            ParameterSetName = 'All'
        )]
        [switch]
        $All
    )

    if ($PSCmdlet.ParameterSetName -eq 'Site') {
        $Disconnect = Get-SharePointConnection -Site $Site

        if ($Disconnect) {
            $Script:Connection.Remove($Disconnect)
        }
        else {
            throw "No connection for site $Site found."
        }
    }

    if ($PSCmdlet.ParameterSetName -eq 'All') {
        $Script:Connection = New-Object System.Collections.ArrayList
    }
}
```

Anyways, hopefully this helps for managing connections, REST auth tokens and a few other little scenarios. I'd be wary of using this approach too often for things like modifying function or module states, as it's not really the one function for a job (the UNIX way) most PowerShell users might expect.