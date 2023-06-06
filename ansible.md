# [ansible](https://www.ansible.com/)
> agentless, connect via ssh to remote machine(s)

* [doc](https://docs.ansible.com/)
* [event driven automation](https://www.redhat.com/en/engage/event-driven-ansible-20220907?extIdCarryOver=true&intcmp=701f20000012m2KAAQ&percmp=7013a0000034e7YAAQ&sc_cid=701f2000001OH7EAAW)
* [variables for scripts, runtime variables, jinja variables](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html#)
* [cheat-sheet](https://cheat.readthedocs.io/en/latest/ansible/index.html)
* [best practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)
* [ansible modules](https://docs.ansible.com/ansible/latest/collections/index_module.html) `ansible-doc --list`, `ansible-doc shell`
* [ansible roles, ansible galaxy, ready roles](https://galaxy.ansible.com/)
* [examples](https://github.com/ansible/ansible-examples)
* [examples 2](https://github.com/mmumshad/ansible-training-answer-keys)
* [examples 3](https://github.com/mmumshad/ansible-training-answer-keys-2)
* [Jinja2 templating](https://jinja.palletsprojects.com/en/2.10.x/templates/)


## installation
```sh
yum install ansible
apt install ansible
```
```sh
pip3 install ansible 
# for python2 - default installation 
pip install ansible
```
remote machine should have 'python' - 'gather_facts: False' or 'gather_facts: no' otherwise

## uninstall
```sh
rm -rf $HOME/.ansible
rm -rf $HOME/.ansible.cfg
sudo rm -rf /usr/local/lib/python2.7/dist-packages/ansible
sudo rm -rf /usr/local/lib/python2.7/dist-packages/ansible-2.5.4.dist-info
sudo rm -rf /usr/local/bin/ansible
sudo rm -rf /usr/local/bin/ansible-config
sudo rm -rf /usr/local/bin/ansible-connection
sudo rm -rf /usr/local/bin/ansible-console
sudo rm -rf /usr/local/bin/ansible-doc
sudo rm -rf /usr/local/bin/ansible-galaxy
sudo rm -rf /usr/local/bin/ansible-inventory
sudo rm -rf /usr/local/bin/ansible-playbook
sudo rm -rf /usr/local/bin/ansible-pull
sudo rm -rf /usr/local/bin/ansible-vault
sudo rm -rf /usr/lib/python2.7/dist-packages/ansible
sudo rm -rf /usr/local/lib/python2.7/dist-packages/ansible
```

## ansible configuration places
* [environment variables](https://docs.ansible.com/ansible/latest/reference_appendices/config.html)
* ansible.cfg file:
  * env variable with file  $ANSIBLE_CONFIG ( point out to ansible.cfg )
  * ~/.ansible.cfg
  * /etc/ansible/ansible.cfg
```sh
# show current config file 
ansible-config view
# description of all ansible config variables
ansible-config list
# list of possible environment variables
ansible-config dump
```

### configuration for external roles
filename: ~/.ansible.cfg
```properties
[defaults]
roles_path = ~/repos/project1/roles:~/repos/project2/roles
```

### configuration for aws for skipping check host key
```sh
[defaults]
host_key_checking = false
```

### check configuration
```sh
ansible-config view
```

## inventory
### without inventory inline host ip 
```
ansible all -i desp000111.vantage.zur, --user=my_user -m "ping" -vvv
```

### without inventory with pem ssh private ssh key
generate PEM file
```sh
ssh-keygen -t rsa -b 4096 -m PEM -f my_ssh_key.pem
ll my_ssh_key.pem

ansible all -i desp000111.vantage.zur, --user=vitalii.cherkashyn -e ansible_ssh_private_key_file=my_ssh_key.pem -m "ping" -vvv
```

### inventory ini file
```properties
# example cfg file
[web]
host1
host2 ansible_port=222 # defined inline, interpreted as an integer

[web:vars]
http_port=8080 # all members of 'web' will inherit these
myvar=23 # defined in a :vars section, interpreted as a string
```
### inventory dynamic inventory from any source
```sh
pip install ansible ansible-inventory
```
python script ( for instance my-inventory.py ) 
* should generate json output:
```sh
python my-inventory.py --list
```
```json
{
    "group1": {
        "hosts": ["host1", "host2"],
        "vars": {
            "ansible_user": "admin",
            "ansible_become": True
        },
        "children":["group2"]
    },
    "group2": {
        "hosts": ["host3"],
        "vars": {
            "ansible_user": "user",
            "ansible_become": False
        },
        "children":[]
    }
}
```
* should generate json output:
```sh
python my-inventory.py --host host1
```
```json
{
  "ansible_user": "admin",
  "ansible_become": True
}
```
How to check the script:
```sh
chmod +x my-inventory.py

ansible -i my-inventory.py all -m ping
```



## execute with specific remote python version, remote python, rewrite default variables, rewrite variables, override variable  
```
--extra-vars "remote_folder=$REMOTE_FOLDER ansible_python_interpreter=/usr/bin/python"
```

## execute minimal playbook locally minimal example start simplest playbook
check-variable.yaml content:
```yaml
- hosts: localhost
  tasks:
  - name: echo variable to console
    ansible.builtin.debug:
      msg: System {{ inventory_hostname }} has gateway {{ ansible_default_ipv4.gateway }}
    no_log: false
```
> log output can be suppressed ( no log output )
```sh
ansible-playbook check-variable.yaml  -v
```

another example but with roles
```yaml
- hosts: localhost
  user: user_temp
  roles:
    - my_own_role
```

## execute ansible for one host only, one host, one remove server, verbosity
```sh
ansible-playbook -i "ubs000015.vantage.org , " mkdir.yaml 

ansible-playbook welcome-message.yaml -i airflow-test-account-01.ini --limit worker --extra-vars="ACCOUNT_ID=QA01" --user=ubuntu --ssh-extra-args="-i $EC2_KEY" -vvv

ansible all -i airflow-test-account-01.ini --user=ubuntu --ssh-extra-args="-i $EC2_KEY" -m "ping" -vvv
ansible main,worker -i airflow-test-account-01.ini --user=ubuntu --ssh-extra-args="-i $EC2_KEY" -m "ping"
```
simple file for creating one folder
```yaml
- hosts: all
  tasks:
    - name: Creates directory
      file:
        path: ~/spark-submit/trafficsigns
        state: directory
        mode: 0775
    - name: copy all files from folder
      copy: 
        src: "/home/projects/ubs/current-task/nodes/ansible/files" 
        dest: ~/spark-submit/trafficsigns
        mode: 0775

    - debug: msg='folder was amazoncreated for host {{ ansible_host }}'
```

## execute ansible locally, local execution
```sh
# --extra-vars="mapr_stream_path={{ some_variable_from_previous_files }}/some-argument" \

ansible localhost \
    --extra-vars="deploy_application=1" \
    --extra-vars=@group_vars/all/vars/all.yml \
    --extra-vars=@group_vars/ubs-staging/vars/ubs-staging.yml \
    -m include_role \
    -a name="roles/labeler"
```


## execute ansible-playbook with external paramters, bash script ansible-playbook with parameters, extra variables, external variables, env var
```j2
# variable from env
{{ lookup('env','DB_VARIANT_USERNAME') }}
```
```sh
ansible-playbook -i inventory.ini playbook.yml --extra-vars "$*"
```
with path to file for external parameters, additional variables from external file
```sh
ansible-playbook -i inventory.ini playbook.yml --extra-vars @/path/to/var.properties
ansible-playbook playbook.yml --extra-vars=@/path/to/var.properties
```

## external variables inline
```sh
ansible-playbook playbook.yml --extra-vars="oc_project=scenario-test mapr_stream_path=/mapr/prod.zurich/vantage/scenario-test"
```

## check is it working, ad-hoc command
```sh
ansible remote* -i inventory.ini -m "ping"
ansible remote* -i inventory.ini --module-name "ping"

## shell
ansible all --module-name shell 'free -m'
# sequence output with fork=1
ansible all --module-name shell --args uptime -f 1

## command
ansible all --module-name command --args 'fdisk -l' --become

## ansible-doc apt
ansible localhost --module-name apt --args 'name=python3 state=present'

## file
# create
ansible all --module-name file --args "path=/tmp/demo_1.txt state=touch mode=666"
ansible all --module-name file --args "path=/tmp/output state=directory mode=666"
# delete
ansible all --module-name file --args "path=/tmp/demo_1.txt state=absent mode=666"
# copy
ansible all --module-name copy --args "src=~/demo.txt dest=/tmp/demo.txt remote_src=yes"

```
```sh
ansible remote* -i inventory.ini -a "hostname"
```

## loop example
```sh
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

## repeat execution
```sh
--limit {playbookfile}.retry
```

## start with task, execute from task, begin with task, skip previous tasks
```sh
ansible-playbook playbook.yml --start-at-task="name of the start to be started from"
```

## replace variables inside file to dedicated file, move vars to separate file
* before
```yaml
   vars:
      db_user: my_user
      db_password: my_password
      ansible_ssh_pass: my_ssh_password 
      ansible_host: 192.168.1.14
```
* after 
*( 'vars' block is empty )*
filepath: 
```sh
./host_vars/id_of_the_server
```
or groupvars:
```sh
./group_vars/id_of_the_group_into_square_brakets
```
code
```yaml
db_user: my_user
db_password: my_password
ansible_ssh_pass: my_ssh_password 
ansible_host: 192.168.1.14
```

## move code to separate file, tasks into file
cut code from original file and paste it into separate file ( with appropriate alignment !!! ),
write instead of the code:
```yaml
    - include: path_to_folder/path_to_file
```
approprate file should be created:
```sh
./path_to_folder/path_to_file
```

## skip/activate some tasks with labeling, tagging
```yaml
tasks:
- template
    src: template/activation.sh.j2
    dest: /usr/bin/activation.sh
  tags:
  - flag_activation
```
multitag, multi-tag
```yaml
tasks:
- template
    src: template/activation.sh.j2
    dest: /usr/bin/activation.sh
  tags:
  - flag_activation
  - flag_skip
```

```sh
ansible-playbook previous-block.yml --skip-tags "flag_activation"
# ansible-playbook previous-block.yml --skip-tags=flag_activation
# ansible-playbook previous-block.yml --tags "flag_activation"
# ansible-playbook previous-block.yml --tags=flag_activation
```
# Debug
## [debug playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks_debugger.html)
```bash
export ANSIBLE_STRATEGY=debug
# revert it afterwards ( avoid "ERROR! Invalid play strategy specified: "):
# export ANSIBLE_STRATEGY=linear
```
print variables
```python
task.args
task.args['src']
vars()
```
change variables
```python
del(task.args['src'])
task.args['src']="/new path to file"
```
set variable
```
- name: Set Apache URL
  set_fact:
    apache_url: 'http://example.com/apache'
    
- name: Download Apache
  shell: wget {{ apache_url }}    
```
shell == ansible.builtin.shell

manage palying
```
redo
continue
quit
```

```yaml
- name: airflow setup for main (web server) and workers
  hosts: all
  tasks:
  - name: airflow hostname
    debug: msg="{{ lookup('vars', 'ansible_host') }}"      
  - name: all variables from host
    debug: msg="{{ vars }}"      
  when: run_mode == "debug"
```

## debug command
```
  - debug:
      msg: "print variable: {{  my_own_var }}"
```
```
  - shell: /usr/bin/uptime
    register: result

  - debug:
      var: result
```

## env variables bashrc
```sh
- name: source bashrc
  sudo: no   
  shell: . /home/username/.bashrc && [the actual command you want run]
```

## rsync copy files
```
  - name: copy source code
    synchronize:
      src: '{{ item.src }}'
      dest: '{{ item.dest }}'
      delete: yes
      recursive: yes
      rsync_opts:
        - "--exclude=.git"
        - "-avz"
        - '-e ssh -i {{ key }} '
    with_items:
    - { src: '{{ path_to_repo }}/airflow-dag/airflow_shopify/', dest: '/home/ubuntu/airflow/airflow-dag/airflow_shopify/' }

```

## ec2 managing airflow ec2
```
export PATH=$PATH:/home/ubuntu/.local/bin
nohup airflow webserver
```

## debug module
argument file ( args.json )
```json
{
    "ANSIBLE_MODULE_ARGS": {
        "task_parameter_1": "just a string",
        "task_parameter_2": 50
    }
}
```
execute file
```bash
python3 -m pdb library/oc_collaboration.py args.json
```
set breakpoint
```python
import pdb
...
pdb.set_trace()
```
run until breakpoint
```sh
until 9999
next
```

## debug module inline, execute module inline, adhoc module check
```sh
ansible localhost -m debug --args msg="my custom message"
# collect facts
ansible localhost -m setup
```

## task print all variables 
```yaml
- name: "Ansible | List all known variables and facts"
  debug:
    var: hostvars[inventory_hostname]
```

## ansible-console 
```sh
ansible-console
debug msg="my custom message"
shell pwd
```

## ansible print stdout
```sh
# default ansible output
ANSIBLE_STDOUT_CALLBACK=yaml
# humand readable output
ANSIBLE_STDOUT_CALLBACK=debug
```

# error handling, try catch
## stop execution of steps (of playbook) when at least one server will throw error
```yaml
  any_errors_fatal:true
```
## not to throw error for one certain task
```yaml
 - mail:
     to: 1@yahoo.com
     subject: info
     body: das ist information
   ignore_errors: yes
```
## fail when, fail by condition, parse log file for errors
```yaml
  - command: cat /var/log/server.log
    register: server_log_file
    failed_when : "'ERROR' in server_log_file.stdout"
```

# template, Jinja2 templating, pipes, [ansible filtering](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html)
default value
```
default path is {{ my_custom_path | default("/opt/program/script.sh") }}
```
escape special characters
```
{{ '{{ filename }}.log' }}
```

operation with list
```
{{ [1,2,3] | default(5) }}
{{ [1,2,3] | min }}
{{ [1,2,3] | max }}
{{ [1,2,3] | first }}
{{ [1,2,3] | last }}
{{ [1,2,3,2,3,] | unique }}
{{ [1,2,3] | union([1,2]) }}
{{ [1,2,3] | intersect([3]) }}
{{ 100 | random }}
{{ ["space", "separated", "value"] | join(" ") }}
{{'latest' if (my_own_value is defined) else 'local-build'}}
```
file name from path (return 'script.sh')
```
{{ "/etc/program/script.sh" | basename }}
```
## copy file and rename it, pipe replace suffix
```yaml
- name: Create DAG config
  template: src={{ item }} dest={{ airflow_dag_dir }}/config/{{ item | basename | regex_replace('\.j2','') }}
  with_fileglob:
    - ../airflow_dags/airflow_dags_gt/config/*.py.j2
```
## jinja conditions
```yaml
{% if deployment.jenkins_test_env is defined -%}
some yaml code
{% endif %}
```

## copy reverse copy from destination machine
```yaml
- name: Fetch template
  fetch:
    src: '{{ only_file_name }}'
    dest: '{{ destination_folder }}'
    flat: yes
  tags: deploy
```

## directives for Jinja
for improving indentation globally in file, add one of next line in the beginning
```yaml
#jinja2: lstrip_blocks: True
#jinja2: trim_blocks:False
#jinja2: lstrip_blocks: True, trim_blocks: True
```
for improving indentation only for the block
```j2
<div>
        {%+ if something %}<span>hello</span>{% endif %}
</div>
```
condition example
```j2
{% if lookup('env','DEBUG') == "true" %}
    CMD ["java", "start-debug"]
{% else %}
    CMD ["java", "start"]
{% endif %}
```

### directives for loop, for last, loop last
```j2
[
{% for stream in deployment.streams %}
    {
        "stream": "{{ stream.stream_name }}",
        "classN": "{{ stream.class_name }}",
        "script": "{{ stream.script_name }}",
        "sibFolders": [
        {% for folder in stream.sub_folders %}
            "{{ folder }}"{% if not loop.last %},{% endif %}
        {% endfor %}
        ]
    }{% if not loop.last %},{% endif %}
{% endfor %}
]
```

## escaping
just a symbol
```
{{ '{{' }}
```
bigger piece of code
```j2
{% raw %}
    <ul>
    {% for item in seq %}
        <li>{{ item }}</li>
    {% endfor %}
    </ul>
{% endraw %}
```


## template with tempfile
```yaml
- hosts: localhost
  gather_facts: no
  tasks:
    - tempfile:
        state:  file
        suffix: config
      register: temp_config

    - template:
        src:  templates/configfile.j2
        dest: "{{ temp_config.path }}"
```
# [modules](https://github.com/ansible/ansible/tree/devel/lib/ansible/modules)  
* [create custom module](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general.html)  
## settings for modules
also need to 'notify' ansible about module giving one of the next option:
* add your folder with module to environment variable ANSIBLE_LIBRARY  
* update $HOME/.ansible.cfg  
  ```properties
  library=/path/to/module/library
  ```

## module documentation
```sh
ansible-doc -t module {name of the module}
```

## minimal module
```python
from ansible.module_utils.basic import AnsibleModule
def main():
    input_fields = {
        "operation": {"required": True, "type": "str"},
        "file": {"required": True, "type": "str"},
        "timeout": {"required": False, "type": "int", "default": "120"}
    }
    module = AnsibleModule(argument_spec=input_fields)
    operation = module.params["operation"]
    file = module.params["file"]
    timeout = module.params["timeout"]
    # module.fail_json(msg="you must be logged in into OpenShift")
    module.exit_json(changed=True, meta={operation: "create"})    
```

# [plugins](https://github.com/ansible/ansible/tree/devel/lib/ansible/plugins/)
example of plugin
```j2
{{ list_of_values | average }}
```
python code for plugin
```python
def average(list):
    return sum(list) / float(len(list))
    
class AverageModule(object):
    def filters(self):
        return {'average': average}
```
execution
```sh
export ANSIBLE_FILTER_PLUGINS=/full/path/to/folder/with/plugin
ansible-playbook playbook.yml
```

## lookups
```sh
# documentation
ansible-doc -t lookup -l
ansible-doc -t lookup csvfile
```

replace value from file with special format
```python
{{ lookup('csvfile', 'web_server file=credentials.csv delimiter=,') }}
{{ lookup('ini', 'password section=web_server file=credentials.ini') }}
{{ lookup('env','DESTINATION') }}
{{ lookup('file','/tmp/version.txt') }}
```
lookups variables
```j2
{{ hostvars[inventory_hostname]['somevar_' + other_var] }}

For ‘non host vars’ you can use the vars lookup plugin:
{{ lookup('vars', 'somevar_' + other_var) }}
```

```yaml
- name: airflow setup for main (web server) and workers
  hosts: all
  tasks:
  - name: airflow hostname
    debug: msg="{{ lookup('vars', 'ansible_host') }}"      
  - name: variable lookup
    debug: msg="lookup data {{ lookup('vars', 'ansible_host')+lookup('vars', 'ansible_host') }}"
  - name: read from ini, set variable
    set_fact:
      queues: "{{ lookup('ini', lookup('vars', 'ansible_host')+' section=queue file=airflow-'+lookup('vars', 'account_id')+'-workers.ini') }}"
  - name: airflow lookup
    debug: msg=" {{ '--queues '+lookup('vars', 'queues') if lookup('vars', 'queues') else '<default>'  }}"
```

# inventory file
---
## inventory file, inventory file with variables, [rules](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)
```ini
[all]
10.2.2.24
10.2.2.25
10.2.2.26
```
```ini
[remote_ssh]
172.28.128.3     ansible_connection=ssh   ansible_port=22   ansible_user=tc     ansible_password=tc
```
## dynamic inventory
python inventory.py (with 'py' extension) instead of txt
```python
import json
data = {"databases": {"hosts": ["host1", "host2"], "vars": {"ansible_ssh_host":"192.168.10.12", "ansible_ssh_pass":"Passw0rd"} }}
print(json.dumps(data))
```
also next logic should be present
```sh
inventory.py --list
inventory.py --host databases
```
[prepared scripts](https://github.com/ansible/ansible/tree/devel/contrib/inventory)

## inventory file with variables ( python Jinja templating)
```ini
[remote_ssh]
172.28.128.3     ansible_connection=ssh   ansible_port=22   ansible_user=tc     ansible_password=tc   http_port=8090
```
playbook usage:
```j2
'{{http_port}}'
```

## execution with inventory examples
for one specific host without inventory file 
```sh
ansible-playbook playbook.yml -i 10.10.10.10
```
with inventory file
```sh
ansible-playbook -i inventory.ini playbook.yml 
```
issue with execution playbook for localhost only, local execution
```text
Note that the implicit localhost does not match 'all'
...
skipping: no hosts matched 
```
solution
```sh
ansible-playbook --inventory="localhost," --connection=local --limit=localhost --skip-tag="python-script" playbook.yaml

# example with external variables
ansible-playbook --inventory="localhost," --connection=local --limit=localhost \
--extra-vars="oc_project=scenario-test mapr_stream_path=/mapr/prod.zurich/vantage/scenario-test" \
--tag="scenario-service" deploy-scenario-pipeline.yaml
```

solution2
```sh
#vim /etc/ansible/hosts
localhost ansible_connection=local
```


# strategy
---
```
  strategy: linear
```
* linear ( default )
*after each step waiting for all servers*
* free
*independently for all servers - someone can finish installation significantly earlier than others*

additional parameter - specify amount of servers to be executed at the time ( for default strategy only )
```
  serial: 3
```
```
  serial: 20%
```
```
  serial: [5,15,20]
```

default value "serial" into configuration **ansible.cfg**
```properties
forks = 5
```

# async execution, nowait task, command execution
**not all modules support this operation**
execute command in asynchronous mode ( with preliminary estimation 120 sec ), 
with default poll result of the command - 10 ( seconds )
```yaml
  async: 120
```
execute command in asynchronous mode ( with preliminary estimation 120 sec ), 
with poll result of the command - 60 ( seconds )
```yaml
  async: 120
  poll: 60
```
execute command and forget, not to wait for execution
```yaml
  async: 120
  poll: 0
```
execute command in asynchronous mode, 
register result
checking result at the end of the file
```yaml
- command: /opt/my_personal_long_run_command.sh
  async: 120
  poll: 0
  register: custom_command_result
  
- name: check status result
  async_status: jid={{ custom_command_result.ansible_job_id }}
  register: command_result
  until: command_result.finished
  retries: 20
```


# roles
* [ansible galaxy](https://galaxy.ansible.com/)
* [ansible-galaxy cli](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html)
---
## init project ansible-galaxy, create new role, init role
execute code into your project folder './roles'
```sh
ansible-galaxy init {project/role name}
```
result:
```text
./roles/{project/role name}
	#         Main list of tasks that the role executes
    /tasks
	#         Files that the role deploys
    /files
	#         Handlers, which may be used within or outside this role
    /handlers
	#         Modules, which may be used within this role
    /library
	#         Default variables for the role
    /defaults
	#         Other variables for the role
    /vars
	#         Templates that the role deploys
    /templates
	#         Metadata for the role, including role dependencies
    /meta
```
insert into code
```yaml
  roles:
  - {project/role name}
```
all folders of the created project will be applied to your project ( tasks, vars, defaults )
*in case of manual creation - only necessary folders can be created*

## ansible search for existing role
```sh
ansible-galaxy search {project/role name/some text}
ansible-galaxy info role-name

```

## import existing roles from [ansible galaxy](https://galaxy.ansible.com/list)
```sh
cd roles
ansible-galaxy import {name of the project/role}

# print out existing roles
ansible-galaxy role list 
```
insert into code
```yaml
  roles:
  - {project/role name}
```
all folders of the imported project will be applied to your project ( tasks, vars, defaults )

## import task from role, role.task, task inside role
```yaml
- hosts: localhost
  # hosts: all
  # hosts: <name of section from inventory file>
  tasks:
  - name: first step
    include_role:
      name: mapr-kafka
      tasks_from: cluster-login
```
or 
```yaml
- name: Deploy session
  import_tasks: roles/dag-common/tasks/deployment-tasks.yaml
  vars:
    dag_deploy_dir: /mapr/swiss.zur/airlfow/deploy
  tags:
    - release
```

## export 
create/update file:
```sh
./roles/{project/role name}/meta/main.yml
```

## local run local start playbook
```yaml
- hosts: localhost
  tasks:
  - name: Ansible create file with content example
    copy:
      dest: "/tmp/remote_server.txt"
      content: |
        dog
        tiger
```
### minimal playbook
```yaml
- hosts: localhost
  tasks:
  - name: Ansible create file with content example
    copy:
      dest: "/tmp/remote_server.txt"
      content: |
        {{ lookup('env','TEST_1') }}
        {{ lookup('env','TEST_2') }}
```

```sh
ansible-playbook ansible-example.yml
```

## execute role, role execution, start role locally, local start, role local execution, check role
```sh
ansible-galaxy init print-message

echo '- name: "update apt packages."
  become: yes
  apt:
    update_cache: yes
    
- name: install apache
  apt:
    name: ["apache2"]

- name: create file
  shell:
    cmd: echo "hello from container {{ role_name }}" > /var/www/html/index.html

- name: start service
  service:
    name: "apache2"
    state: "started"
' > print-message/tasks/main.yml

# execute for localhost
ansible localhost --module-name include_role --args 'name=print-message'

# execute ansible role for docker container
docker start atlassian/ssh-ubuntu:0.2.2
ansible all -i 172.17.0.2, --extra-vars "ansible_user=root ansible_password=root" --module-name include_role --args 'name=print-message'

x-www-browser http://172.17.0.2
```

```sh
ansible localhost \
    --extra-vars="deploy_application=1" \
    --extra-vars=@group_vars/all/defaults/all.yaml \
    --extra-vars=@group_vars/all/vars/all.yaml \
    --extra-vars="mapr_stream_path={{ some_variable_from_previous_files }}/some-argument" \
    -m include_role \
    -a name="new_application/new_role"
```
where "include_role" - module to run ( magic word )   
where "new_application/new_role" - subfolder to role 
where @group_vars/all/default/all.yaml - sub-path to yaml file with additional variables

## console output with applied roles should looks like
```text
TASK [{project/role name}: {task name}] ***********************************
```
for example
```text
TASK [java : install java with jdbc libraries] ***********************************
```

# file encryption, vault
```sh
ansible-vault encrypt inventory.txt
ansible-vault view inventory.txt
ansible-vault create inventory.txt
```
ask password via command line
```sh
ansible-playbook playbook.yml -i inventory.txt -ask-vault-pass
```
file should contain the password
```sh
ansible-playbook playbook.yml -i inventory.txt -vault-password-file ./file_with_pass.txt
```
script should return password
```sh
ansible-playbook playbook.yml -i inventory.txt -vault-password-file ./file_with_pass.py
```

# modules
* [list of built in modules](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html)  
* [custom module creation doc](docs.ansible.com/ansible/latest/dev_guide/developing_modules_general.html)  

### [apt](https://docs.ansible.com/ansible/latest/modules/apt_module.html), python installation 
```yaml
- name: example of apt install 
  apt: name='{{ item }}' state=installed
  with_items:
    - python
    - python-setuptools
    - python-dev
    - build-essential
    - python-pip
```

### [service](https://docs.ansible.com/ansible/latest/modules/service_module.html)
```yaml
- name: example of start unix service
  service:
    name: mysql
    state: started
    enabled: yes
```

### [pip](https://docs.ansible.com/ansible/latest/modules/pip_module.html)
```yaml
- name: manage python packages via pip 
  pip:
    name: flask
```

### include variables import variables
```json
- name: External variables
  include_vars: roles/marker-table/defaults/main.yaml
  tags: deploy
```

### echo
add flag for ```ansible``` or ```ansible-playbook```:-vvv(3) -vv (2) or -v (1)
```yaml
- debug:
    msg: ">>> {{ data_portal_deploy_folder }}/data-portal.jar"
    var: src
    verbosity: 2
```

### [copy](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html?extIdCarryOver=true&sc_cid=701f2000001OH7nAAG#ansible-collections-ansible-builtin-copy-module)
```yaml
- name: Ensure MOTD file is in place
  copy:
    src: files/motd
    dest: /etc/motd
    owner: root
    group: root
    mode: 0644
    
- name: Ensure MOTD file is in place
  copy:
    content: "Welcome to this system."
    dest: /etc/motd
    owner: root
    group: root
    mode: 0644
```

### [template](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html#ansible-collections-ansible-builtin-template-module)
```json
- name: Ensure MOTD file is in place
  template:
    src: templates/motd.j2
    dest: /etc/motd
    owner: root
    group: root
    mode: 0644
```

### [user](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html)
```json
- name: Ensure user1 exists
  user:
    name: user1
    group: users
    groups: wheel
    uid: 2001
    password: "{{ 'mypassword' | password_hash('sha512') }}"
    state: present
```

### [package](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html)
```json
- name: Ensure Apache package is installed
  package:
    name: httpd
    state: present
```

### [firewalld](https://docs.ansible.com/ansible/latest/collections/ansible/posix/firewalld_module.html)
```json
- name: Ensure port 80 (http) is open
  firewalld:
    service: http
    state: enabled
    permanent: yes
    immediate: yes
```

### [file](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html)
```json
- name: Ensure directory /app exists
  file:
    path: /app
    state: directory
    owner: user1
    group: docker
    mode: 0770
```

### [lineinfile](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html)
```json
- name: Ensure host my-own-host in hosts file
  lineinfile:
    path: /etc/hosts
    line: 192.168.0.36 my-own-host
    state: present
    
- name: Ensure root cannot login via ssh
  lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^PermitRootLogin'
    line: PermitRootLogin no
    state: present
```
```json
- name: Set version information
  lineinfile:
    path: "{{ working_folder }}/version.json"
    regexp: "{{ item.Reg }}"
    line: "{{ item.Line }}"
  with_items:
    - { Reg: 'releaseName', Line: '"releaseName": "{{ release_name }}",' }
    - { Reg: 'releaseNotesUrl', Line: '"releaseNotesUrl": "{{ release_notes_url }}",' }
  delegate_to: localhost
  tags: deploy
```

### [unarchive](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/unarchive_module.html)
```json
- name: Extract content from archive
  unarchive:
    src: /home/user1/Download/app.tar.gz
    dest: /app
    remote_src: yes
```

### [command](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html)
```json
- name: Run bash script
  command: "/home/user1/install-package.sh"
```

### TBD
* system
* commands
* database
* cloud
* windows


# [ansible awx](https://github.com/ansible/awx)

# issues

## fingerprint checking
```text
fatal: [172.28.128.4]: FAILED! => {"msg": "Using a SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this.  Please add this host's fingerprint to your known_hosts file to manage this host."}
```
resolution
```sh
export ANSIBLE_HOST_KEY_CHECKING=False
ansible-playbook -i inventory.ini playbook-directory.yml
```
