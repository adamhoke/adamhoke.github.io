---
layout: post
title: Drupal High Availability
date: 2015-02-17 12:03:00
categories: tech
image: "posts/6/bg.png"
category: blog
published: true
comments: true
tags:
- Drupal
- stability
---

##Tips for Creating a High Availablity Drupal Site


**What does it mean to be 'Highly Availablity'?**

Any high availability website,
not just a Drupal site,
will excel in the following four areas:

* _Redundancy:_
	* The ablity of a site to stay up (failover) to the same system running on another location in the event of a partial system outage.
* _Scalability:_
	* The ability of a site to balance its resources or increase it's resources if the amount of traffic or usage load (throughput) increase beyond the capacity of the currently provisoned resources.
* _Performance:_
	* The ability of a site to handle usage and traffic load in efficient manner (speed matters).
* _Managbility:_
	* The ability of a site to do any of the above mentioned things in a convenient fashion with capabilities to monitor the sites vital statistics.


**Coding for Performance**


The first things you should do, which are simple and not-cost,
and will improve performance, are code-related.
First limit the amount of Javascript you use, and move any dom manipulation into CSS.
You also want to limit the amount of external Javascript calls you make.
Though, this is not always feasabile, so if you do use Javascript, load the Javascript after the body and compress both your Javascript and CSS.
In Drupal this can easily be done by using [Drush](https://github.com/drush-ops/drush "Drush Drupal Command-line."),
setting the preprocess css and preprocess js variables to '1'.

```

drush vset preprocess_js 1
drush vset preprocess_css 1
```


You can also remove unused CSS from your stylesheets.

Going deeper into the stack, it's important to write well written PHP code:
making use of PHP's built in functions and writing your own code to be fast and efficient,
[using efficient algorithms (big-o notation)](http://discrete.gr/complexity/ "A gentle Introduction to Algorithm Complexity").
When available, use Drupal's built in  API functions, or write you own to avoid writing lots of boilerplate MySQL code.
When you do write MySQL code, query optimization is essential.
I prefer to hand write SQL using Drupals db query api methods,
As I notice using views with AJAX is always a bit slower.


**Caching**

Drupal has it's own cache built in cache (which is not enabled by default), and cache for both views and panelizer if you use them.
You can also add Memcached, which is a combination of Memcached software running on the server,
an extension module for PHP and a [Drupal community](https://www.drupal.org/project/memcache "Drupal Memcache") module you'll need to install and enable.

Another option is also [Varnish](https://www.varnish-cache.org/ "Varnish Community").
Varnish is server software that acts as a reverse proxy,
which is a proxy server that serves up content of a specific site on behalf of the sites origin.
Usually the origin is unreachable from the outside world.
In order to use varnish, you need the varnish software running on a server, and also a configurable [Drupal Community module](https://www.drupal.org/project/varnish "Varnish Accelrator").



**Modules and Managability**

Another module you'll want to install is [Fast 404](https://www.drupal.org/project/fast_404 "Drupal Fast 404").
If you use Queues, you'll want to check out [Queue UI](https://www.drupal.org/project/queue_ui "Queue UI - Drupal").

Other Drupal modules worth looking at:

* [Argcache](http://drupal.org/project/agrcache)
* [APC](http://drupal.org/project/apc)
* [Boost](http://drupal.org/project/boost)
* [Labjs](http://drupal.org/project/labjs)
* [Performance Hacks](http://drupal.org/project/performance_hacks)
* [Deploy](http://drupal.org/project/deploy)

Additionaly to any community Drupal modules you install, you'll also want to keep your Drupal core updated regularly.


**Scalable and Redundant Architecture**

At the very least you'll want to have seperate servers for database and application code.
Ideally these database and web application tiers would be clustered, meaning theres multiple servers with the data replicated across each.
Each server should also have multiple cores and lots of ram.
For the databases, you'll want to fine tune them as much as possible,
[these tips](http://www.percona.com/blog/2007/11/01/innodb-performance-optimization-basics/ "MySQL Database Optimization") can be used as starting point.
Additionally many large scale web websites use a distributed files system such as Gluster to help with replication of files (in the case of Drupal, these would be uploaded images and other CMS files).

Database and application server clusters should sit behind one or more load balancers.
The top most, or first layer of the optimal architecture stack would site behind a CDN, such as [Akamai](http://www.akamai.com/ "Akamai Cloud CDN").
To help with your CDN configuration, Drupal also has a [CDN module](http://drupal.org/project/cdn "Drupal CDN Module").
Finally you can "offload" some of the traffic by using external auto-scaling services,
such as Amazons3 buckets for images, and Google App engine or AWS Elastic Beanstalk for scripts and services-like code that can be hosted externally (or in iframes).


**What's left?**

In order to manage your site to its fullest potential,
you need to know things like how much traffic you get, when your peak traffic is,
and what parts of your site get the most usage (hit distrobution).
Tools like google analytics, [New Relic](http://newrelic.com "New Relic Application Monitoring"), [Nagios](http://www.nagios.org/ "Nagios Infastructure Monitoring") on the server side, and A/B testing.
In addition to all of this, you'll also need a reliable [development server](http://adamhoke.com/tech/2015/02/06/creating-a-centos-vagrant-box-for-nodejs-1.html) that closely mirrors your production environment.

A couple other resources for Drupal high performance / high scalability I've come across recently:

* [Nathaniel Catchpole's book](http://www.amazon.com/High-Performance-Drupal-Scalable-Designs/dp/144939261X "High Performance Drupal: Fast and Scalable Designs")
* [Pressflow](http://pressflow.org/ "pressflow")
