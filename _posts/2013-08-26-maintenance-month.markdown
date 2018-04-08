---
layout: post
title: "Maintenance Month"
date: 2010-05-10 14:38
comments: true
categories: web-development
---
Following Chris Coyier's <a href="http://css-tricks.com/may-is-maintenance-month/">lead</a>, I decided to go back and clean up some parts of the re-design I had neglected.  It took some digging into pages and posts that I barely touched, and that have barely been seen, so I was glad to have the push.

<!-- more -->

Some fixes you'll see:
<ul>
	<li>Images from the Archives: After moving my site from /blog to the root of my domain, a few image links became broken.  I was fearful that I'd have to dig into the meta data of the wordpress install to designate the new image folder, but I had actually linked the images on the site absolutely, a big no-no regardless.  Simple fix, and finally implemented.</li>
	<li>Archive, Post, and Page Layout: I wasn't sure at the time about including the sidebar on all child pages, so I left these layouts in limbo, including the sidebar, but not doing it very well.  The CSS was in place, so a bit more html markup completed the changes.</li>
	<li>Post Navigation: The post navigation had some float issues.  Since every element in the post navigation div was floated, a clearing element needed to be introduced so that the subsequent html didn't run right up into it.  IE7 has a margin-bottom bug with floated elements, so instead of introducing a bottom margin on the post navigation, I merely threw a top margin on the post.</li>
</ul>

Have you started maintenance on your own site this month?  Plan to begin in the coming weeks?  Leave a comment.