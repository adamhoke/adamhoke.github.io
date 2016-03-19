---
layout: blog
title: HTTPS and SSL Certificates Explained
date: 2016-03-20 22:00:00
categories: hosting
image: "/images/posts/7/bg.png"
category: blog
published: true
comments: true
tags:
- pki
- security
---

##HTTPS and SSL explained in detail, what they are and how they work together.


**HTTPS vs HTTP**

-http unencrypted
-https encrypted
-what encrypted mean
-why you need an https connection
= https verifies legitimate trustworthy servers and publishers by using ssl certificates

**SSL Certificate**

- CERTificate verifies authenticity of domain and server
- created using a standard x509
- user generates keys
- public and private key
- generates a signed certificate request
- user than can self sign certificates for development or use a certificate authority
http://www.tldp.org/HOWTO/SSL-Certificates-HOWTO/x64.html

**Certificate Authorities**

- a trusted authority to digitally sign certificates
- can be a large corporation like verisign or godady
- these com preinstalled on major browsers
- intranet and governemtn usually create their own certificate authiorities
- those groups install in browsers
- digital signature loooks like (show example)
- certificates in browsers are usually root certificates
- certificates issued by cas are usually subordinate or intermediate certificates
- certificate tree and chain of trust
https://www.entrust.com/chain-certificates/

**Obtaining an SSL Certificate**

- steps using openssl to self sign
- steps to get a subordinate ssl from a ca vendor
- steps to create your own Certificate authority


**Installing the Certificate and running https on your server**

- example using node
- some example using apache

**Public Key Infastructure (PKI)**

- TODO find some good sources and an image, explain the image, use a new anaology


**Wrapping up**
- singler verification vs multi (client and server) verification