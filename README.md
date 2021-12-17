# Docker Compose & Nextcloud <img src="https://xibo.org.uk/img/svg/Home/icon_home_ubuntu_blue.svg" alt="Docker Logo" width='45' align="right"> <img src="https://lh3.googleusercontent.com/proxy/iaZFsbFW-t1es5SFKZ9HDnx7I1Q5tIO-sSS91WTpmNcw9jUSbcQlUq3qoSy0NR2rZxBsudsSih2B71UQKPmQFjyHTlo65Pj80r98kkW2QWc97VL7pwd2umLmdoYW" alt="alfa Logo" height='50' align="right">

**Installation report for an `Ubuntu` installation within `Vagrant` running `Docker` and `Docker Compose` with `NextCloud` and a `MySQL` Database.**

***

<p align="center">
	&bull;
	<a href="#vagrant-installation">Vagrant Installation</a>  
	&bull;
	<a href="#docker-installation">Docker Installation</a>
	&bull;
	<a href="#docker-compose-installation">Docker Compose Installation</a>
	&bull;
	<a href="#nextcloud-installation">Nextcloud Installation</a>
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

The machine has now been created, we will do some configuration within the newly created vagrant file to give it a static IP within the private network. Next open the newly created vagrantfile in your favorite text editor and replace the content with the following lines. Change the ip if wanted.
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

## Docker Installation
Now we want to start installing the needed packages to run our environment and we are starting with docker. In order to not have to use `$ sudo` for every command we are going into superuser mode. Make sure to first set a root password with `$ sudo passwd root` and fill in a new password if it prompts you too. By using the command `$ su -` we will login as root and now we can start installing docker.

First we want to update our package list with `$ apt update` and after that we can install docker with `$ apt -y install docker.io`. I will be using combined commands in order to work more efficiently so for me this command will be `$ apt update && apt -y install docker.io`

Docker should now be succesfully installed. In order to test it out you can use `$ docker --version`

## Docker Compose Installation
Next we will be installing docker-compose, a tool that was developed to help define and share multi-container applications. Firstly go to the following link to find out what the latest version is of compose https://docs.docker.com/compose/release-notes/ At the time of writing this is 1.29.2. 

Now within your ubuntu machine download compose with the following command and replace x.xx.x with the correct version, in this case 1.29.2. `$ curl -L "https://github.com/docker/compose/releases/download/x.xx.x/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

Lastly make the program executable with `$ chmod +x /usr/local/bin/docker-compose` and test if it works with `$ docker-compose --version`
Docker-Compose should now be succesfully installed.

## Nextcloud Installation
Here we will be installing Nextcloud which consists of two parts, the nextcloud container and the nextcloud database. First start by pulling the image of nextcloud with `$ docker pull nextcloud` this should only take a few seconds depending on your hardware. When finished check if its correctly installed by running `docker images` which pulls up all the images installed for docker, it should say something along the lines of `nextcloud | latest | e848004dfd99 | 2 weeks ago | 969MB`


