# Inventory
SSH to ansible-master if you have not done it. All the operations in this section run on ansible-master.
```bash
docker exec -it ansible-master bash
```

## Basics
Ansible inventory is in INI format which consists of the following elements:
* host - can be IP address or FQDN
* alias - Provide human readable name for IP 
* group - container of hosts. 

The semi-colon indicates a comment line for INI file. 

The following is a sample Ansible inventory file. 
```ini
; inside ungrouped group
ansible-node1
172.16.238.3
; alias
node2 ansible_host=172.16.238.4

; group
[allhosts]
ansible-node2
```

There are two special groups which always exist without explicit declaration:
* all - all hosts for an inventory
* ungrouped - hosts not declared under any group. 

## Alias of host
Sometimes you might know the IP address of hosts only, it is much easier for human beings to remember if they are given meaningful names. Alias comes to the rescure under such scenarios. 

Please update /etc/ansible/hosts with this [file](./inventory_alias). It will look like:
```ini
node1 ansible_host=172.16.238.3

[local]
localhost ansible_connection=local

[allhosts]
node1
ansible-node2
```
Alias "node1" is now representing host 172.16.238.3, which was ansible-node1. Now you can ping this host by:
```bash
ansible node1 -m ping
```

You'll see output:
```text
node1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    }, 
    "changed": false, 
    "ping": "pong"
}
```

As the alias "node1" is referenced by group "allhosts", so you can also ping the group.

## Range of hosts
If the hosts naming convention allows, sometimes you can replace some lengthy code with a single line within your inventory file. Please udpate your /etc/ansible/hosts with this [file](./inventory_range):
```init
[local]
localhost ansible_connection=local

[allhosts]
ansible-node[1:2]
```
You can specify a range of hosts by a single line. You'll see both managed nodes in ping result of group "allhosts".

This grammar is similar to Python list, the only difference is that both the beginning and ending numbers are inclusive. 

## Host in multiple groups
Hosts can be in multiple  "groups. Please replace /var/ansible/hosts with [this](./inventory_multiple_groups) file.

Run the following commands and see if the outputs are as expected.
```bash
ansible building1 -m ping
ansible building2 -m ping
ansible building1:building2 -m ping
ansible allhosts -m ping
```

## Nested groups
Group can be nested in another group, which effectively put hosts of child group into parent group. Nested groups are declare by "[groupname:children]", here the ":children" is a keyword. 

**NOTE:**
* Only groups can be put under the :children sections.
* Group range is not allowed, you must list group children individually.
 

Please replace /etc/ansible/hosts with [this](./inventory_nested_groups) file. 
```ini
[local]
localhost ansible_connection=local

[building1]
ansible-node1

[building2]
ansible-node2

[allhosts:children]
building1
building2
```
Ping "allhosts", you'll see both ansible-node1 and ansible-node2.

## Variable 
**You can put variables at host, alias or group levels, but this is not a good practice.** You will likely to put these in separate files when project grows. 
```ini
; alias level variables
node1 ansible_port=22 ansible_host=172.16.238.3
 
; host level variables
[local]
localhost ansible_connection=local

[allhosts]
ansible-node2

; group level variables
[allhosts:vars]
localtion=nz
language=en
```

### Variable inheritance of nested groups
* Variables of all groups a host holds a membership with are merged
* For the same variable which is assigned values at different levels, the value at lowest level (closest to host) takes precedence.

### Best practices for organizing variables
Put hosts or groups variables under subfolders relative to inventory files. Take the following inventory as an example:
```ini
ansible-node0

[building1]
ansible-node1

[building2]
ansible-node2
```

There will be three files under two subfolders relative to the folder in which inventory or playubook resides. 
```text
/etc/ansible/hosts/group_vars/building1.yml
/etc/ansible/hosts/group_vars/building2.yml
/etc/ansible/hosts/host_vars/ansibel-node0
```

The two subfolders "group_vars" and "host_vars" can be created under inventory folder or playbook folder. If they are created under both folders, the variables under playbook folder override those under inventory folder.

## Load multiple inventories
Load multiple inventories by
```bash
ansible someplaybook.yml -i inventory1 -i inventory2
```

The host and invetory varibles are merged by the order of inventory files specified. In the example above, variables of inventory2 will override those of inventory1. 

## Use Custom inventory file
So far, all the examples use the system inventory file at /etc/ansible/hosts. Actually, inventory files can be specified by command line. 

All the examples have been mapped to Ansible control node at folder /root/playground/, you can run the examples of this section by the process below:
```bash
docker exec -it ansible-master bash
cd /root/playground/inventory
ansible -i inventory_alias allhosts -m ping
```

