---
layout: blog
title: HTTPS and SSL Certificates Explained
date: 2016-03-19 22:00:00
categories: hosting
image: "/images/posts/8/bg.png"
category: blog
published: true
comments: true
tags:
- pki
- security
---

*HTTPS and SSL explained in detail, what they are and how they work together.*


<br />
**HTTPS vs HTTP**

If you look at a web URL in your browser bar the first part of the address is the protocol.
You're probably looking at http://.
http is unencrypted, meaning all the data flowing from your browser (client) to the web site (server) can be read if someone were listening to your web traffic using packet anlyzer software
In the case of ecommerce transactions and other sensitive data such as login you'll want to make sure you're using https://.
In fact, https:// was initially devised for the use of online payments. It uses a technology called tls/ssl (transport layer security / secure sockets layer).
TLS is widely used today as SSL is mostly deprecated. But people write them together or often refer to the certificate you need to place on a webserver as a "SSL Certificate"
SSL Certificates are what's used to encrypt the traffic help ensure that an encrypted https connection is connecting to a trustworthy source.


<br />
**SSL Certificate**

An SSL certificate validates the connection is trustworthy. It is part of a system called PKI, or public Key Encryption and uses a standard for digitally signing certificates called X509
An SSL cerificate is created by a user generating a public and private key pair, generating a "cerified signing request" file and submitting it to a cerificate authority.
These certificates are usually leased for a yearly fee from the commercial CA.
For development purposes you can also issue self-signed certificates to yourself using tools like OpenDNS.


<br />
**Certificate Authorities**

A cerificate Authority is a trusted entity that is a 3rd party both the end user and the web site should trust.
Most of the time, in the commecrial web space, certificate authorities are large multinational tech companies like Verisign, Godaddy, or Commodo.
People usually subit their csr (certificate signing request) to a large CA and are given a SSL certificate.
The certificates websites recieve are whats called subordinate certificates. They are generated from root certificates which are self signed by CAs.
The root certificates from these CAs come pre-installed in popular web browsers, which maintain a list of the most well-known CAs
Because of this, SSL certificates properly signed from a trusted CA are automatically accepted when visiting a site using them
This is part of certificate tree, which is referred to as a "chain of trust"

Government and Intranet administrators usually act as their own CAs and usually signed their own root certificates and install them
in the browsers on their networks.

There are a few different types of certificates a CA can issue, each with increasing levels of cost and background checks that apply.
The three most common are
 - Domain validation: CA verifies the domain contact info matches the CSR
 - Organization validation: domain validation plus vetting of the registered organization requesting the certificate
 - Extended Validation: domain validation, organization validation, plus thoughough vetting done manually by a human resource on the organization and its trustworthyness.


<br />
**Obtaining an SSL Certificate**

In order to obtain a SSL certificate from a CA, the CA will ask you to generate a csr.
This can be done with a tool like openssl in one command:

```
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
````

whats happening in this command is the user is using openenssl req command to ask for a certificate request (csr file).
The x509 flag is to specify the previously mentioned x509 standard.
The newkey flag is set since at this point there isn't a user key created.
With this flag the user is specifying the RSA 2048 encryption method (rsa:2048) and the key file name with the keyout flag.
The out flag specifies the csr file name.
You'll then submit this csr and any other appropriate information to your vendor, if you are using one.

If you wanted to self sign you own certificate, you would use a different command:

```
openssl -req x509 -in server.csr -signkey server.key -out server.crt
```

In this example you're specifying the key and csr file , and using the -out flag to specify the name of the certificate file.

If you are interested in using opwnssl to act as your own certificate authority, I will be creating a tutorial on how to do that
at a later date, and I will link it back to here.


<br />
**Installing the Certificate and running https on your server**

Now it's time to use the certificates you've created to. I'll use the simplest possible examble, a Node JS server:

```    
var https = require('https');
var fs = require('fs');

var myKey = fs.readFileSync('mydomain.key');
var myCert = fs.readFileSync('mydomain.crt')

var options = {
    key: myKey,
    cert: myCert
};

https.createServer(options, function (req, res) {
    res.writeHead(200);
    res.end("https works");
}).listen(8000);
```

In nodejs it's as simple as reading the key and certificate files into the server options.

Heres an example of using that same server through apache:

```
<Proxy *>
    Order deny,allow
    Allow from all
</Proxy>
<Location />
    ProxyPass http://localhost:3000/
    ProxyPassReverse http://localhost:3000/
</Location>
```     
     
And your apache ssl config file:

```     
LoadModule ssl_module modules/mod_ssl.so

Listen 443
<VirtualHost *:443>
    ServerName www.example.com
    SSLEngine on
    SSLCertificateFile "/path/to/www.example.com.cert"
    SSLCertificateKeyFile "/path/to/www.example.com.key"
</VirtualHost>
```   
 
<br />
**Putting It All Together**

Now lets take a step back and look at whats happening under the hood:
First a user requests a secure page over the https protocol. When this happens the webserver sends back its public key and its (SSL) certificate. The browser checks to see if the certificate is issued by a trusted CA (either a vendor or internal CA).
The web browser then uses the public key to create and encrypt a random symmetric encryption key and sends it to the web server with encrypted http(s) data.
The web server decrypts the symmetric encryption key using its own private key and uses the nexly decrypted symmetric key to decrypt the encrypted data, which also includes the encrypted URL
The web server sends back the requested data, in the form of an html document, and http data encrypted with the newly decrypted symmetric key.
The web browser decrypts the http(s) data and html document using the original symmetric key and displays the information.
  
<br />
**Wrapping up**

This post does not descript the concepts of encryption/decryption nor what it means to be randomly symmetric. It should be clear to you why you need an SSL certificate if you need to secure data on your webserver. If you can graps the concepts here you can understand how to setup https for your webserver with your own research for your choice of server and application.
