---
layout: post
title: Adding Disqus comments to your blog
date: 2015-01-21 19:00:00
categories: tech
image: "/images/posts/2/bg.png"
category: blog
published: true
comments: true
tags:
- analytics
---

##Disqus: an easy way to add comments to your Jekyll blog

**Comments on the blog**

It's important for your blog to have comments.
It allows your visitors and followers to give you feedback on your posts and tutorials.
The comment platform I've chosen is [disqus](http://disqus.com "Disqus Commenting"), to go along with [my newly created Jekyll blog](/tech/2014/01/18/blogging-with-jekyll-and-github.html "Adam Hoke: Blogging with Jekyll and Github.io")


**Using Disqus with Jekyll**

When I was looking for a commenting system to add to my blog,
the first result of my search was Disqus.
After some quick research I decided to go with this solution because I could see it
has support documentation for jekyll integration,
which meant integration would hopefully be quick and painless.

![Disqus Screenshot](/images/posts/2/1.png "disqus.com")

The benefit I've found to using disqus is that it's simple,
responsive, and require's very little code to run.

**Registering at Disqus**

The first step to getting disqus up and running is heading over to disqus.com and registering an account for yourself.
After that head to [http://disqus.com/websites](http://disqus.com/websites "Disqus for Websites") and click "Get Started" and register your website.
Fill out the information, then for implementation options choose "Universal Embed Code".

![Universal Embed Code Screenshot](/images/posts/2/2.png "Adding Disqus Code to your Blog")

The neccesary variables should already be present in your embed code if you're logged into Disqus.

**Changes to Jekyll**

Paste this code into the posts.html file of your '_layouts' folder
you'll want to create some code at the bottom of the file under the content section:

```
    if page.comments
        paste universal embed code here
    endif

```


Important: wrap the 'if' and 'endif' statements in bracket-percent.
You'll see what I'm talking about in the post template file, I just can't paste it here because it won't render.
The code here is part of the [liquid templating engine](http://liquidmarkup.org/ "The Liquid Templating Engine"), which Jekyll uses.

**Conclusion**

Disqus is a great commenting plugin if you want a simple solution for your blog.
You can also change the options in your commenting plugin on disqus.com.
There are other solutions, such as [Gigya](http://gigya.com "Gigya Social Plugin") which are more complex and offer user management.
However I didn't need something like that for this blog.