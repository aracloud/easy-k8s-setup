# suppress parallel vm creation
# used only for generic k8s installation uk8sXXX systems
#ENV['VAGRANT_NO_PARALLEL'] = 'yes'

# eth0 is the default ubuntu nic (dhcp enabled, data plane mgmt access)
# eth1 add static default route for the ESXi lab second nic eth1 (control plane)
box_eth1_gateway = "192.168.60.67"
box_dns_local = "192.168.2.6"
#box_dns_resolvd = "8.8.8.8 8.8.4.4"
box_dns_resolvd = "192.168.2.6"

# k8s pod network cidr (master-playbook for native k8s installation)
pod_network_cidr= "10.244.0.0/16"

#os_nodes= "centos/7"
os_nodes= "generic/ubuntu2004"
os_nodes_master_pb= "master-playbook-ubuntu.yml"
os_nodes_worker_pb= "worker-playbook-ubuntu.yml"
os_rke= "generic/ubuntu2004"
os_rke_pb= "rke-playbook.yml"

# RKE vars 
rke_version_vag= "v1.2.4"
rke_install_dir_vag= "/usr/local/bin"
rke_config_file_vag= "rancher-cluster.yml"
rke_cluster_kube_config_vag= "kube_config_rancher-cluster.yml"
rke_cert_mgr_version_vag= "v1.0.4"
rke_rancher_hostname_vag= "rancher.arahome.ml"
rke_rancher_hostip_vag= "192.168.60.120"

# this would not work with multiple masters of course 
# Just use this setup with only one master in masters array !!!
masters = {
   "uk8s1m" => [os_nodes, 2, 3072, 30, os_nodes_master_pb, "192.168.2.121", "192.168.60.121", "00:50:56:aa:a1:aa", "00:50:56:aa:a2:aa" ],
}

workers = {
   "uk8s2w" => [os_nodes, 2, 3072, 30, os_nodes_worker_pb, "192.168.2.122", "192.168.60.122", "00:50:56:aa:b1:aa", "00:50:56:aa:b2:aa" ],
   "uk8s3w" => [os_nodes, 2, 3072, 30, os_nodes_worker_pb, "192.168.2.123", "192.168.60.123", "00:50:56:aa:c1:aa", "00:50:56:aa:c2:aa" ],
}

rkes = {
   "uk8s-rke" => [os_rke, 1, 4096, 30, os_rke_pb, "192.168.2.120", "192.168.60.120", "00:50:56:aa:d1:aa", "00:50:56:aa:d2:aa" ],
}

Vagrant.configure("2") do |config|

  # building master nodes
  masters.each do | (name, cfg) |
    box, numvcpus, memory, storage, playbook, nodeip1, nodeip2, macaddr1, macaddr2 = cfg

    config.vm.define name do |machine|
      machine.vm.box = box
      machine.vm.hostname = name

      machine.vm.provider :vmware_esxi do |esxi|
        esxi.esxi_hostname = 'esxi'
        esxi.esxi_username = 'root'
        esxi.esxi_password = 'file:./esxi_password'
        esxi.esxi_virtual_network = ['VM Network', 'internal-60']
        esxi.esxi_disk_store = 'datastore'
        esxi.guest_memsize = memory
        esxi.guest_numvcpus = numvcpus
        esxi.guest_boot_disk_size = storage
        esxi.guest_mac_address = [ macaddr1, macaddr2 ]
        esxi.guest_guestos = 'ubuntu-64'
        esxi.guest_nic_type = 'vmxnet3'
        esxi.debug = 'false'
        
      end #end of provider

      machine.vm.provision "ansible" do |ansible|
        ansible.playbook = playbook
        ansible.extra_vars = {
                node_ip1: nodeip1,
                node_ip2: nodeip2,
                gateway_ip: box_eth1_gateway,
                dns_ip_local: box_dns_local,
                dns_ip_resolvd: box_dns_resolvd,
                first_master: name,
                pod_net_cidr: pod_network_cidr,
                rke_rancher_hostname: rke_rancher_hostname_vag,
                rke_rancher_hostip: rke_rancher_hostip_vag,
            }
      end #end ansible provision
    end #end of machine
  end #end of each loop

  # building worker nodes
  workers.each do | (name, cfg) |
    box, numvcpus, memory, storage, playbook, nodeip1, nodeip2, macaddr1, macaddr2 = cfg

    config.vm.define name do |machine|
      machine.vm.box = box
      machine.vm.hostname = name

      machine.vm.provider :vmware_esxi do |esxi|
        esxi.esxi_hostname = 'esxi'
        esxi.esxi_username = 'root'
        esxi.esxi_password = 'file:./esxi_password'
        esxi.esxi_virtual_network = ['VM Network', 'internal-60']
        esxi.esxi_disk_store = 'datastore'
        esxi.guest_memsize = memory
        esxi.guest_numvcpus = numvcpus
        esxi.guest_boot_disk_size = storage
        esxi.guest_mac_address = [ macaddr1, macaddr2 ]
        esxi.guest_guestos = 'ubuntu-64'
        esxi.guest_nic_type = 'vmxnet3'
        esxi.debug = 'false'
        
      end #end of provider

      machine.vm.provision "ansible" do |ansible|
        ansible.playbook = playbook
        ansible.extra_vars = {
                node_ip1: nodeip1,
                node_ip2: nodeip2,
                gateway_ip: box_eth1_gateway,
                dns_ip_local: box_dns_local,
                dns_ip_resolvd: box_dns_resolvd,
                rke_rancher_hostname: rke_rancher_hostname_vag,
                rke_rancher_hostip: rke_rancher_hostip_vag,
            }
      end #end ansible provision
    end #end of machine
  end #end of each loop

  # building rke nodes
  rkes.each do | (name, cfg) |
    box, numvcpus, memory, storage, playbook, nodeip1, nodeip2, macaddr1, macaddr2 = cfg

    config.vm.define name do |machine|
      machine.vm.box = box
      machine.vm.hostname = name

      machine.vm.provider :vmware_esxi do |esxi|
        esxi.esxi_hostname = 'esxi'
        esxi.esxi_username = 'root'
        esxi.esxi_password = 'file:./esxi_password'
        esxi.esxi_virtual_network = ['VM Network', 'internal-60']
        esxi.esxi_disk_store = 'datastore'
        esxi.guest_memsize = memory
        esxi.guest_numvcpus = numvcpus
        esxi.guest_boot_disk_size = storage
        esxi.guest_mac_address = [ macaddr1, macaddr2 ]
        esxi.guest_guestos = 'ubuntu-64'
        esxi.guest_nic_type = 'vmxnet3'
        esxi.debug = 'false'
        
      end #end of provider

      machine.vm.provision "ansible" do |ansible|
        #ansible.verbose = "vvv"
        ansible.playbook = playbook
        ansible.extra_vars = {
                node_ip1: nodeip1,
                node_ip2: nodeip2,
                gateway_ip: box_eth1_gateway,
                dns_ip_local: box_dns_local,
                dns_ip_resolvd: box_dns_resolvd,
                rke_version: rke_version_vag,
                rke_install_dir: rke_install_dir_vag,
                rke_cluster_config: rke_config_file_vag,
                rke_cluster_kube_config: rke_cluster_kube_config_vag,
                rke_cert_mgr_version: rke_cert_mgr_version_vag,
                rke_rancher_hostname: rke_rancher_hostname_vag,
                rke_rancher_hostip: rke_rancher_hostip_vag,
            }
      end #end ansible provision
    end #end of machine
  end #end of each loop

end #end of vagrant
