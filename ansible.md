# [ansible](https://www.ansible.com/)

## installation
```
yum install ansible
apt install ansible
```
```
pip install ansible 
```
remote machine must have 'python' !!!

## [ansible examples](https://github.com/ansible/ansible-examples)

## ansible configuration places
* path variable $Ansible_Config
* ./ansible.cfg
* ~/ansible.cfg
* /etc/ansible/ansible.cfg

## inventory file, inventory file with variables, [rules](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)
```
[remote_ssh]
172.28.128.3     ansible_connection=ssh   ansible_port=22   ansible_user=tc     ansible_password=tc
```

## inventory file with variables ( python Jinja templating)
```
[remote_ssh]
172.28.128.3     ansible_connection=ssh   ansible_port=22   ansible_user=tc     ansible_password=tc   http_port=8090
```
playbook usage:
```
'{{http_port}}'
```

## check working, ad-hoc command
```
ansible remote* -i inventory.ini -m "ping"
ansible remote* -i inventory.ini --module-name "ping"
```
```
ansible remote* -i inventory.ini -a "hostname"
```

## [all modules](https://docs.ansible.com/ansible/devel/modules/list_of_all_modules.html)

## conditions "when"
TBD

## modules
TBD
* system
* commands
* files
* database
* cloud
* windows

## ad-hoc commands
TBD

# [ansible awx](https://github.com/ansible/awx)
