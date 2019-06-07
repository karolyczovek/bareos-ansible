# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_IMAGE = "bento/ubuntu-18.04"
NODE_COUNT = 4


Vagrant.configure("2") do |config|
  config.vm.synced_folder "../data", "/vagrant_data"
  (1..NODE_COUNT).each do |i|
    config.vm.define "node#{i}" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.hostname = "node#{i}"
      if i == NODE_COUNT
        subconfig.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "site.yml"
          extra_vars = "storage_on_nfs: false"
          ansible.groups = {
            "director" => ["node1"],
            "storage" => ["node2"],
            "clients" => ["node3","node4"]
           }
        end
      end
    end
  end

end
