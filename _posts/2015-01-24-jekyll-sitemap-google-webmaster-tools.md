---
layout: blog
title: Jekyll Sitemaps
date: 2015-01-24 17:30:00
categories: tech
image: "/images/posts/3/bg.png"
category: blog
published: true
comments: true
tags:
- analytics
---

##Using Google Webmaster tools to add a sitemap to your Jekyll blog

**Having a Sitemap**

Having a sitemap,
an XML file that lists the links and secitons in your site,
is important for Search Engine Optimization.
It allows search engines to be feed a list of locations within your site to be crawled.
The four largest search engines (Bing, Google, Yahoo, and Ask) all look for an xml sitemap.
However, having a sitemap doesn't necesarily guarantee your a page will be crawled,
and if a page is crawled, it doesn't neccisarily mean it will be indexed.


**Google Webmaster Tools**

Step one: [sign up for Google Webmaster Tools](http://www.google.com/webmasters "Google Webmaster Tools").
You'll need a Google accont to do this.
Next you'll add a site (which you'll have to verify).
For my site, I added both the non-www and www versions.
Once that's done, set the default site to your non-www version.
![Google Webmasters Dashboard](/images/posts/3/1.png "Google Webmasters Dashboard")
On the left hand side you'll see a menu.
Here you'll be able to click "Crawl",
then click "Sitemap" and click the button in the upper red corner "Add/Test Sitemap"
But before we do that, we're going to add the sitemap to the Jekyll blog.


**Adding the Sitemap to Jekyll**

This is the preferred method for a Jekyll blog that lives on github pages.
First you'll want to open your Gemfile and add the line:
```
gem 'jekyll-sitemap'
```
This will tell the bundle application to include this gem everytime you run Jekyll.
Next you'll add a gems section (if you don't already have it) to your '_config.yaml' file and add add jekyll-sitemap to that, like this:

```
gems: [jekyll-sitemap]
```

(Note: I only have one gem in my gemfile, yours could look different)
Now to test, run 'bundle exec jekyll serve' locally and you should see a generated sitemap.xml in your Jekyll '_sites' directory:

![adamhoke.com sitemap.xml](/images/posts/3/2.png "Github.io generated sitemap.xml")
Now push the code to github.
Within a few minutes (up to 30), your sitemap should be automatically generated.

**Final Thoughts**

Each CMS usually has their own sitemap generating tools.
Its important to have a sitemap that is automatically maintained and accessible for getting your site indexed.
[After setting your blog up](http://adamhoke.com/tech/2015/01/21/adding-disqus-comments-to-jekyll.html "Adding Disqus to a Jekyll Blog"), adding a sitemap is the next logical step.