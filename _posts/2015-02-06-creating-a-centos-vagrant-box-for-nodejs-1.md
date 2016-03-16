---
layout: post
title: Creating a CentOS Vagrant Box with a NodeJS Server
date: 2015-02-06 17:30:00
categories: tech
featured_image: "/images/posts/5/bg.png"
published: true
comments: true
tags:
- Linux
- virtualization
- nodejs
---

##Creating a Centos Vagrant box with a NodeJS server, Part 1


**Vagrant for Development**

I'll assume if you're reading this you at least know a little about [Vagrant](http://vagrantup.com "Vagrant VM").
It's software that leverages Virtual Machine providers like [Virtualbox](http://www.virtualbox.org "Virtualbox Virtualization Software"),
to create portable development environments.
"Boxes" which are prepackages OS flavors and installed software can be passed to multiple developers on a team,
saving dev environment setup (devops) time, and potentially saving on the cost of development servers.


**Preparing Your Tools**

To follow this article, you need to download / install the following items if you dont already have them:

* VirtualBox
* Vagrant
* An image of [CentOS 6.6](http://isoredirect.centos.org/centos/6/isos/x86_64/ "CentOS 6.6")

You'll need a to access the command line and you'll need a text editor.


**Setting up the Virtual Machine**

Once you've downloaded and installed Virtualbox and Vagrant, and downloaded the CentOS6.6 ISO,
the next step is to create a new virtual machine with these settings:

* Base Memory: 2048MB
* Processors: 2
* Boot Order: CD/DVD, Hard Disk
* Acceleration:
	* VT-x/AMD-V
	* Nested Paging
	* PAE/NX
* Display:
	* 32MB Video Memory
	* 3D Acceleration (though this may change later)
* Network: Intel PRO/1000 MT Desktop (NAT)
* Drive: SATA with 20GB pre-allocated fixed disk
* CD/DVD : IDE Secondary Master Empty
* Disable:
	* USB
	* Audio
	* Shared Folders

![VirtualBox Screenshot](/images/posts/5/1.png "VirtualBox Vagrant settings")


**Adding and configuring the OS**

Start up the newly created virtual machine and choose your CentOS image.
Added your local languages and settings etc., choose "Desktop" server.
This will be nice if you want to do development from within the virtual machine.
Set the root password to vagrant,
and also create another user with the username and password both "vagrant" (this is the default user for the Vagrant software).

The next, and a very important step for any VirtualBox VM, is to install the guest additions.
Usually this is a simple task, but it's complicated.

While logged in as root:


1. Turn on the ethernet interface to access the internet:
```
ifup eth0
```

2. Make sure your software is up to date:
```
yum update
```

3. Install Linux kernal development files, which are needed to get VBox guest additons running:
```
yum install kernel-devel
```

4. Install the "Development Tools" which allow for the Guest Additions to leverage the kernel:
```
yum groupinstall "Development Tools"
```

Once everything installs,
shutdown the virtual machine and then log back in as root once to see that guest additions have installed corrrectly.
Log off of root and log back in as vagrant.
Go to System > Preferences > Network Connections.
Find eth0 and check "Connect Automatically".

Vagrant has an [insecure public keypair](https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub "Vagrant Public Keypair") it uses as the default key to ssh into vagrant boxes.
CD into the .ssh folder of the vagrant directory,
and copy the public key into the authorized_keys file (you'll have to create it if it doesn't exist)
Use chmod to change the permissions for authorized_keys to 600, and the .ssh directory, and vagrant user directory to 700.

```
chmod 600 authorized_keys

chmod 700 .

chmod 700 ..
```

Ensuring the .ssh and vagrant directory both have the correct permission is crucial to vagrant being able to read the key and ssh into the box.
The last step is to use the visudo command to grant all privelegeds to the vagrant user.
Visudo will open the sudoers file for edit,
when it does find the line for the vagrant users permissions and change it to this line:



```
vagrant ALL=(ALL) NOPASSWD: ALL

```


**Adding the FTP Server, Git, Nodejs, and Folders**


For this project we're adding an FTP server to be used alongside the NodeJS server.
CD to the /srv directory and create a folder for the project:
```
cd /srv
mkdir myproject
```

Install vsftp by runnning the following command:
```
yum install vsftpd
```

Next you'll need to modify a setting in selinux to allow the vagrant user to pass through the firewall via ftp into it's own directory:
```
sudo setsebool -P ftp_home_dir 1
```

For convenience, you'll want to set vsftp to run on boot:
```
sudo chkconfig vsftpd on
```

Lastly create a folder on the vagrant home directory called ftp:
```
cd
mkdir ftp
```

Test out the FTP server by opening a browser and navigating to ftp://vagrant@localhost.
You'll be prompted for a password.
Enter your password (vagrant),
and you'll be shown a directory listing of vagrant's home directory:

![Screenshot of vsftp on CentOS](/images/posts/5/2.png "vsftp running on CentOS")

Installing Git and NodeJS are pretty straightforward:
```
yum install git
```

And:


```
yum install nodejs
```

**Package the box**

If you've already downloaded vagrant, the next few steps should be easy.
On you host machine, create a directory for you project.
Inside that directory run the command:
```
vagrant package --base "MyVirtualBox" --output "MyBox"
```

Where "MyVirtualBox" is the name of the Virtual Machine you created (must match exactly) and "MyBox" is the name you want to give your box.
If the box was created successfully, you can now run the command:

```
vagrant init
```

This with initialize the project folder as a vagrant project directory and create a Vagrantfile,
the file used by Vagrant when starting a vagrant box.
Inside the Vagrant file you'll see the configuration vagrant uses to start.
First test the box using the gui, by running vagrant up.
This will start the virtual box you created with a gui, similar to what you'd see if you ran the box through the VirtualBox application
shutdown the box by typing 'vagrant halt' on the host command line.
Now edit the Vagrantfile and under the line that says config.vm.provider "virtualbox" add the following line:

```
vb.customize ["modifyvm", :id, "--accelerate3d", "off"]
```

This will disable the 3d acceleration in VirtualBox, which due to a bug,
prevents ssh through vagrant using CentOS (I had a link to a discussion about this, but unfortunatley I lost it.)
Save the Vagrantfile and run:
```
vagrant reload
```

Followed by:
```
vagrant ssh
```

If the public keypair has been added correctly you should be automatically sshed into the running vagrant box as the vagrant user.

![Vagrant SSH Screenshot](/images/posts/5/3.png "Vagrant SSH")

You can exit and stop vagrant by running 'vagrant halt'.


**What's Next?**


That concludes the first part of this tutorial.
In the next part, I'll add the puppet provisioning scripts neccesary to have the delopment environment up and running,
making a completely portable development environment.