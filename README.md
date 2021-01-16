# ISEMS Ansible

This repository contains [ansible](https://www.ansible.com) configuration management code for easier deployment of the ISEMS app(s).

The idea is that you use these scripts to install the [isems-app](https://github.com/isems/isems-app) (the static page app that is used to display data) and the [isems-data-collector](https://github.com/isems/isems-data-collector) (the python application that collects data and makes it available as an api) on a raspberry pi in your network.

## Prerequisites
This tutorial assumes that you are installing the application on a rasperry pi so there should be a user called `pi` on the machine that we are installing to.

Ansible works by sshing into the raspberry pi so you need to make sure that the user on the machine that you are running the ansible commands with can ssh into the raspberry pi as the pi user. To do so add your `~/.ssh/id_rsa.pub` to the raspberry pi's `/home/pi/.ssh/authorized_keys`.

The ansible scripts will install some software as root, using sudo. Make sure that your `pi` user can become root without password input. On the rasperry pi run `visudo` on it and add this line at the end of the file`pi ALL=(ALL) NOPASSWD: ALL`.


### Checklist
- [ ] User `pi` exists on raspberry pi
- [ ] Local user can ssh into raspberry pi as pi user
- [ ] `pi` user can become `root` using `sudo` without password


## Configuration
The configuration of the application is slightly different for every installation. You will need to make some cofiguration changes.

1. Edit the `hosts` file at the root of this repository and put the IP of the raspberry pi there. It could for example look like this:
```
[isems-raspi]
2001:bc8:600:11c::1    ansible_user=pi ansible_ssh_user=pi ansible_python_interpreter=/usr/bin/python3
```
2. Edit the `node_ips` section in `roles/flask/vars/main.yml`. It should contain a list of all the isems node IPs that the data-collector should collect data from. 


## Installation
To install the required dependencies, make sure you are inside a [virtual environment](https://realpython.com/python-virtual-environments-a-primer/) and run: 
```
pip install ansible
ansible-galaxy install -r requirements.yml
```


## Deployment
To deploy the application run the following command:
```
 ansible-playbook isems.yml -i hosts
```
