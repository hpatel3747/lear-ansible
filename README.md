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
**install python**
#sudo dnf list | grep python # see whats available
#sudo dnf list | grep ansible # see what ansible is installed
latest version is ansible core 2.17
# python --> shows python version
# pip --> shows availble pips
# sudo pip3.11 install ansible ansible-core --> to install Ansible
# ansible --version --> check version

create other nodes
create inventory file /tmp/inv
add ips in the inventory

[frontend]
10.0.0.6
10.0.0.7
[cart]
10.0.0.10

above is for static ip inventory
in cloud ip may change
so go for dynamic inventory
**To connect to servers listed in inventory**
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
```