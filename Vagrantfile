# -*- mode: ruby -*-
# vi: set ft=ruby :
# Vagrant.require_version ">= 1.7.0"

Vagrant.configure(2) do |config|
    
    config.vm.box = "ubuntu/trusty64"
    config.vm.hostname = "dokku.me"
    config.vm.network :private_network, ip: "192.168.33.15"
    
    config.ssh.insert_key = false
    
    config.vm.provider :virtualbox do |v|
        v.name = "dokku.me"
        v.memory = 1024
        v.cpus = 2
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
    end
    
    config.vm.provision "ansible" do |ansible|
        ansible.verbose = "v"
        ansible.playbook = "dokku.yml"
    end
end
