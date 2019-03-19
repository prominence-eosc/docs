# Quick start
Here we demonstrate running all PROMINENCE components on a single node. Each service is run in a Docker container. To keep things as simple as possible here we use host networking so everything can communicate over localhost.

## Setup
Copy the repository containing the required example config files:
```
```
Generate a passwordless ssh key to be used by Ansible:
```
cd 
ssh-keygen -t rsa -b 4096 -P '' -f id_rsa_ansible
```

## Start the services
Infrastructure Manager:
```
docker run -d \
           --name=prominence-im \
           --net=host \
           grycap/im:latest
```
Open Policy Agent:
```
docker run -d \
           --name=prominence-opa \
           --net=host \
           -v ./prominence-quick-start/policies:/policies \
           openpolicyagent/opa:latest run --server /policies
```

IMC:
```
docker run -d \
           --name prominence-imc \
           --net=host \
           -e PROMINENCE_IMC_CONFIG_DIR=/etc/prominence \
           -v ./prominence-quick-start/imc:/etc/prominence \
           alahiff/prominence-imc:latest
```           
