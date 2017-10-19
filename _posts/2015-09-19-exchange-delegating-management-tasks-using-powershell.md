---
layout: post
title: Exchange - Delegating Management tasks using PowerShell
date: 2015-09-19 10:00
author: Dave Green
comments: true
categories: [Computers]
---
As is always the case, things to write about come along all at once. Here's another little administrative thing I had to read up on and piece together, so I'm posting it as a note to forgetful future me!

I had to find out how to delegate a set of Exchange permissions to create and manage mail users, without allowing the same users to remove things. This sounded like a problem uniquely suited to the powers of the Exchange Management Shell.

To manage Exchange permissions like this, you must be a member of the built-in role group '[Organization Management](https://technet.microsoft.com/en-us/library/dd638186)', which will grant you the permission to see and run these cmdlets.

I started by creating an [Exchange role group](https://technet.microsoft.com/en-us/library/dd638105(v=exchg.150).aspx), which is an Active Directory (AD) security group that will provide the new delegated rights set. Role groups can be created using the cmdlet [New-RoleGroup](https://technet.microsoft.com/en-us/library/dd638181(v=exchg.150).aspx). To see an example of an existing role group, the [Get-RoleGroup](https://technet.microsoft.com/en-us/library/dd638115(v=exchg.150).aspx) command will get you started (Output edited for clarity):

    Get-RoleGroup | Select-Object -First 1 | Format-List

    ManagedBy       : {contoso.test/Microsoft Exchange Security Groups/Organization Management}
    RoleAssignments : {Active Directory Permissions-Organization Management-Delegating...}
    Roles           : {Active Directory Permissions, Cmdlet Extension Agents...}
    Members         : {contoso.test/contoso/Admins/Users/TestAdmin, contoso.test/Users/Administrator}
    SamAccountName  : Organization Management
    Description     : Members of this management role group have permissions to manage Exchange objects and their properties in the Exchange organization. Members can also delegate role groups and management roles in the organization. This role group shouldn't be deleted.
    RoleGroupType   : Standard
    Name            : Organization Management

You can create the new role group with a command like this:

    New-RoleGroup -Name "Helpdesk Mail User Management" -samAccountName "Helpdesk Mail User Management" -Description "Members of this role group have the ability to create and manage Exchange user objects."

When you look at the group again using Get-RoleGroup, you'll notice that there are two attributes that are empty compared to the existing groups, 'Roles' and 'RoleAssignments'. These are the attributes that will contain the references to the rights we need our group to have.

Now we have a role group (with no permissions!), we next need to find out and set up the permissions sets, or management roles, we need. Some of this is found out using [documentation (under Management role types)](https://technet.microsoft.com/en-us/library/dd298116(v=exchg.150).aspx#RoleEntries), but a lot of it is working out what PowerShell command (or commands) an action boils down to and using that to scope your management role.

You can discover this by first finding out what roles contain a specific cmdlet the user will need to do their job:

    Get-ManagementRole | Get-ManagementRoleEntry | Where-Object {$_.Name -eq "New-Mailbox"}

The next step is finding out how good a fit that role would be as a base to the custom role by seeing all the cmdlets that role contains. It appears that picking a role with more than you need, then reducing it down is the way to go, as you cannot add entries that don't exist in the parent role:

    Get-ManagementRole "Mail Recipient Creation" | Get-ManagementRoleEntry

Once you've planned the cmdlets you'll need and the roles you can copy and tweak to your satisfaction, you can copy an existing management role:

    Get-ManagementRole "Mail Recipient Creation" | New-ManagementRole "Mail Recipient Creation-NoDelete"

To tweak you new management role, you can use the cmdlets we've already explored, then filter it with Where-Object:

    Get-ManagementRole "Mail Recipient Creation-NoDelete" | Get-ManagementRoleEntry | Where-Object {$_.Name -like "Remove-*"}

Once we've discovered the exact cmdlets we need to remove from our new management role, we can use Remove-ManagementRoleEntry as part of the pipeline to get the job done.

    Get-ManagementRole "Mail Recipient Creation-NoDelete" | Get-ManagementRoleEntry | Where-Object {$_.Name -like "Remove-*"} | Remove-ManagementRoleEntry

Remember not to be too crazy with wildcards, as you don't want to remove the cmdlets you need!

Now we have all our management roles sorted out, we can add our new management roles to the security group, so they'll take effect.

    New-ManagementRoleAssignment -SecurityGroup "Helpdesk Mail User Management" -Role "Mail Recipient Creation-NoDelete"

And that's it! You can add multiple roles to the same group to provide the right permissions set and the process isn't too difficult once you get your head around how it's all supposed to work. I haven't covered scoping here, but there's a lot of information on [TechNet](https://technet.microsoft.com/en-us/library/dd351083(v=exchg.150).aspx) to get you started on [setting scopes on role assignments](https://technet.microsoft.com/en-us/library/dd335096(v=exchg.150).aspx).