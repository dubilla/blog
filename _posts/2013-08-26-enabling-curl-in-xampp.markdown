---
layout: post
title: "Enabling cURL in XAMPP"
date: 2009-12-12 16:04
comments: true
categories: web-development
---
Implementing cURL in an XAMPP environment for the first time can cause a quick, early headache.  The most common error seen is a simple curl_init() can not be found.  Luckily, the solution is most likely simple.   cURL comes included with Apache but not installed, so you have to go in and turn it on yourself.

Open up your php.ini located under the /php directory.  The line that needs editing will read: ;extension=php_curl.dll.  Simply remove the semi-colon to uncomment the line.  Then restart Apache to finish the change!