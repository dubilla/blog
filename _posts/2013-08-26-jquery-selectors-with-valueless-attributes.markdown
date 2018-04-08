---
layout: post
title: "jQuery Selectors with Valueless Attributes"
date: 2012-07-26 12:06
comments: true
categories: web-development
---
It goes without saying that the possibilities involving jQuery Selectors are vast. I recently needed to select all table cells within a given table that only spanned one row. The start to this seemed easy enough. The not selector would be able to target all td’s with a rowspan, but how could the selector cover all possible rowspans? It might be easy to assume that the values would be small enough.

<!-- more -->

<blockquote>
jQuery(“td:not([rowspan='2']):not([rowspan='3']):not([rowspan='4'])”)
</blockquote>

This selector becomes unwieldy quickly. Not only does the selector look daunting, the code is simply fragile. What happens when a table cell with a rowspan of 5 comes along. It becomes a silly arm’s race. Luckily, jQuery allows attribute selectors without values. So a simple selector such as

<blockquote>
jQuery(“td:not([rowspan])”)
</blockquote>

works beautifully.

This of course makes more sense when you consider attributes that don’t necessarily need values. So selecting all inputs that aren’t disabled can be done like so:

<blockquote>
jQuery(“input:not([disabled])”)
</blockquote>

Any other easy jQuery selector involving attributes without values? Share them below!