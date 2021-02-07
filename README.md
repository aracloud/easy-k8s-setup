## Easy way to setup a Kubernetes (k8s) cluster on ESXi for lab purposes

### Prep ESXi:
Make ssh available and configure ESXi passwordless access as
described in WWW many times already ;-).

Important:
<br>
I use DHCP on the "VM Network" and configured static IP mappings
for all the uk8sxx hosts I defined in Vagrantfile.

Issues I faced:
<br>
ESXi uses (as far as I understand) 00:0C:29 as vendor mac for ESXi.
In order to automate VM's I used 00:50:56 as defined in Vagrantfile.

### Prep Vagrant:

```bash
Install vagrant as describe at Hashicorp!

ESXi plug-in idocumentation: https://github.com/josenk/vagrant-vmware-esxi

Install plugin: vagrant plugin install vagrant-vmware-esxi

```

### Prep ESXi params based on your needs and bring the k8s cluster alive:

```bash
$ vagrant up |tee log.txt
```

