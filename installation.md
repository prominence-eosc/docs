---
layout: default
title: "Installation"
permalink: /installation
parent: Command line interface
nav_order: 2
---
# Installation
The PROMINENCE CLI can be installed from PyPI, or if preferred, it can be run using containers. It is possible for normal users to install the PROMINENCE CLI without having to request any assistance from their system administrators.

## Using pip

### With sudo or root access
The PROMINENCE CLI can be installed on a host by typing the following:
```
sudo pip install prominence
```

### As a normal user without using virtualenv
The PROMINENCE CLI can be installed in a user's home directory by running:
```
pip install --user prominence
```

### As a normal user using virtualenv
If `virtualenv` is not available it can be installed in a user's home directory by typing:
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py --user
pip install --user virtualenv
```
The PROMINENCE CLI can be installed in a new virtual environment, e.g.
```
virtualenv ~/.virtualenvs/prominence
source ~/.virtualenvs/prominence/bin/activate
pip install prominence
```

## Using Singularity

An alternative is to use Singularity, if available, and create an alias for the command `prominence`. Firstly pull the Docker image:
```
singularity pull docker://eoscprominence/cli
```
This will create a file `cli_latest.sif`.  An alias can be created by putting the following in your `~/.bashrc`: 
```
alias prominence="singularity run <path>/cli_latest.sif"
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
udocker pull eoscprominence/cli
udocker create --name=prominence eoscprominence/cli:latest
```
An alias for the `prominence` command can be created by putting the following in your `~/.bashrc`: 
```
alias prominence="udocker run --bindhome --hostenv prominence"
```

## Checking what version is installed
The version of the PROMINENCE CLI installed can be checked by running:
```
prominence --version
```

