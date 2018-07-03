# CKAN deployment on CentOS 7
[Ansible](https://docs.ansible.com) scripts for automatic deployment of CKAN

## Requirements
### Control machine requirements
* [Ansible](https://docs.ansible.com/ansible/intro_installation.html) (>= 2.4)
* [VirtualBox](https://www.virtualbox.org/manual/ch02.html) (>= 5.1)
* [Vagrant](https://www.vagrantup.com/docs/installation/) (>= 1.9)

### Managed node requirements
* [CentOS](https://www.centos.org/) (>= 7.3)

## Deploying CKAN development instance (Non Local Development)

In the hosts file (environment/development/hosts)
```
[all]
<host address>
```

Set the variable epos_msl_fqdn (in playbook.yml) to the same value as the host
(or the domain name if available):
```
epos_msl_fqdn: <host address> OR <domain name>
```

Update the playbook to use the vagrant credentials by switching remote_user
in the ansible.cfg file (under defaults) to the ssh username
```
remote_user = <username>
```

In the primary playbook in the top directory add an security configurations
under the ansible_ssh_private_key_file if necessary
 ```
ansible_ssh_private_key_file: ~/Path/To/Keys.rsa
 ```

Deploy ckan to development virtual machine:
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
