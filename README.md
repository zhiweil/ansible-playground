# Ansible Playground
This project spins up a docker-compose environments which allows you to run Ansible playbooks without worring about setting up any VM. 
SSH is also setup between Ansible master and remote nodes. 

## Prerequesite
Install the following tools before running the examples in this repository. You may also need to install some dependecies before installing these tools. 
* [Docker CE](https://docs.docker.com/engine/install/)
* [Docker Compose](https://docs.docker.com/compose/install/)

## Environment Overview
The environment comprises of three docker containers running on a docker network 172.16.238.0/24. 
* docker container "ansible-master" is the Ansible master node which runs Ubuntu with Ansible already installed.
* docker containers "ansible-node1" and "ansible-node2" are the Ubuntu systems to be configured.

![Ansible Playground Overview](./ansible-playground.png)

## Create Environment
* To start up a new environment:
```bash
docker-compose build
docker-compose up
```

* To tear down an environment
```bash
docker-compose down
```

## Verify SSH connections
Log into ansible-master and SSH onto the other two remote nodes.
```bash
# Log into ansible-master docker image
docker exec -it ansible-master bash

# SSH to ansible-node1, you'll be asked to confirm that you want to continue if you do this the first time.
ssh ansible-node1

# quit SSH connection to ansible-node1
exit

# SSH to ansible-node2. Again, you'll be asked to confirm that you want to continue if you do this the first time.
ssh ansible-node2

# quit SSH connection to ansible-node2
exit

# quit the connection to ansible-master, you'll return to your host
exit
```

