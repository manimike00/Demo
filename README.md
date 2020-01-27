
# CKAN deployment on CentOS 7
[Ansible](https://docs.ansible.com) scripts for automatic deployment of CKAN

## Requirements
### Control machine requirements
* [Ansible](https://docs.ansible.com/ansible/intro_installation.html) (>= 2.4)
* [VirtualBox](https://www.virtualbox.org/manual/ch02.html) (>= 5.1)
* [Vagrant](https://www.vagrantup.com/docs/installation/) (>= 1.9)

### Managed node requirements
* [CentOS](https://www.centos.org/) (>= 7.3)

## Deploying CKAN on development instance (Non Local Development)

1. [Ignore if already cloned] Run the below command to clone the ckanDeploy on the development CKAN server under dev directory.
```bash
cd ~/dev; git clone git@git.syngentaaws.org:dda/platforms/GISPlatform/gris/ckandeploy.git
```

2. Run below command to checkout latest deployment code for development environment 
```bash
git checkout rebase-dev-from-master; git pull -r
```

3. Run the playbook using the following commands
```bash
ansible-playbook playbook.yml -i environments/development/hosts
```

## Deploying CKAN on production instance

*Note:* You should create a branch named _release-<release_version>_ from the _rebase-dev-from-master_ which has changes required to be deployed on production. For example, release-0.1.0.

1. [Ignore if already cloned] Run the below command to clone the ckanDeploy on the production CKAN server under prod directory.
```bash
cd ~/prod; git clone git@git.syngentaaws.org:dda/platforms/GISPlatform/gris/ckandeploy.git
```

2. Run below command to checkout latest deployment code for production environment 
```bash
git checkout release-<release_version>; git pull -r
```
3. Provide the production database and oauth_2 credentials in vars section in the file _environments/production/hosts_

4. Run the playbook using the following commands
```bash
ansible-playbook playbook.yml -i environments/production/hosts
```


## Deploy ckan to development virtual machine:
```bash
ansible-playbook playbook.yml
```

## Upgrading a CKAN instance
Upgrading the CKAN development instance to the latest version can be done by running the Ansible playbooks again.

Upgrade Ansible scripts:
```bash
git pull
```

Upgrade the CKAN instance:
```bash
ansible-playbook playbook.yml
```
## Local development environment setup
Setup the VM using vagrant

```bash
vagrant up
```

SSH into the vagrant box
```bash
vagrant ssh epos-msl
```

Double check that the ip configured is: 192.168.60.10
by running:
```bash
ip addr
```

Exit the ssh session
```bash
exit
```

set 192.168.60.10 for the host variable in (environment/development/hosts)
```
[all]
192.168.60.10
```

The path to the rsa key is
./vagrant/ssh/vagrant

### Note:
Before you are able to run the ansible playbook you need to change the
permissions on the rsa private (or else SSH will not work, with a key
  not secure error)
```
sudo chmod 600 ./vagrant/ssh/vagrant
```

Set the ansible_ssh_private_key_file to the rsa key in playbook.yml under vars
```
ansible_ssh_private_key_file: ./vagrant/ssh/vagrant
```

Update the playbook to use the vagrant credentials by switching remote_user
in the ansible.cfg file (under defaults) to the 'vagrant'
```
remote_user = vagrant
```

Set the variable epos_msl_fqdn (in playbook.yml) to the same value as the host:
```
epos_msl_fqdn: 192.168.60.10
```

Run the playbook
```bash
ansible-playbook playbook.yml
```

Once the playbook has completed you can access the instance in the browser at:
https://192.168.60.10

## License
This project is licensed under the GPL-v3 license.
The full license can be found in [LICENSE](LICENSE).
