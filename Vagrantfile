# suppress parallel vm creation
ENV['VAGRANT_NO_PARALLEL'] = 'yes'

# this would not work with multiple masters of course 
# Just use this setup with only one master in masters array !!!
masters = {
   "uk8s1m" => ["bento/ubuntu-20.04", 2, 3072, 30, "master-playbook.yml", "192.168.2.121", "192.168.60.121", "00:50:56:aa:a1:aa", "00:50:56:aa:a2:aa" ],
}

workers = {
   "uk8s2w" => ["bento/ubuntu-20.04", 2, 3072, 30, "worker-playbook.yml", "192.168.2.122", "192.168.60.122", "00:50:56:aa:b1:aa", "00:50:56:aa:b2:aa" ],
   "uk8s3w" => ["bento/ubuntu-20.04", 2, 3072, 30, "worker-playbook.yml", "192.168.2.123", "192.168.60.123", "00:50:56:aa:c1:aa", "00:50:56:aa:c2:aa" ],
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
        esxi.guest_storage = storage
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
                first_master: name,
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
        esxi.guest_storage = storage
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
            }
      end #end ansible provision

    end #end of machine

  end #end of each loop

end #end of vagrant
