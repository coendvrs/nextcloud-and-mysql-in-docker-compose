# Nextcloud & MySQL in Docker Compose <img src="https://xibo.org.uk/img/svg/Home/icon_home_ubuntu_blue.svg" alt="Docker Logo" width='45' align="right"> <img src="https://www.healthiport.nl/mediadepot/18124d89e9e46/450/600/alfa-college.png" alt="alfa Logo" height='50' align="right">

**Installation report for an `Ubuntu` installation within `Vagrant` running `Docker` and `Docker Compose` with `NextCloud` and a `MySQL` Database. Also contains the installation of `Git` and `Gitlab`. Made for a school assignment.**

***

<p align="center">
	&bull;
	<a href="#vagrant-installation">Vagrant Installation</a>
	&bull;
	<a href="#docker-installation">Docker Installation</a>
	&bull;
	<a href="#docker-compose-installation">Docker Compose Installation</a>
	&bull;
	<a href="#docker-container-configuration">Docker Container configuration</a>
	&bull;
	<a href="#final-yaml-file">Final YAML File</a>
	&bull;
	<a href="#running-docker-compose">Running Docker-Compose</a>
	&bull;
	<a href="#going-to-the-website">Going to the Website</a>
	&bull;
	<a href="#git-installation">Git Installation</a>
	&bull;
	<a href="#adding-ssh-keys-to-gitlab">Adding SSH-Keys to Gitlab</a>  
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
Now we want to start installing the needed packages to run our environment and we are starting with docker.
First we want to update our package list with `$ sudo apt update` and after that we want to get multiple prerequisite packages `$ sudo apt-get install curl gnupg2 apt-transport-https ca-certificates  software-properties-common` and after that we want to add the GPG keys and docker repositories and finally docker.

```
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt update
$ sudo apt install docker-ce
```

Docker should now be succesfully installed. In order to test it out you can use `$ sudo docker --version`.

## Docker Compose Installation
Next we will be installing docker-compose, a tool that was developed to help define and share multi-container applications. Firstly go to the following link to find out what the latest version is of compose https://docs.docker.com/compose/release-notes/ At the time of writing this is 1.29.2.

Now within your ubuntu machine download compose with the following command and replace x.xx.x with the correct version, in this case 1.29.2. `$ sudo curl -L "https://github.com/docker/compose/releases/download/x.xx.x/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`.

Next apply executable permissions to the binary `$ sudo chmod +x /usr/local/bin/docker-compose` and test if it works with `$ docker-compose --version`

Lastly create a new file called docker-compose.yaml with `$ sudo touch docker-compose.yaml`.\
We are now almost ready to install images for docker.

## Docker Container configuration
Before we start defining services inside of the newly created YAML file we want to create a network so the containers can internally communicate with each other. We do this by creating a docker network with `$ sudo docker network create nextcloud_network`.

Because we want to put Nextcloud and other related services inside of containers, we will define all of them inside of the YAML file and link them together. Now open the YAML file with `$ sudo nano docker-compose.yaml`

Within this file we will be putting two services, these are MySQL and Nextcloud. For Nextcloud to work properly we want to add a database, we could use the SqlLite database Nextcloud provides internally but for the sake of it we will be using MySQL so add it into the YAML file.
```
version: '3'

services:

  db:
    image: mysql
    container_name: nextcloud-mysql
    networks:
      - nextcloud_network
    volumes:
      - db:/var/lib/mysql
      - /etc/localtime:/etc/localtime:ro
    environment:
      - MYSQL_ROOT_PASSWORD=secret
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=mysql
    restart: unless-stopped
```
MySQL's container is called db which is of course short for database, as you can see it's also part of `nextcloud_network` and other than that the code explains it self, again we added the `restart unless-stopped` in order to keep it running. We also added passwords, database names and a user.

And finally we will be configuring the Nextcloud container.
```
app:
	image: nextcloud:latest
	container_name: nextcloud-app
	networks:
		- nextcloud_network
	ports:
		- 1337:80
	depends_on:
		- db
	volumes:
		- nextcloud:/var/www/html
		- ./app/config:/var/www/html/config
		- ./app/custom_apps:/var/www/html/custom_apps
		- ./app/data:/var/www/html/data
		- ./app/themes:/var/www/html/themes
		- /etc/localtime:/etc/localtime:ro
	environment:
		- MYSQL_DATABASE=nextcloud
		- MYSQL_USER=nextcloud
		- MYSQL_PASSWORD=mysql
		- MYSQL_HOST=172.19.0.2:3306
	restart: unless-stopped
```
Nextcloud depends on the database. In order to make the data persistent while upgrading we used a named docker volume `nextcloud` similar to the volume named db for MySQL. Again it will always restart unless manually stopped.

Lastly we want to add volumes for Nextcloud and MySQL for data persistence and we want to add the network.
```
volumes:
  nextcloud:
  db:

networks:
  nextcloud_network:
```


## Final YAML File
If all is done correctly the final YAML file should look something like this:
```
version: '3'

services:

  db:
    image: mysql
    container_name: nextcloud-mysql
    networks:
      - nextcloud_network
    volumes:
      - db:/var/lib/mysql
      - /etc/localtime:/etc/localtime:ro
    environment:
      - MYSQL_ROOT_PASSWORD=secret
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=mysql
    restart: unless-stopped

  app:
    image: nextcloud:latest
    container_name: nextcloud-app
    networks:
      - nextcloud_network
    ports:
      - 1337:80
    depends_on:
      - db
    volumes:
      - nextcloud:/var/www/html
      - ./app/config:/var/www/html/config
      - ./app/custom_apps:/var/www/html/custom_apps
      - ./app/data:/var/www/html/data
      - ./app/themes:/var/www/html/themes
      - /etc/localtime:/etc/localtime:ro
    environment:
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=mysql
      - MYSQL_HOST=172.19.0.2:3306
    restart: unless-stopped

volumes:
  nextcloud:
  db:

networks:
  nextcloud_network:
```

## Running Docker-Compose
Finally we will be running `docker-compose` from the terminal to create all of the containers. This should install MySQL, proxy, encryption and Nextcloud itself. We will do this by running `$ sudo docker-compose up -d`. If all has gone well we will be running `$ sudo docker ps -a` to see all the installed containers. You should see all of our containers here.

Now make sure to run `$sudo docker inspect [MYSQL_CONTAINER_ID]` the container_id can be found with `$ sudo docker ps -a` With inspect you want to find the IP of the MySQL container which is most likely `172.19.0.2`. Now open up the YAML file again with `$ sudo nano docker-compose.yaml` and replace `MYSQL_HOST=0.0.0.0:3306` with `MYSQL_HOST=mysql_ip:3306` after this make sure to run `$ sudo docker-compose up -d` again.

## Going to the Website
If all is done correctly you should now be able to go to `server_ip:1337` in a webbrowser on your host pc. You should see a setup page where you can make an administrator account for Nextcloud. Fill in a username and strong password in the given inputs and press the button. You now have an installation of Nextcloud with MySQL.

## Git Installation
Within our VM we will also be installing Git and going over the basics of working with Git on Gitlab. Before we can work with Git and Gitlab we will have to install Git itself. This can be done with apt install. First update our package list with `sudo apt update` and next install Git with `sudo apt install git`. When the installation has finished we can check this by retrieving the git version with `git --version`

Next we want to make a gitlab account by going to https://gitlab.com/users/sign_up where you can make a gitlab account for free.

## Adding SSH-Keys to Gitlab
As soon as you have made an account on gitlab and have logged in we will be going to `settings` of your account. In here you will find `SSH Keys` in here we want to add our own public key which can be found on our VM. Within our VM we want to show the contents of the .ssh folder in our home directory with `ls -l ~/.ssh/`
