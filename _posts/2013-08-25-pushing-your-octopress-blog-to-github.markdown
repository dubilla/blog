---
layout: post
title: "Pushing Your Octopress Blog to Github"
date: 2013-08-25 14:12
comments: true
categories: web-development
published: false
---
I have just gotten started using the Octopress framework for blogging, and I love it. The setup process is unbelievably well-documented, and if you're familiar with Git and Ruby projects, you'll pick it up fairly quickly. The only issue I had early on was pushing my new blog up to Github for version control. It's a little tricky since the first step is cloning from Brandon Mathis' Octopress repo, but the fix is relatively simple.

<!-- more -->
Once you've clone the Octopress project, thrown up a few posts, and tidied up any configuration you want, you're going to want to get the repo into version control using Git. If you're like me, you're also going to want to push it up to Github. I was surprised upon first trying to do this, that my repo was already set up with a remote named origin. Of course, it was; I had cloned from the Octopress repo. I went ahead and pulled a quick switcheroo before add the remote Github origin.

> git rename remote clone
> git add remote origin [github url]

It feels a little hacky, but it got the trick done.

Pretty fitting that I'm writing about using Octopress as my first post written on the service, huh?