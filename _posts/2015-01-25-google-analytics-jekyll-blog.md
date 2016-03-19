---
layout: blog
title: Google Analytics on a Jekyll Blog
date: 2015-01-26 17:30:00
categories: tech
image: "/images/posts/4/bg.png"
category: blog
published: true
comments: true
tags:
- analytics
---

##Adding Google Analytics to a Jekyll blog for visitor tracking


**Visitor Tracking**

Visitor tracking is important to blog owners because it helps them assesss the value of the content they provide.
Metrics that are commonly tracked about visitors are (but not limited to):

* Traffic sources
* Unique visitors vs return visitors
* Time spent on site
* Pages visited / exit page
* Bounce rate


**Google Analytics**

In my opinion, the best tool to use is Google Analytics.
It's free and widely accepted as the de-facto way to prove traffic statistics about a site.
Sign up at [http://www.google.com/analytics](http://www.google.com/analytics "Google Analytics") by using your google account to add your site information.
After you sign up and are logged in, you'll be taken to the dashboard and focused on the page with your tracking code, that'll be added to Jekyll:
![Google Analytics Screenshot](/images/posts/4/1.png "Google Analytics tracking Code")

**Adding GA to your Jekyll Blog**


Adding the GA tracking code to your Jekyll blog is simple.
Copy the tracking javascript code in the "default" template under your '_posts' folder.
This ensures the code will show up on every page.

![Google Analytics Code Screenshot](/images/posts/4/2.png "Google Analytics Tracking Code")

There also may be a delay between when you add the code and push to github and when the admin section of Google Analytics recognizes the tracking code has been added.
The delay can be up to 24 hrs, so you may get a message on the tracking code section of the GA dashboard,
but you can check if it's installed correctly by going to the real time section of reporting and seeing it working.


**Wrappng Up**

The reporting section of Google analytics captures all of the metrics I suggested, and mor including geolocation and technology usage.
There's also integration with other google products such as [Google Webmaster Tools](http://adamhoke.com/tech/2015/01/24/jekyll-sitemap-google-webmaster-tools.html "Adam Hoke: Adding a Sitemap to Your Jekyll Blog")
I'd suggest getting used to the different screens on the dashboard and get a feel for the potential tracking campaigns you can run.