# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-18.04"
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a forwarded port mapping which allows access to a specific port
  config.vm.network "forwarded_port", guest: 1313, host: 1313, host_ip: "127.0.0.1"

  # Share an additional folder to the guest VM.
  config.vm.synced_folder "../vagrant-dev", "/home/vagrant/vagrant-dev",
                            create: true, owner: 'vagrant', group: 'vagrant'

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus   = 1
    # Internet was too slow
    # https://serverfault.com/questions/495914/vagrant-slow-internet-connection-in-guest
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  # Enable Cache feature if you have the cache plugin.
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  # Play ansible
  config.vm.provision "ansible_local" do |ansible|
    ansible.limit    = "localhost"
    ansible.playbook = './ansible/all.yml'
    ansible.inventory_path = './ansible/inventries/hosts'
  end
end
