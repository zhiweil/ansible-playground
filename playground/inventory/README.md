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

The following is a sample of an Ansible inventory file. 
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
If the hosts naming convention allows, sometimes you can replace some lengthy code with a single line within your inventory file. Please udpate your /etc/ansible/hosts with:
```init
[local]
localhost ansible_connection=local

[allhosts]
ansible-node[1:2]
```
You can specify a range of hosts by a single line. You'll see both managed nodes in ping result of group "allhosts".

This grammar is similar to Python list, the only difference is that both the beginning and ending numbers are inclusive. 
