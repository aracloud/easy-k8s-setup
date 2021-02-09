## Easy way to setup a Kubernetes (k8s) cluster on ESXi for lab purposes

Goal
====

This setup allows to create a multi-node kubernetes cluster
on an ESXi lab environment. 

Disclaimer
==========

These scripts are only meant to be used for testing purposes 
only ! Even if they strive to create a environement relatively 
representative of a production kubernetes cluster (with notably 
different roles on different nodes and the activation of RBAC), 
the cluster comes with default configuration and with no 
hardening whatsoever !

Prerequisites
=============

* Free ESXi (https://my.vmware.com/en/web/vmware/evalcenter?p=free-esxi7)
* Vagrant (https://www.vagrantup.com/downloads.html)
* Ansible (https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
* Host machine uses linux


### Prep ESXi:
Make ssh available and configure ESXi passwordless access as
described in WWW many times already ;-).
Test with a Linux box first before you move on.

Important:
<br>
I use DHCP on the "VM Network" and configured static IP mappings
for all the uk8sxx hosts I defined in Vagrantfile.

Issues I faced regarding mac addresses in ESXi:
<br>
ESXi uses (as far as I understand) 00:0C:29 as vendor prefix for ESXi.
In order to automate VM's I used 00:50:56 as prefix defined in Vagrantfile.

### Prep Ansible via python:
I installed ansible via pip3 on a linux box:
<br>
$ sudo pip3 install ansible

### Prep Vagrant:

```bash
Install vagrant as describe at Hashicorp!

ESXi plug-in documentation: https://github.com/josenk/vagrant-vmware-esxi

Install plugin: vagrant plugin install vagrant-vmware-esxi

```

### Prep ESXi params based on your needs and bring the k8s cluster alive:

```bash
$ vagrant up |tee log.txt
```

