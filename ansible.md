# [ansible](https://www.ansible.com/)

## installation
```
yum install ansible
apt install ansible
```
```
pip install ansible 
```
remote machine should have 'python' - 'gather_facts: False' otherwise

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

## check is it working, ad-hoc command
```
ansible remote* -i inventory.ini -m "ping"
ansible remote* -i inventory.ini --module-name "ping"
```
```
ansible remote* -i inventory.ini -a "hostname"
```

## [all modules](https://docs.ansible.com/ansible/devel/modules/list_of_all_modules.html)

## loop example
```
    - name: scripts {{ item }}
      template:
        mode: 0777 
        src: "templates/{{ item }}" 
        dest: "{{ root_folder }}/{{ item }}" 
      loop:
        - "start-all.sh"
        - "status.sh"
        - "stop-all.sh"
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


# [ansible awx](https://github.com/ansible/awx)

# issues

## fingerprint checking
```
fatal: [172.28.128.4]: FAILED! => {"msg": "Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this.  Please add this host's fingerprint to your known_hosts file to manage this host."}
```
resolution
```
export ANSIBLE_HOST_KEY_CHECKING=False
ansible-playbook -i inventory.ini playbook-directory.yml
```
