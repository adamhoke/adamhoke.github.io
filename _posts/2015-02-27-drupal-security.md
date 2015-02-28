---
layout: post
title: Drupal Security
date: 2015-02-27 22:00:00
categories: security
featured_image: "/images/posts/7/bg.png"
published: true
comments: true
---

##Tips for making your Drupal site more secure


**Why Drupal Security**


If you consider Wordpress more of a blogging platform,
as I do,
then Drupal is [the most popular CMS on the planet](http://trends.builtwith.com/cms "Drupal CMS Popularity").
When a CMS, framework or software becomes popular,
that polularity will unfortunatley also paint a target on the back of sites that are built with said software.
This is true for Drupal, as anyone who's ever launched a vanilla install of Drupal can see with in a few days
spammers and hackers creating accounts and trying to poke holes in the site.
In this article I'm going to show you a few ways to harden your Drupal site and prevent attacks.


**Secure Coding**


The first area to focus on, and can apply to most any site is coding securley.
When setting up forms and other widgets that take user input,
just assume all user input is untrustworthy.

Avoid using $\_GET variables directly and sanitize input with the 'htmlspecialchars' php method,
and the Drupal check\_url(), check\_plain(), check\_markup(), and filter\_xss() methods.
Failure to do so leaves you open to XSS attacks,
which are the most popular type of attack on drupal sites and most common type of vulnerability found in contributed Drupal modules.

Another type of attack that's common is SQL injection.
Similar to XSS, SQLi attacks the database layer.
To sanatize input, use prepared statements within Drupals DB abstraction layer API functions.
An example:

```
$search = $_GET['lookup'];
$result = db_query ("SELECT n.title FROM node n WHERE n.title LIKE %:search%"), array(':search' => $search))->fetch();
```

Assuming in this example we might have a url entered that ends with the parameter query ?lookup=worlds (ex: wwww.mysite.com?lookup=worlds).
In the code example we assign the GET paramter to $search, then use a prepared statement (':search' => $search) in the Drupal db\_query method to run execute the DB statement.
db\_query will also run SQLi sanitization against bad characters.

Lastly avoid using the "eval()" statement that can execute native php code.
It should be common sense why this is bad.
Many of these points are also covered in [the Drupal coding standards](https://www.drupal.org/coding-standards "Drupal Coding Standards").


**Authentication and Session Security**

You need to be mindful of authentication and session security issues both in the CMS configuration and in code.
Be sure to "tighten up" views, or in other words make the relationships and filters as optimized as possible
to avoid a site outage from a malicious user constantly hitting a page with poorly optimized view on it.
When using views also avoid giving users access or references to nodes they shouldn't have access to (published=true).
When writing your own modules also be sure to use access arguments and access callbacks if you are using menu hooks,
and never output user session information to the frontend.

```
/* An example Drupal Menu hook
 * Implements hoook_menu
 */

function mymodule_menu() {
  $items = array();

	$items['/stuff/myhook'] = array(
    'title' => 'Look at My Stuff',
		'page callback' => 'mymodule_lookatmystuff_render',
		'access arguments' => array('site administrator', 'another role here'),
		'access callback' => mymodule_lookatmystuff_access',
	);

	return $items;
}

/* Renders page content, called if access callback returns true */
function mymodule_lookatmystuff_render() {
 //whatever
}

/* Returns true if condition met, false if not met */
function mymodule_lookatmystuff_access() {
 //Check some condition
}
```


Apply the principle of least privilege,
both for the permissions set on user roles,
and for the type of text input allowed for editing nodes.
No need to enable html when it isn't neccesary and php should be disabled altogether.
When displaying user information, handle personally identifiable information with care and display it as little as possible.


**Server Security**


For file security,
make sure everything in your Drupal install directry is owned by www-data, which is also the name for the apache process user.
Permissions should generally be set to 644, unless its a batch script or some other script-tool.
You should use seperate stage and production environments, to fully test your changes before deploying.
Stage should be hidden to the outside world,
either behind a firewall or htpasswd. Both of these environments should have your drupal site, and only your drupal site running,
no other php apps should be installed.


On all of your environments, you should:

* Keep your linux, Mysql, and PHP up to date.
* Use up to date ssl certificates and full ssl.
* Avoid plain ftp (sftp only).
* Take backups regulary in case of hack or server crash.


There are other things you can do to your server to make it more "obscure" to potential attacks.
Use a robots.txt to prevent pages that could be vulnerable from being crawled by bots and search engines.
You can also [modify Drupal to hide clues that your site runs drupal](https://www.drupal.org/node/766404 "Drupal Security through Obscurity").
If you use a CDN, some of those also have different types of DDOS protection.


**Modules**


You should keep you core modules up to date to ensure you have the latest security patches.
There are a number of popular Drupal security modules available for download that also help in securing your site:

* [Securepages](https://www.drupal.org/project/securepages "Drupal Secure Pages")
* [Securepages Prevent Hijack](https://www.drupal.org/project/securepages_prevent_hijack "Drupal Prevent Hijack")
* [Securelogin](https://www.drupal.org/project/securelogin "Drupal Secure Login")
* [Security Review](https://www.drupal.org/project/security_review "Drupal Security Review")

These are also some lesser known modules, you may want to explore [here](http://drupalmodules.com/category/Security "Drupal Security Modules").


**Whats Left?**

Drupal developers should always keep up to date with the latest security updates from Drupal,
available on [Drupal.org](https://www.drupal.org/security "Drupal Security Updates").
If you do find a security issue you can report it [here](https://www.drupal.org/security-team/report-issue "Report Security Issue").
Another interesting project I've recently come across is a python based tool to audit and scan multiple CMS, called CMSMap:
[https://github.com/dionach/CMSmap](https://github.com/dionach/CMSmap "CMSMap Scanner").