# -*- mode: ruby -*-
# vi: set ft=ruby :
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Base VM OS configuration.
  config.vm.box = "debian/wheezy64"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 256
    v.cpus = 1
  end

  # Define two VMs with static private IP addresses.
  boxes = [
    { :name => "node1", :ip => "192.168.77.11" },
    { :name => "node2", :ip => "192.168.77.12" },
    { :name => "node3", :ip => "192.168.77.13" }
  ]

  # Provision each of the VMs.
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:ip]

      # Provision both VMs using Ansible after the last VM is booted.
      if opts[:name] == "node3"
        config.vm.provision "ansible" do |ansible|
          ansible.playbook = "cassandra.yml"
          ansible.inventory_path = "envs/cassandra"
          ansible.limit = "all"
        end
      end
    end
  end

end
