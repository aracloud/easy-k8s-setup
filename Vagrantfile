# suppress parallel vm creation
ENV['VAGRANT_NO_PARALLEL'] = 'yes'

masters = {
   "uk8s1" => ["generic/ubuntu1804", 2, 3072, 30, "master-playbook.yml", "192.168.2.121", "00:0c:29:aa:aa:aa" ],
}

workers = {
   "uk8s2" => ["generic/ubuntu1804", 2, 3072, 30, "worker-playbook.yml", "192.168.2.122", "00:0c:29:bb:bb:bb" ],
   "uk8s3" => ["generic/ubuntu1804", 2, 3072, 30, "worker-playbook.yml", "192.168.2.123", "00:0c:29:bb:ab:bb" ],
}

Vagrant.configure("2") do |config|

  # building master nodes
  masters.each do | (name, cfg) |
    box, numvcpus, memory, storage, playbook, nodeip, macaddr = cfg

    config.vm.define name do |machine|
      machine.vm.box = box
      machine.vm.hostname = name

      machine.vm.provider :vmware_esxi do |esxi|
        esxi.esxi_hostname = 'esxi'
        esxi.esxi_username = 'root'
        esxi.esxi_password = ''
        esxi.esxi_virtual_network = ['VM Network', 'internal-60']
        esxi.esxi_disk_store = 'datastore'
        esxi.guest_memsize = memory
        esxi.guest_numvcpus = numvcpus
        esxi.guest_storage = storage
        esxi.guest_mac_address = macaddr
        esxi.guest_guestos = 'ubuntu-64'
        esxi.guest_nic_type = 'vmxnet3'
        esxi.debug = 'false'
        
      end #end of provider

      machine.vm.provision "ansible" do |ansible|
        ansible.playbook = playbook
        ansible.extra_vars = {
                node_ip: nodeip,
            }
      end

    end #end of machine

  end #end of each loop

  # building worker nodes
  workers.each do | (name, cfg) |
    box, numvcpus, memory, storage, playbook, nodeip = cfg

    config.vm.define name do |machine|
      machine.vm.box = box
      machine.vm.hostname = name

      machine.vm.provider :vmware_esxi do |esxi|
        esxi.esxi_hostname = 'esxi'
        esxi.esxi_username = 'root'
        esxi.esxi_password = ''
        esxi.esxi_virtual_network = ['VM Network', 'internal-60']
        esxi.esxi_disk_store = 'datastore'
        esxi.guest_memsize = memory
        esxi.guest_numvcpus = numvcpus
        esxi.guest_storage = storage
        esxi.guest_mac_address = macaddr
        esxi.guest_guestos = 'ubuntu-64'
        esxi.guest_nic_type = 'vmxnet3'
        esxi.debug = 'false'
        
      end #end of provider

      machine.vm.provision "ansible" do |ansible|
        ansible.playbook = playbook
        ansible.extra_vars = {
                node_ip: nodeip,
            }
      end

    end #end of machine

  end #end of each loop

end #end of vagrant
