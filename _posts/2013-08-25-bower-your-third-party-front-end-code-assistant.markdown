---
layout: post
title: "Bower: Your Third-Party Front-End Code Assistant"
date: 2013-07-20 10:36
comments: true
categories: web-development
---
As I've started diving into more client-side heavy web apps, I'm finding myself constantly asking why any given tool was developed. Lots of web technologies can look cool, but what problem is each trying to solve. Bower was one that mystified me, even as I let <a href="http://yeoman.io/">Yeoman</a> go ahead and install it in my latest project. It describes itself as a package manager for the web. As a user of npm, I was aware of various packages and tools that can be installed, but I did not understand what packages Bower could be capable of handling. What problem is Bower trying to solve?

<!-- more -->

It turns out Bower speeds up my work process in a way I didn't really complain about, but now that I've seen the light, I am never going back. For Bower, any third-party code library for the front-end is a package. Backbone, RequireJS, Jasmine, and countless other scripts can all be installed in your project with a simple
<blockquote>bower install</blockquote>
command. If you want to be see what's out there and available, search through available Bower packages with
<blockquote>bower search</blockquote> Feel free to mess around with Bower, and when you're done, simply
<blockquote>bower uninstall</blockquote> any scripts you deem unnecessary.

I never thought the workflow of cloning a git repository into a local project was ever much of a pain, but I will gladly let Bower do all of the heavy lifting for me. I think the ability to quickly uninstall a package is hands-down the best feature. Give Bower a shot even for your smaller projects, and I doubt you'll go back either.