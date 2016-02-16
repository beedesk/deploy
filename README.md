# ansible-vagrant-dokku
Ansible playbooks for set up vagrant with dokku

### Install vagrant and setup environment

Open `setup.yml` with your favorite text redactor and check it.
This script require sudo so you can execute `sudo echo "Hey!"` in the same shell before ansible or ask for prompt password with `-K` option.
Then execute:
```
ansible-playbook setup.yml
```

For verbose output use `-v` option.

### Setup Vagrant

Now you can setup Vagrant with your new Vagrantfile:
```
vagrant up
```

Vagrant will set up new VM for you and apply to it `dokku.yml`, that will set up dokku into VM.
If something goes wrong you can reapply `dokku.yml` with command:
```
vagrant provision
```
