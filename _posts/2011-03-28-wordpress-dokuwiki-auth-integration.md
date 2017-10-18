---
layout: post
title: Wordpress + DokuWiki Auth Integration
date: 2011-03-28 17:37
author: dave
comments: true
categories: [Computers]
---
I decided on creating a private wiki for some of my ideas/random stuff, so that i could have a private working area i can access from anywhere. I like DokuWiki, so I searched out an <a href="http://keeyai.com/projects-and-releases/dokuwiki-tools/dokuwiki-and-wordpress-3x-authentication-integration/">authentication plugin</a>, so i could use my wordpress login with DokuWiki. The plugin did need a little bit of tweaking to work with Wordpress 3.1 though.

If you comment out/remove:
<blockquote>require( ABSPATH . WPINC . ‘/classes.php’ );</blockquote>
As the file doesnt exist in WP3.1

Then add:
<blockquote>require( ABSPATH . WPINC . ‘/class-wp.php’ );
require( ABSPATH . WPINC . ‘/class-wp-error.php’ );</blockquote>
Tested with WordPress 3.1 and DokuWiki-2010-11-07a. Things appear to work with no issues.

Massive thanks to <a href="http://keeyai.com/about/">Collin Green</a>, plugins like these are really useful, but it's something I wouldn't have bothered to write just for my site (although now I've used it, It might find its way into use with i3Portal when we get that going).
