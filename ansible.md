# [ansible](https://www.ansible.com/)

## installation
```
yum install ansible
apt install ansible
```
```
pip install ansible 
```

## [ansible examples](https://github.com/ansible/ansible-examples)


## inventory file
```
172.17.0.2 label_ssh_example ansible_host=ansible_connection=ssh ansible_user=user ansible_password=secret
172.17.0.3 label_ssh_example ansible_host=ansible_connection=ssh ansible_user=user ansible_password=secret
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
