## Easy way to setup a Kubernetes (k8s) cluster on ESXi for lab purposes

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

