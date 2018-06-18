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


## inventory file, [rules](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)
```
[remote_ssh]
172.28.128.3     ansible_connection=ssh   ansible_port=22   ansible_user=tc     ansible_password=tc
```

## inventory file with variables ( python Jinja templating)
```
172.17.0.2 label_ssh_example ansible_host=ansible_connection=ssh ansible_user=user ansible_password=secret http_port=8090
172.17.0.3 label_ssh_example ansible_host=ansible_connection=ssh ansible_user=user ansible_password=secret http_port=8090
```

playbook usage:
```
'{{http_port}}'
```

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
