# Boostrapping and securing an Ubuntu server 

This repository contains [Ansible](http://ansible.com) scripts for bootstrapping and securing an Ubuntu server.
Scripts have been tested on Ubuntu 14.04 hosted on [RunAbove](http://www.runabove.com) and [DigitalOcean](http://www.digitalocean.com).

The included tasks are following:

* Update and upgrade Ubuntu packages via apt-get
* Configure locale
* Install vim and mc (my personal preference)
* Install fail2ban to block ssh brute-force attempts
* Delete root password
* Lock down sudo
* Lock down ssh to prevent root and password login
* Setup the ufw firewall
* Configure unattended security upgrades
* Install collectd deamon and collect-web front-end client (optionally) 
* Create users (optionally)

## Ansible

First of all, install the latest version of Ansible, in Ubuntu:

```
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
```

## Clone scripts

Next, you have to clone this repository:

```
$ git clone https://github.com/zenzire/ansible-bootstrap-ubuntu.git
```

## Configuration files

Copy sample configuration files:

```
$ cp hosts.sample hosts
$ cp global_vars/server.yml.sample global_vars/server.yml
```

Edit configuration files (hosts and global_vars/server.yml) with your own configuration.

## Prerequisites for RunAbove hosting

We need to set password for admin user and assing him to sudoers group.

```
$ ansible-playbook user.yml

Enter username: admin
Enter password: 
confirm Enter password: 
Enter id_rsa.pub path [~/.ssh/id_rsa.pub]: 
Add user to sudoers group (y/n) [n]: y
```

## Prerequisites for DigitalOcean hosting

We need to create admin user and assing him to sudoers group.

```
$ ansible-playbook user.yml --user root

Enter username: admin
Enter password: 
confirm Enter password: 
Enter id_rsa.pub path [~/.ssh/id_rsa.pub]: 
Add user to sudoers group (y/n) [n]: y
```


## Script execution

Finally, execute bootstrap Ansible task for admin user:

```
ansible-playbook bootstrap.yml --ask-sudo
```

## Collectd

Also you can install [collectd](https://collectd.org/), deamon which collects system performance statistics 
periodically and [collectd-web](https://github.com/httpdss/collectd-web), web-based front-end for data 
collected by collectd.

```
$ ansible-playbook collectd.yml --ask-sudo
```

## Users

You can add new user or update the existing one using the following script:

```
$ ansible-playbook user.yml --ask-sudo
```


## License

Released under the MIT License, Copyright (c) 2015 - Marcin Mierzejewski

