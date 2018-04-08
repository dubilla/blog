---
layout: post
title: "Introducing MaxWell"
date: 2013-09-04 11:48
comments: true
categories: web-development
published: true
---
In my last post, Yeoman, Backbone, and a Smarter Client, I discussed diving into the new development-minded technologies that have flooded the JS proverbial toolbox. The first project I put together with said tools is [MaxWell](http://dubilla.github.io/Maxwell/), a single-page web app for graphing your point totals in ESPN's Fantasy MaxPart games (think Pigskin Pick'em and ESPN's march madness game, Tournament Challenge). [Yeoman](http://yeoman.io/) and [the Yeoman Backbone generator](https://github.com/yeoman/generator-backbone) were used to bootstrap the application. [RequireJS](http://requirejs.org/) is used to manage dependencies. The application itself lives inside a [Backbone](http://backbonejs.org/) framework. [Grunt](http://gruntjs.com/) is used to build the webapp and prepare it for deployment. Finally, the app is deployed using git and it lives on [GitHub pages](http://pages.github.com/).

<!-- more -->

All in all, I could not be more pleased with the single-page web app tools I've discovered and workflow I've cultivated. Now that I have the workflow in place, I'm already looking to the next side project, FoursightSquare, to see just how quickly I can spin up a web app, start iterating on it, and deploy quickly with each iteration. You can follow along in the meanwhile on its Github page.

It's important to note that MaxWell is a proof of concept with hard-coded values. Given an API, this web app could exist in its current form, or, in an API's absence, this app could be pushed to ESPN's servers and hooked up with just a little bit of server-side code.