---
layout: post
title: "The Importance of bind() in jQuery"
date: 2010-05-17 12:00
comments: true
categories: web-development
---
The jQuery library has always been held in high appeal for its gradual learning curve and generally quick implementation.  The API is concise and includes such simple event watching nomenclature as show(), hide(), ready(), submit(), focus(), and more.  The one popular event watcher that I kept seeing popping up in script after script was the generic bind() function.  I never fully understood what made implementing the bind function any more desirable than click() or focus() until working on a larger scale javascript project the other day.

<!-- more -->

<em>Any element with two event watchers where one must be expired <strong>requires</strong> the bind function.</em>

Imagine a tabbed user interface wherein the content is loaded via ajax.  If both event watchers are on click, the ajaxing needs to be expired while the tabbed javascript must stay active.  With binding, an event can be passed that can then be expired at the end of the function.  The bind function gives you a greater amount of control over event expiration than any of the jQuery shortcut functions can provide.