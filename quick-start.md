# Quick start
Here we demonstrate running all PROMINENCE components on a single node. Each service is run in a container. To keep things as simple as possible here we use host networking so everything can communicate using localhost.

## Setup and configuration
Copy the repository containing the required example config files:
```
```
Generate a passwordless ssh key to be used by Ansible:
```
cd 
ssh-keygen -t rsa -b 4096 -P '' -f id_rsa_ansible
```
Update the ___ini.json__ file to contain the details of a Google service account. For this you will need a username, project name and private key. It will look something like this:
```
{
  "ansible":{},
  "credentials":{},
  "im":{
    "auth":{
      "im":"id = im; type = InfrastructureManager; username = user; password = pass",
      "Google":"id = gce; type = GCE; username = <username>@<project>.iam.gserviceaccount.com; password = <private key>; project = <project>"
    }
  }
}
```
Note that the private key needs to be converted into a single line with newlines replaced with literal "\n". This can be easily done using awk, e.g. `awk -v ORS='\\n' '1' <private key file>`.

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
