# Getting Started

## Add hosts to system inventory
SSH to ansible-master if you have not done it. All the operations in this section run on ansible-master.
```bash
docker exec -it ansible-master bash
```

If no inventory is specified, the inventory at /etc/ansible/hosts will be applied. Add a new group "allhosts" to this INI file. The file will look like this after the modification. 
```ini
[local]
localhost ansible_connection=local

[allhosts]
ansible-node1
ansible-node2
```

The group "local" is for configuring local host, please don't touch this. 

## Ping remote hosts
The "ping" module can be used to test the connections with managed nodes.
```bash
ansible allhosts -m ping
```
The command above asks Ansible to ping all the hosts in the "allhosts" group just added in the /etc/ansibl/hosts. You'll see output like:
```text
ansible-node2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    }, 
    "changed": false, 
    "ping": "pong"
}
ansible-node1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    }, 
    "changed": false, 
    "ping": "pong"
}
```

You can ping hosts in multiple groups or even all the hosts at the same time. Try the following commands and see the differences. 
```bash
ansible all -m ping
ansible local:allhosts -m ping
```

## Run ad-hoc command
You can run any kind of commands on managed nodes.
```bash
ansible allhosts -a 'echo -e Hi you!'
```

You'll see output which shows that command has run on all hosts in "allhosts" group.
```text
ansible-node2 | CHANGED | rc=0 >>
Hi you!
ansible-node1 | CHANGED | rc=0 >>
Hi you!
```

