# Homestead

In a nutshell, Homestead is an official, pre-packaged Vagrant “box” that provides you a 
wonderful development environment without requiring you to install PHP, a web server, 
and any other server software on your local machine.

## Prerequisites

```php
vagrant box add laravel/homestead
```

Download and install the following prerequisites: <a href='https://www.virtualbox.org/wiki/Downloads'>virtualbox</a> 
and <a href='https://www.vagrantup.com/downloads.html'>vagrant</a> 

Once installed, add the laravel vagrant box by running in the terminal

`vagrant box add laravel/homestead`

## Installation

```php
cd ~
git clone https://github.com/laravel/homestead.git Homestead
//go to your Homestead directory
cd Homestead
bash init.sh
```

Clone the repository into a Homestead folder within your "home" directory, as the Homestead box will serve as the host to all of your Laravel projects::

`cd ~`

`git clone https://github.com/laravel/homestead.git Homestead`

Once installed, run from the <b>Homestead directory</b> the following command:

`bash init.sh`

<aside class="notice">
This will create the Homestead.yaml configuration file in your ~/.homestead
</aside>


## Configuration

> Homestead.yaml sample (located in ~/.homestead/)

```php
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: /Users/Elie/Desktop/Code
      to: /home/vagrant/Code

sites:
    - map: homestead.app
      to: /home/vagrant/Code/test

    - map: phpmyadmin.app
      to: /home/vagrant/Code/phpmyadmin

databases:
    - homestead
```

A configuration sample of the Homestead.yaml file.
Last thing to do is, to edit the hosts file to add your sites hometead.app and phpmyadmin.app 

`vi /etc/hosts` 

save it and run `vagrant up` from the Homestad directory.

<aside class="notice">
Every time you edit the Homestead.yaml, make sure to run <br/>
`vagrant reload --provision`
</aside>

<aside class="success">
phpmyadmin on homestead credentials <br/>
`homestead secret`
</aside>

