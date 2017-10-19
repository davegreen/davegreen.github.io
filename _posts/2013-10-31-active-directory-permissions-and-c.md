---
layout: post
title: Active Directory Permissions and C#
date: 2013-10-31 20:25
author: Dave Green
comments: true
categories: [Computers]
---
I've been doing some work recently with C# querying AD for locked out users. One of the requirements for this was to only show users that can be altered by the user running the program.

Fortunately there is a computed AD attribute available for this to do the job, called allowedAttributesEffective. [Here](https://gist.github.com/davegreen/7253114) is some sample code to check a user for attributes you can write to:

https://gist.github.com/davegreen/7253114
