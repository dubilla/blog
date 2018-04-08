---
layout: post
title: "HTML5 and Media Queries"
date: 2011-07-29 22:50
comments: true
categories: web-development
---
I finally took it upon myself to tackle HTML5, and you are looking at the results.  Of course, implementing any form of HTML5 isn't necessarily rewriting all of your markup with fancy new &lt;section&gt; and &lt;nav&gt; tags nor does it have to be adding HTML5 native Javascript.  The first step is the simplest, but it's a first step nonetheless.  I started by simply replacing my doctype with &lt;! DOCTYPE HTML5&gt;.  Gorgeous in its simplicity, no?  This change alone will give you HTML5  features in all modern browsers<sup>1</sup>.

<!-- more -->

The first step was so easy, I decided to take some more.  I ripped out the header and footer of my Wordpress template and filled it in with some of <a title="Paul Irish" href="http://paulirish.com/" target="_blank">Paul Irish</a>'s <a title="HTML5 Boilerplate" href="http://html5boilerplate.com/" target="_blank">HTML5 Boilerplate</a>.  After, fighting off the fright and intimidation, I was able to follow along with the comments and decide what was needed from the boilerplate, what I wanted, and what I could toss away.  There's a corresponding CSS file with fantastic default styles and even some placeholders for media queries.  Which, in fact, is where I tidied up my work.  Check out this site on webkit-enabled mobile device (iPhone, Android, etc), and you should see a nice, readable mobile layout.  It's by no means perfect, but I would consider the site mobile-enhanced.

<div class="references">
<h5>References</h5>
<ol>
	<li>John Resig, HTML5 DOCTYPE, <a title="http://ejohn.org/blog/html5-doctype/" href="http://ejohn.org/blog/html5-doctype/" target="_blank">http://ejohn.org/blog/html5-doctype/</a>
</li>
</ol>
</div>