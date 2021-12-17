# Docker Compose & Nextcloud <img src="https://xibo.org.uk/img/svg/Home/icon_home_ubuntu_blue.svg" alt="Docker Logo" width='45' align="right"> <img src="https://lh3.googleusercontent.com/proxy/iaZFsbFW-t1es5SFKZ9HDnx7I1Q5tIO-sSS91WTpmNcw9jUSbcQlUq3qoSy0NR2rZxBsudsSih2B71UQKPmQFjyHTlo65Pj80r98kkW2QWc97VL7pwd2umLmdoYW" alt="alfa Logo" height='50' align="right">

**Installation report for an `Ubuntu` installation within `Vagrant` running `Docker` and `Docker Compose` with `NextCloud` and a `MySQL` Database.**

***

<p align="center">
	&bull;
	<a href="#vagrant-installation">Vagrant Installation</a>  
	&bull;
	<a href="#linux-user-configuration">Linux User Configuration</a>
	&bull;
	<a href="#docker-installation">Docker Installation</a>
	&bull;
	<a href="#docker-compose-installation">Docker Compose Installation</a>
	&bull;
	<a href="#nextcloud-installation">Nextcloud Installation</a>
	&bull;
	<a href="#mysql-installation">MySQL Installation</a>
</p>

***

## Vagrant Installation
For the installation we will be needing two things, vagrant itself and a copy of virtualbox or an alternative virtualization program. Here we will be using virtualbox for its out of the box compatability with vagrant and because it is free to download.

Firstly download vitualbox from the following link, make sure to pick the highest possible version.
http://download.virtualbox.org/virtualbox/

Second download Vagrant from Hashicorp, the machine will prompt to restart after the installation has finished.
https://releases.hashicorp.com/vagrant/

After your PC has booted up again we will be making the machine inside of a new folder. Start by making a folder at the desired location on your pc and open this in a terminal with `> cd (path of folder)`. When you have navigated to this folder within a terminal execute the following commands.

Installing Ubuntu for Vagrant\
`> vagrant box add ubuntu/bionic64`

Creating the Ubuntu Machine in Vagrant\
`> vagrant init ubuntu/bionic64`

The machine has now been created, we will do some configuration within the newly created vagrant file to give it a static IP within the private network. Open the newly created vagrantfile in your favorite text editor and replace the content with the following lines. Change the ip if wanted.
```
Vagrant.configure("2") do |config|
   config.vm.box = "ubuntu/bionic64"
   config.vm.network "private_network", ip: "192.168.50.10"
end
```

Starting the newly created Machine\
`> vagrant up`

Connecting to the Machine\
`> vagrant ssh`

## Linux User Configuration
If everything went correctly you should now have a fully operational Ubuntu Machine up and running within a command prompt or in powershell. This part covers automatically login in as root user.

Within the vagrant file add the following lines above the end argument. This will make it so when you use vagrant ssh you will login as root. Note that this solution is not intended to be used for a box that is accessible publicly.
```
config.ssh.username = 'root'
config.ssh.password = 'vagrant'
config.ssh.insert_key = 'true'
```

## Docker Installation

## Docker Compose Installation

## Nextcloud Installation

## MySQL Installation
