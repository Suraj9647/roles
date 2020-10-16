Ansible:

Ansible without roles                                                                                      


default configuration file location= /etc/ansible/ansible.cfg
default host file location= /etc/ansible/hosts


Ansible master:

- yum update -y
- yum install ansible -y
- sudo su -
- vim /root/suraj.pem
- vim /etc/ansible/hosts
- chmod 400 suraj.pem
- ansible all -m ping
- ansible all -m yum -a 'name=git state=present' --become
- ansible all -m service -a 'name=httpd state=started' --become


=====hosts====

/etc/ansible/hosts

[webservers]
web01 ansible_host=
web02 ansible_host=

[webservers:vars]
ansible_user=ec2-user
ansible_connection=ssh
ansible_ssh_private_key_file=/root/suraj.pem
#make sure to copy the pem file to above location
ansible_ssh_extra_args='-o StrictHostKeyChecking=no'


[group2]
grp1 ansible_host=x.x.x.x
grp2 ansible_host=x.x.x.y


[group2:vars]
ansible_user=ec2-user
ansible_connection=ssh
ansible_ssh_private_key_file=/root/newpem.pem
ansible_ssh_extra_args='-o StrictHostKeyChecking=no'

==================================

Ansible cfg file

[defaults]
#inventory       = /etc/ansible/hosts
#library         = ~/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
#module_utils    = ~/.ansible/plugins/module_utils:/usr/share/ansible/plugins/module_utils
#remote_tmp      = ~/.ansible/tmp
#local_tmp       = ~/.ansible/tmp

==============================

Example:

- vim nginx.yml

- name: Install nginx
  hosts: webservers
  become: true

  tasks:
  - name: Add epel-release repo
    ignore_errors: yes
    yum:
      name: epel-release
      state: present

  - name: Install nginx
    yum:
      pkg: nginx
      state: present

  - name: Insert Index Page
    template:
      src: /root/index.html
      dest: /usr/share/nginx/html/index.html

  - name: Start NGiNX
    service:
      name: nginx
      state: started

=====================================
create a file index.html in the given location in source(src)


- vim index.html

<h1>sample text here<h1>


sudo amazon-linux-extras enable nginx1
=========================

how to run ansible playbook

- ansible-playbook nginx.yml

===============================

Ansible Roles:

ansible-galaxy init /etc/ansible/roles/(role name)

/etc/ansible/roles/apache/

|-- README.md
|-- defaults
|   `-- main.yml
|-- files
|-- handlers
|   `-- main.yml
|-- meta
|   `-- main.yml
|-- tasks
|   `-- main.yml
|-- templates
|-- tests
|   |-- inventory
|   `-- test.yml
`-- vars
    `-- main.yml

tasks/main.yml - the main list of tasks that the role executes.

handlers/main.yml - handlers, which may be used within or outside this role.

library/my_module.py - modules, which may be used within this role (see Embedding modules and plugins in roles for more information).

defaults/main.yml - default variables for the role (see Using Variables for more information). These variables have the lowest priority of any variables available, and can be easily overridden by any other variable, including inventory variables.

vars/main.yml - other variables for the role (see Using Variables for more information).

files/main.yml - files that the role deploys.

templates/main.yml - templates that the role deploys.

meta/main.yml - metadata for the role, including role dependencies.

=============================

Ansible vault:

It is used to encrypt the password ans sensitive data.

ansible-vault create vault-pass.yml (create a vault )
ansible-vault view vault-pass.yml (view file)
ansible-vault edit vault-pass.yml (edit file)
ansible-vault decrypyt vault-pass.yml (decrypt file)
ansible-vault encrypt vault-pass.yml (encrypt file)
--ask-vault-pass (to provide password while running playbook)
--vault-password-file (to pass a vault password through a file)


