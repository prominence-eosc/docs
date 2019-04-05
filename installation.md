---
layout: single
classes: wide
title: "Installation"
permalink: /installation
sidebar:
  nav: "docs"
---

The PROMINENCE CLI can be installed from PyPI, or if preferred, it can be run using containers. It is possible for normal users to install the PROMINENCE CLI without having to request any assistance from their system administrators.

## Using pip

The PROMINENCE CLI can be installed on a host by typing the following:
```
sudo pip install prominence
```
It can also be installed in a new virtual environment, e.g.
```
virtualenv ~/.virtualenvs/prominence
source ~/.virtualenvs/prominence/bin/activate
pip install prominence
```

## Using Singularity

An alternative is to use Singularity, if available, and create an alias for the command `prominence`. Firstly pull the Docker image:
```
singularity pull docker://alahiff/prominence
```
This will create a file `prominence.simg`.  An alias can be created by putting the following in your `~/.bashrc`: 
```
alias prominence="singularity run <path>/prominence.simg"
```
where the full path to the container image should be specified.

## Using udocker

Similarly to Singularity, another alternative is to use udocker. Because udocker can be installed as an unprivileged user, this method can be used to allow the PROMINENCE CLI to be used on a UI or login node which does not have Singularity installed.

Firstly install udocker if necessary:
```
curl https://raw.githubusercontent.com/indigo-dc/udocker/master/udocker.py > udocker
chmod u+rx ./udocker
./udocker install
```
and move the file `udocker` to somewhere in your PATH. See [here](https://github.com/indigo-dc/udocker/blob/master/doc/installation_manual.md) for more information.

Once udocker is installed, pull the image and create a container:
```
udocker pull alahiff/prominence
udocker create --name=prominence alahiff/prominence:latest
```
An alias for the `prominence` command can be created by putting the following in your `~/.bashrc`: 
```
alias prominence="udocker run --bindhome --hostenv prominence"
```

