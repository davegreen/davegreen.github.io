---
layout: post
title: Working with Workday - The SOAP Opera (APIra?)
date: 2018-08-10 20:00
author: Dave Green
comments: true
categories: [Computers]
---

It's been a couple of months and I'm back with new learning from the ever busy front where i'm at war with my own ignorance!

I've been having a look recently at quite a few web APIs (REST and SOAP and all that) and I'm here to share some slightly whacky odd script for getting Workday data from their SOAP API. For those that don't know, Workday is a SaaS Human Capital Management (HCM) system with some grand ambitions around employee lifecycles, payroll, expenses and all those buzzwords and things you need to do when you employ people, pretty cool.

So looking at the [Workday API](https://community.workday.com/api) pages, you might think SOAP? that's old school!, use REST! For Workday, fortunately there is a REST API that's easy to get to grips with, however it's very difficult to get the security right and automate the login/authorization piece (at least with PowerShell, or my knowledge thereof). This means I've been concentrating on the SOAP API, which seems harder to set up the query, but a bit more usable from my position and security standpoint.

OK, so **New-WebServiceProxy** to the rescue? I wish. For some reason I cannot get it to authenticate requests, so I had to fall back on good old **Invoke-WebRequest** for this one! Essentially, I took a bunch of syntax for queries that already worked and amalgamated them in one string replacement hackfest. It's not the cleanest piece of work in the world, but it gets the job done and allows me to keep trying **New-WebServiceProxy** in the background until I get that working. Any suggestions? Please let me know!

Here's the functions:

```powershell
function GetWDWorkerData {
    <#
        .Synopsis
        A function to construct and send a SOAP request to Workday to retrieve worker data from the Workday API.

        .Description
        A function to construct and send a SOAP request to Workday to retrieve worker data from the Workday API.

        .Parameter Credential
        The credentials required to access the Workday API.

        .Parameter RequestType
        The type of data you would like to retrieve. Defaults to returning all data.

        RequestType can be one of the following:

            Contact
            Employment
            Management
            Organization
            Photo

        .Parameter WorkerID
        Limit your search data to a single worker. If your request encompasses multiple workers, use the pipeline.

        By default all worker information is returned.

        .Parameter Tenant

        The tenant to query for information.

        .Example
        GetWDWorker -Credential (Get-Credential) -Tenant TENANT -RequestType Contact

        Get contact data about all workers from the TENANT environment.

        .Notes
        Author: David Green
    #>
    [CmdletBinding()]
    Param (
        [Parameter(Mandatory)]
        [string]
        $Tenant,

        [Parameter(Mandatory)]
        [pscredential]
        $Credential,

        [Parameter(Mandatory)]
        [ValidateSet(
            'Contact',
            'Employment',
            'Management',
            'Organization',
            'Photo'
        )]
        [string[]]
        $RequestType,

        [Parameter()]
        [ValidateScript( {
                $_ -lt 1000 -and $_ -gt 0
            })]
        [int]
        $RecordsPerPage = 750,

        [Parameter(ValueFromPipeline)]
        [string]
        $WorkerID
    )

    Process {
        $page = 0

        do {
            $page++
            $Query = @{
                Uri             = "https://wd3-services1.myworkday.com/ccx/service/$Tenant/Human_Resources/v30.2"
                Method          = 'POST'
                UseBasicParsing = $true
                Body            = @"
<?xml version="1.0" encoding="utf-8"?>
<env:Envelope
    xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd">
    <env:Header>
        <wsse:Security env:mustUnderstand="1">
            <wsse:UsernameToken>
                <wsse:Username>$($Credential.UserName)</wsse:Username>
                <wsse:Password
                    Type="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0#PasswordText">$($Credential.GetNetworkCredential().Password)</wsse:Password>
            </wsse:UsernameToken>
        </wsse:Security>
    </env:Header>
    <env:Body>
        <wd:Get_Workers_Request xmlns:wd="urn:com.workday/bsvc" wd:version="v30.2">
       $(if ($WorkerID) { @"
    <wd:Request_References wd:Skip_Non_Existing_Instances="true">
                <wd:Worker_Reference>
                    <wd:ID wd:type="Employee_ID">$($WorkerID)</wd:ID>
                </wd:Worker_Reference>
            </wd:Request_References>
"@ })
            <wd:Response_Filter>
                <wd:Page>$($page)</wd:Page>
                <wd:Count>$($RecordsPerPage)</wd:Count>
            </wd:Response_Filter>
            <wd:Response_Group>
                $(switch ($RequestType) {
                    'Contact' { "<wd:Include_Personal_Information>true</wd:Include_Personal_Information>" }
                    'Employment' { "<wd:Include_Employment_Information>true</wd:Include_Employment_Information>" }
                    'Management' { "<wd:Include_Management_Chain_Data>true</wd:Include_Management_Chain_Data>" }
                    'Organization' { "<wd:Include_Organizations>true</wd:Include_Organizations>" }
                    'Photo' { "<wd:Include_Photo>true</wd:Include_Photo>" }
                })
            </wd:Response_Group>
        </wd:Get_Workers_Request>
    </env:Body>
</env:Envelope>
"@
                ContentType     = 'text/xml; charset=utf-8'
            }

            [xml]$xmlresponse = Invoke-WebRequest @Query
            Write-Verbose -Message $Query.Uri

            if ($xmlresponse) {
                $xmlresponse
                $ResultStatus = $xmlresponse.Envelope.Body.Get_Workers_Response.Response_Results
                [int]$Records += [int]$ResultStatus.Page_Results
                Write-Verbose -Message "$Records/$($ResultStatus.Total_Results) records retrieved."
                Write-Verbose -Message "$($ResultStatus.Page_Results) records this page ($($ResultStatus.Page)/$($ResultStatus.Total_Pages))."
                $TotalPages = $ResultStatus.Total_Pages
            }
        }
        while ($page -lt $TotalPages)
    }
}
```