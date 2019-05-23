---
layout: single
classes: wide
title: "Quick start"
permalink: /quick-start
sidebar:
  nav: "docs"
---
Here we demonstrate a quick way start using PROMINENCE with the command line interface. We assume the user wants to run PROMINENCE on a host on which they do not have root access. For more installation options, see [here](/docs/installation).


The PROMINENCE CLI can be installed in a new virtual environment by typing the following:
```
virtualenv ~/.virtualenvs/prominence
source ~/.virtualenvs/prominence/bin/activate
pip install prominence
```

Two environment variables need to be set which define the URL of the PROMINENCE server and OpenID Connect (OIDC) server:
```
export PROMINENCE_URL=https://...
export PROMINENCE_OIDC_URL=https://...
```

Register as an OIDC client:
```
prominence register
```
The text `Client registration successful` should appear.

Now obtain a token in order to be able to authenticate with PROMINENCE:
```
prominence login
```
Here you will be asked to go to a web page provided by the OIDC server. After logging in you will need to enter the provided device code. One this is done `Authentication successful` will appear.
You are now ready to use PROMINENCE.

For a very simple job you can just specify a container image and the command to execute (and any required arguments). For example, here we run a [https://hub.docker.com/r/docker/whalesay/](container) from the Docker demo tutorial:
```
prominence create docker/whalesay "cowsay boo"
Job created with id 22071
```

Now you can check the status of the job (replace the job ID as appropriate):
```
prominence list
ID      NAME   CREATED               STATUS      ELAPSED      IMAGE             CMD       
22071          2019-05-23T12:13:59   deploying                docker/whalesay   cowsay boo
```
Once the job has finished running you can look at the standard output:
```
prominence stdout 22071
 _____ 
< boo >
 ----- 
    \
     \
      \     
                    ##        .            
              ## ## ##       ==            
           ## ## ## ##      ===            
       /""""""""""""""""___/ ===        
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~   
       \______ o          __/            
        \    \        __/             
          \____\______/   

```
