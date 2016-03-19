---
layout: blog
title: Blogging with Jekyll and Github
date: 2015-01-18 19:00:00
categories: tech
category: blog
image: "/images/posts/1/bg.png"
published: true
comments: true
tags:
- analytics
---

##Blogging with github.io, Jekyll, and your own domain name

**Your Own Blog**

Blogs are an important way to market yourself, knowledge and skills.
I won't go into detail because I'll assume that if you're reading this,
You're interested in creating a blog.


**Using Github.io**

The blogging platform being used in this tutorial is [Github](http://github.com "Github").
Normally most wouldn't consider Github a blogging platform,
however Github has set up a domain ["github.io"](http://github.io "github.io") for developers to use for personal or project blogs.
You can have one blog per account, and one blog per project.
We'll be using the account blog.

![Github.io Screenshot](/images/posts/1/1.png "Github.io")

The benefit to using github is the free hosting,
and being hosted on github your blog will never go down unless github itself goes down,
and in that scenario you'll more likely be worried about your inability to push code or work than people reading your blog.
In addition using the [Jekyll](http://jekyllrb.com/ "Jekyll Blogging Platform") platform is simple and allows you to pump out blog posts
without requiring you to mess with a database or use a login:
just have a github account, which if you're reading this you most likely have already.

**Your Username.github.io Repo**

The first step to setting up a Jekyll based blog is to find a Jekyll theme you want to use.
Head over to [Jekyll Themes](http://jekyllthemes.org/ "Jekyll Themes") and find a theme you'd like to use.
For [my blog](http://adamhoke.com "Adam Hoke Blog"), and this tutorial, I decided to use [Strange Case](https://github.com/thephuse/strange_case "Strange Caseon Jekyll Theme")
Go to the themes page on github and [fork the repo](https://help.github.com/articles/fork-a-repo/ "How to Fork on github")  to your own repo.
You'll want to name the repo yourusername.github.io.
Make sure the default branch is Master.

![Fork Repo Screenshot](/images/posts/1/2.png "Forking a Github Repo")

Master should be the default branch because you're creating an account blog.
This is an important point other tutorialsI've seen on github blogging fail to mention.
The gh-pages branch is used by project page blogs.
If the repo you forked doesn't have a master branch we'll create it in the next section.

**Using Jekyll**

Now what you'll want to do is clone your repo to a local dev environment.
This is so you can edit your posts and view them on the localhost server before making them public on Github.
[Install Ruby](https://www.ruby-lang.org/en/documentation/installation/ "The ruby Programming language") if you don't already have it (if you have a Mac, it's there already).
After you've cloned your forked of the theme (yourusername.github.io) to your local dev env,
now's your chance to create the master branch if it didn't exist (git checkout -b master)
The next thing you'll want to do is install a tool called [Ruby Bundler](http://bundler.io/ "Ruby Bundler")
Ruby Bundler is gem manager for Ruby projects.
You can think of it as something similar to pip for python or npm for node.js
After installing Bundler, you'll make use of it by createing a file named simply 'Gemfile'
A Gemfile will contain one or more source declarations and one or more gem declarations to be used in your project.
Here's an example:

```
		source 'https://rubygems.org'
		gem 'github-pages'
```

Run 'bundle install'.
This will install the gem in your gemfile, which is required to host Jekyll on Github.
Now try running the Jekyll server local.
Run the command 'bundle exec jekyll serve'.
Which essentially runs the 'jekyll serve' command but requiring all gems in the gemfile
'jekyll serve' will launch a Jekyll serve on port 4000 of the localhost,
so open a browser and go to http://localhost:4000 and you should see your running Jekyll blog.

![Jekyll Screenshot](/images/posts/1/3.png "Jekyll running on localhost")

Make sure you set the correct config options in the _config.yaml file.
If your going to use a custom domain, which I did, set base url to nothing.
New posts are created by dropiing a readme file in the _posts directoy.
Name a post with this convention 'YYY-MM-DD-post-name.md'
You will need to add important metadata at the top of each post file.
Here's an example, the metadata for this post:

```
		---
		layout: post
		title: Blogging with Jekyll and Github
		date: 2014-01-12 20:00:00
		categories: tech
		featured_image: "/images/posts/1/bg.png"
		published: true
		---
```
The rest is just adding text in [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet "Markdown Cheatsheet").

**Custom Domain with Your Blog**

I'm using my own domain to point to my blog.
I use [Godaddy](http://godaddy.com "Godaddy") as a registrar,
so I create a file called 'CNAME' in my repo and add my domain to it
```
adamhoke.com
```

I log into godaddy and change my @ record to point to either 192.30.252.153 or 192.30.252.154
Make sure to change your project website url of your username.github.com repo to point to your domain name.
Now push all of your changes to Github.
It could take up to 30 minutes for your content to show up.