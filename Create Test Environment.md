On MacOS X

1. Download and Install Vagrant
    a. Create a new folder << vagrant folder >> (such as `ubuntu-docker`)
    b. `cd << vagrant folder >>`
    c. `vagrant init ubuntu/trusty64`
    d. `vi VagrantFile` 
    e. Pick an IP address for Vagrant (such as 10.88.0.8)
    f. change line `config.vm.network "private_network", ip: "<< ip address >>"`
    g. vagrant up
  
2. Install Docker and Dokku 
    a. `vagrant ssh`
    b. `apt-get update`
    c. `apt-get install docker`
    d. Install dokku: (needed to run it twice last time, maybe a transient problem)
        - wget -qO- https://raw.github.com/progrium/dokku/v0.3.12/bootstrap.sh | sudo DOKKU_TAG=v0.3.12 bash
    e. Edit /etc/hosts to add line:
        - << ip address >>   dokku.me
    f. `cat ~/.ssh/id_rsa.pub | vagrant ssh dokku.me "sudo sshcommand acl-add dokku $USER"`

3. Create a project and push