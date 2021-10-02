---
layout: single
classes: wide
title: "Deployment quick start"
permalink: /deployment-quick-start
sidebar:
  nav: "docsa"
---
A simple model for deployment is to use two machines. The services are divided up as follows:
* PROMINENCE REST API, HTCondor, Nginx
* IMC, Infrastructure Manager, Open Policy Agent, PostgreSQL, MariaDB, Nginx

## Frontend

## Backend
Here we will assume the machine has Ubuntu installed.

### PostgreSQL
PostgreSQL is used as the database backed for IMC. Run the following to install the PostgreSQL server:
```
apt install postgresql
```
Create a user `imc` by running:
```
sudo -u postgres createuser --interactive
```
Create a database `imc` by running:
```
sudo -u postgres createdb imc
```
Set the password for the imc user:
```
sudo -i -u postgres
psql
```

### Install and setup MySQL
MySQL is used as the database backend for Infrastructure Manager. Run the following to install the MariaDB server:
```
apt-get install software-properties-common
apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
add-apt-repository 'deb http://mariadb.mirror.liquidtelecom.com/repo/10.4/ubuntu xenial main'
apt update
apt install mariadb-server
```
Note that the default version of MariaDB available in Ubuntu 16.04 is too old.

Set a root password and secure the installation by running:
```
/usr/bin/mysql_secure_installation
```
Create a user and database to be used by IM:
```
create database im;
CREATE USER 'im'@'%' IDENTIFIED BY '<password>';
GRANT ALL ON im.* TO 'im'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

### Infrastructure Manager

### Open Policy Agent
Make a policies directory for Open Policy Agent:
```
mkdir -p /etc/prominence/policies
cd /etc/prominence/policies
wget https://raw.githubusercontent.com/prominence-eosc/imc/master/policies/imc.rego
```
Run Open Policy Agent:
```

```

### IMC
#### Service
Copy the example configuration file https://raw.githubusercontent.com/prominence-eosc/imc/master/imc.ini to `/etc/prominence`.

#### Periodic cleanup and checks cron
Create a file `/etc/cron.d/prominence` containing:
```
0 * * * * root docker run -i --rm -e PROMINENCE_IMC_CONFIG_DIR=/etc/prominence -v /etc/prominence:/etc/prominence:ro -v /var/log/imc:/var/log/imc --net=host eoscprominence/imc-periodic-checks:latest >> /dev/null 2>&1
```

### Nginx
This is only necessary if IMC needs to be secured with TLS.

