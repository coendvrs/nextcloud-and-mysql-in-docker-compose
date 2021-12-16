# Docker Compose & Nextcloud <img src="https://xibo.org.uk/img/svg/Home/icon_home_ubuntu_blue.svg" alt="Docker Logo" width='45' align="right"> <img src="https://lh3.googleusercontent.com/proxy/iaZFsbFW-t1es5SFKZ9HDnx7I1Q5tIO-sSS91WTpmNcw9jUSbcQlUq3qoSy0NR2rZxBsudsSih2B71UQKPmQFjyHTlo65Pj80r98kkW2QWc97VL7pwd2umLmdoYW" alt="alfa Logo" height='50' align="right">

**Installation report for an `Ubuntu` installation within `Vagrant` running `Docker` and `Docker Compose` with `NextCloud` and a `MySQL` Database.**

***

<p align="center">
	&bull;
	<a href="#vagrant-installation">Vagrant Installation</a>  
	&bull;
	<a href="#linux-machine-installation">Linux Machine Preparation</a>
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

After your PC has booted up again we will be making the machine inside of a new folder. Start by making a folder at the desired location on your pc and open this in a terminal with `cd (path of folder)`. When you have navigated to this folder within a terminal execute the following commands.

`> vagrant box add ubuntu/bionic64`
`> vagrant box init ubuntu/bionic64`
`> vagrant up`
`> vagrant ssh`

***

## Linux Machine Installation

***

## Docker Installation

***

## Docker Compose Installation

***

## Nextcloud Installation

***

## MySQL Installation
