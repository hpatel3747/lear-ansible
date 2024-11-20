## learn-ansible
```text
Configuration management tools
- CFEngine
- Puppet
- Chef
- Ansible

Ansible is declarative

Ansible supports both Push and pull configuration

original author Michael, developed v1, called ansible core
red hat acquired it, then called V2, then Ansible V3 was released
check out http://pypi.org
ansible is developed in python
free to use, its product from python community
Ansible tower is UI version
Ansible and Ansible core (red hat)
Ansible core comes with support, Ansible has external support
Ansible uses SSH
Needs inventory
need CM server with Ansible installed
Ansible requires >Python 3.10
Create Workstation as Configuration Manager server
```

### **install python**
```text
#sudo dnf list | grep python # see whats available

#sudo dnf list | grep ansible # see what ansible is installed
latest version is ansible core 2.17

# python3 --version #shows python version

# pip list #shows available pips

# sudo dnf install python3.12 #Install python version 3.12

# sudo pip3.11 install ansible ansible-core --> to install Ansible
# ansible --version --> check version
```

#### **create other nodes**
```text

create inventory file /tmp/inv
add ips in the inventory

[frontend]
10.0.0.6
10.0.0.7
[cart]
10.0.0.10

# To apply playbook using above inventory file to frontend servers, applying site.yml playbook
$ansible-playbook -i /tmp/inv -l frontend site.yml
```

#### **To connect to servers listed in inventory**
```text
#ansible -i /tmp/inv -e ansible_user=ec2-user -e ansible_pass=DevOps321 -m ping --> will connect to all the servers listed in the /inv file over ssh and do ping
instead of /inv you can provide ip of servers sperated by commas
-m is also called as collection
-b become sudo
dnf collection ansible - search in google and it will show
#ansible -i /tmp/inv -e -b ansible_user=ec2-user -e ansible_pass=DevOps321 -m ansible.builtin.dnf -a name=nginx,state=present --> installs nginx on the servers in the inventory
search for systemctl collection ansible
ansible.builtin.systemd_service
write in file format called playbook
vim /tpm/test.yml
 -host: all
  become: yes
  tasks:
 - name:install nginx
   ansible.builtin.dnf:
    name: nginx
    state: installed
    
   -name: start nginx
    ansible.builtin.systemd_service:
     name: nginx
     state: started
 
 #ansible-playbook -i 172.32.24.248, -e ansible_user=ec2-user -e ansible_password=DevOps321 /emp/test.yml
```
### Markup language
```text
yaml is markup language
standard is required for processing

HTML
eXtensible Markup language (XML)
Java script object notation (JSON)
YAML

-plain
-list
-MAP/Dictionary

XML
<key>Value</key>
<a>10</a>

JSON
{
    "a" = 10
}

YAML
a: 10
b: [ 99,89 ]
b:
 - 99
 - 89
c:
 course: DevOps
 time: 730am

c: { course: DevOps, 
time: 730am }

keys are provided by ansible
some values are also provided by ansible
 it can be in.yaml or .yml
 there are multiple plays
 every play starts with name - it not mandatory
 host is must and should
 either tasks/roles is must to have 
 spaces are very important in yaml (not a tab)
```
### list of Ansible Modules
```text
ansible.builtin.dnf
ansible.builtin.copy
ansible.builtin.shell
ansible.builtin.debug
ansible.builtin.template
ansible.builtin.file
ansible.builtin.unarchive
ansible.builtin.user # to manage users ad groups
ansible.builtin.service # to manage service
ansible.builtin.setup # to get ad hoc information about system
```
### Ansible Utilities
```text
ansible  #Define and run a single task ‘playbook’ against a set of hosts
ansible-config  #View ansible configuration
ansible-console  #REPL console for executing Ansible tasks
ansible-doc  #plugin documentation tool
ansible-galaxy  #Perform various Role and Collection related operations.
ansible-inventory  #Show Ansible inventory information, by default it uses the inventory script JSON format
ansible-playbook  #Runs Ansible playbooks, executing the defined tasks on the targeted hosts
ansible-pull  #pulls playbooks from a VCS repo and executes them on target host
ansible-vault  #encryption/decryption utility for Ansible data files
```
### Ansible Default Files
```text
/etc/ansible/hosts – Default inventory file
/etc/ansible/ansible.cfg – Config file, used if present
~/.ansible.cfg – User config file, overrides the default config if present
```

### Ansible variable
```text
create 01-variable.yml
- name: demo on variable
  hosts: all
  connection: local --> this will run ansible locally instead of ssh on remote server
  vars:
    URL: vars.google.com
  tasks:
    - name: Print URL
    ansible.builtin.debug:
      msg: URL - {{ URL }}  
# two {{ }} flower brackets are used to show variable value, just like $ sign in shell
then run playbook on workstation
# git pull ; ansible-playbook 02-variable.yml

you can also specify variable on command line 

#git pull ; ansible-playbook 02-variable.yml -e URL=cli.google.com
-e means encvronment data in ansible
```
### function (Roles)
```text
ansible roles are function equivalent
by default ansible will look for main.yml in the directory..
create directory structure
roles/sample/tasks/main.yml

roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            nginx.conf    #  <------- templates end in .j2
        files/            #
            mongodb.repo  #  <-- files for use with the copy resource
            rabbitmq.repo #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies
```
### ansible examples

#### list all installed collection
```text
ansible-galaxy collection list
```
#### list all installed roles
```text
ansible-galaxy role list
```
#### install a package
```text
ansible localhost -m ansible.builtin.apt -a "name=apache2 state=present" -b -K
```
#### show plugin names and thier source files
```text
ansible-doc -F
```
#### show available plugins
```text
ansible-doc -t module -l
```
#### running playbook in check mode
```text
ansible-playbook --check playbook.yaml
```
#### get ansible specific feed on a playbook
```text
$ ansible-lint verify-apache.yml
[403] Package installs should not use latest
verify-apache.yml:8
Task/Handler: ensure apache is at the latest version
```
#### setting PATH environment variable
```text
hosts: servers
environment:
  PATH: "{{ ansible_env.PATH }}:/thingy/bin"
  SOME: value
```

