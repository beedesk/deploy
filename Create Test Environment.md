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
        * wget -qO- https://raw.github.com/progrium/dokku/v0.3.12/bootstrap.sh | sudo DOKKU_TAG=v0.3.18 bash
    5. `sudo apt-get install daemontools`
    6. `sudo apt-get install build-essential`
    7. `sudo apt-get install libtool`
    9. Increase swap space: http://stackoverflow.com/a/22247782/1022903


3. Config host machine
    7. Exit vagrant ssh shell
    8. Edit `/etc/hosts` to add line:
        * << ip address >>   dokku.me
    9. `cat ~/.ssh/id_rsa.pub | vagrant ssh dokku.me "sudo sshcommand acl-add dokku $USER"`
        * (reference: https://github.com/progrium/dokku/blob/master/docs/installation.md)
    10. Update build-step
        * git clone https://github.com/progrium/buildstep.git
        * cd buildstep
        * git pull origin master
        * sudo make build
        * (reference: http://progrium.viewdocs.io/dokku/upgrading)


4. Create a project and push
    1. `git remote add dokkudev dokku@dokku.me:<< app name >>`
    2. `git push dokkudev master`
    3. For each config:
        * `dokkudev config:set << app name >> ... `
    4. Or, ssh into vagrant `cd << vagrant folder >> && vagrant ssh`
        * And, edit `~dokku/<< app name >>ENV` directly
        * ** remember to include `export` in front of each lines
        * `dokku config:set << app name >> a=b`  # to cause config reload 
    5. `dokkudev logs << app name >>`
    6. If error arise: `setuidgid: fatal: unable to run failure`, do step 2.5 ('daemontools')


5. Install plugins
    1. log into vagrant `vagrant ssh`
    2. create a sub directory for `plugins`: `mkdir /var/etc/lib/dokku-plugins`
    2. `cd /var/etc/lib/dokku-plugins`
    3. `git clone https://github.com/dyson/dokku-docker-options`
    3. `git clone https://github.com/ohardy/dokku-mariadb.git`
    3. `git clone https://github.com/ohardy/dokku-psql.git`
    6. `cd /var/etc/lib/dokku/plugins`
    3. `ln -s /var/etc/lib/dokku-plugins/dokku-docker-options`
    3. `ln -s /var/etc/lib/dokku-plugins/dokku-mariadb.git`
    3. `ln -s /var/etc/lib/dokku-plugins/dokku-psql.git`
    8. `sudo dokku plugins:install`


6. Update MySQL

        FLUSH TABLES WITH READ LOCK;
        FLUSH LOGS;
        SET GLOBAL binlog_format = 'MIXED';
        FLUSH LOGS;
        UNLOCK TABLES;
        
        # http://dba.stackexchange.com/a/6753