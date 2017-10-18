---
layout: post
title: Setting AD permissions for GALSync through Microsoft Identity Manager
date: 2016-05-31 23:08
author: dave
comments: true
categories: [Computers]
---
A quick function for setting permissions in Active Directory to allow the Microsoft Identity Manager Global Address List (GAL) sync Management Agent to do its thing. I put it in a <a href="https://gist.github.com/davegreen/368303ff31ed7c0036d6e53debebccca">GitHub gist</a> so it might help someone else!

It's also a good intro to look at for getting into dealing with Access Control Lists (ACLs) in AD, as it deals with setting <a href="https://msdn.microsoft.com/en-us/library/ms679424.aspx">specific permissions</a> or <a href="https://msdn.microsoft.com/en-us/library/ms683985.aspx">extended rights</a>. Anyways, here's the script!

https://gist.github.com/davegreen/368303ff31ed7c0036d6e53debebccca
