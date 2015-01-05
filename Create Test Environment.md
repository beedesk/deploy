On MacOS X

1. Download and Install Vagrant
    1. a. Create a new folder << vagrant folder >> (such as `ubuntu-docker`)
    2. `cd << vagrant folder >>`
    3. `vagrant init ubuntu/trusty64`
    4. `vi VagrantFile` 
    5. Pick an IP address for Vagrant (such as 10.88.0.8)
    6. change line `config.vm.network "private_network", ip: "<< ip address >>"`
    7. `vagrant up`
  
2. Install Docker and Dokku 
    1. `vagrant ssh`
    2. `apt-get update`
    3. `apt-get install docker`
    4. Install dokku: (needed to run it twice last time, maybe a transient problem)
        * wget -qO- https://raw.github.com/progrium/dokku/v0.3.12/bootstrap.sh | sudo DOKKU_TAG=v0.3.12 bash
    5. Exit vagrant ssh shell
    6. Edit `/etc/hosts` to add line:
        * << ip address >>   dokku.me
    7. `cat ~/.ssh/id_rsa.pub | vagrant ssh dokku.me "sudo sshcommand acl-add dokku $USER"`

3. Create a project and push
    1. `git remote add dokkudev dokku@dokku.me:<< app name >>`
    2. `git push dokku dev master`
    3. For each config:
        * `dokku config:set << app name >> ... `
