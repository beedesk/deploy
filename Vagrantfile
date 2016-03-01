# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    
    config.vm.box = "ubuntu/trusty64"
    config.vm.hostname = "dokku.me"
    config.vm.network :private_network, ip: "10.88.0.8"
    
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
        ansible.limit = "dokku"
        ansible.playbook = "setup_dokku.yml"
        ansible.inventory_path = "test_hosts"
        ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
    end
end
