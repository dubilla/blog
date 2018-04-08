---
layout: post
title: "Creating a New Post for Octopress from Zsh"
date: 2014-01-31 00:17
comments: true
categories: web-development
published: true
---
A couple of years back, I made the switch from bash to zsh. I did so mainly because I saw [a fantastic post]("http://code.tutsplus.com/tutorials/how-to-customize-your-command-prompt--net-24083", "Customizing Your Command Prompt on Nettuts") on customizing the command prompt that I dove into head first, and I've stuck with it for the slightly improved tab completion. Your mileage may vary with regards to zsh, but I always find it difficult to do without it when I end up working on somebody else's machine. Despite the improvement, there are a few differences between the shells, and I've come across a scipt or two that wasn't especially zsh friendly. One of those is the very rake task that created this blog post.

<!-- more -->

To create a post in Octopress from bash, you simply need to run the command:
> rake new_post["One man forgot to account for zsh..."]

Running the same command from zsh leaves you with a cryptic error:
> zsh: no matches found: new_post[One man forgot to account for zsh...]

Zsh escapes the quotes necessary to name the new blog post, as you can see by the error that is output. The syntax that creates a blog post in bash causes zsh to begin attempting to match a filename.

To create a post in Octopress from zsh, you simply need to run the command:
> rake "new_post[One man forgot to account for zsh...]"

It's a minor inconvenience, and one that could probably be solved by some extended documentation on the Octopress site. No harm, no foul; zsh Octopress bloggers just need to remember the change in syntax.

Any other zsh tips for Octopress? Or any noteworthy zsh "workarounds" to share? Drop them in comments.
