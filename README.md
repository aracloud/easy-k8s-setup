## Easy way to setup a Kubernetes (k8s) cluster on ESXi for lab purposes

Prep Vagrant:

```bash
Install vagrant as describe at Hashicorp!

ESXi plug-in used by: https://github.com/josenk/vagrant-vmware-esxi

Install: vagrant plugin install vagrant-vmware-esxi

```

Prep ESXi params based on your needs and bring the k8s cluster alive:


```bash
$ vagrant up |tee log.txt
```

