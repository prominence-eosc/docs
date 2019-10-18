---
layout: single
classes: wide
title: "Quick start"
permalink: /quick-start
sidebar:
  nav: "docs"
---

## First steps

Here we demonstrate a quick way to start using PROMINENCE with the command line interface. We assume the user wants to run PROMINENCE on a host on which they do not have root access. For more installation options, see [here](/docs/installation).


Install the PROMINENCE CLI in a new virtual environment:
```
$ virtualenv ~/.virtualenvs/prominence
$ source ~/.virtualenvs/prominence/bin/activate
$ pip install prominence
```

Two environment variables need to be set which define the URLs of the PROMINENCE and OpenID Connect (OIDC) servers:
```
$ export PROMINENCE_URL=https://...
$ export PROMINENCE_OIDC_URL=https://...
```

Register as an OIDC client:
```
$ prominence register
```
The text `Client registration successful` should appear. This step only needs to be done once.

Now obtain a token in order to be able to authenticate with PROMINENCE:
```
$ prominence login
```
Here you will be asked to go to a web page provided by the OIDC server. After logging in you will need to enter the provided device code. One this is done `Successfully retrieved token` will appear. The tokens are short-lived: they currently expire after 1 hour.

You are now ready to use PROMINENCE.

For a very simple job you can just specify a container image and the command to execute (and any required arguments). For example, here we run a [container](https://hub.docker.com/r/docker/whalesay/) from the Docker demo tutorial:
```
$ prominence create docker/whalesay "cowsay boo"
Job created with id 22071
```

You can check the status of the job using the `prominence list` command (replace the job ID as appropriate):
```
$ prominence list
ID      NAME   CREATED               STATUS      ELAPSED      IMAGE             CMD       
22071          2019-05-23T12:13:59   deploying                docker/whalesay   cowsay boo
```
The job will initially be in the *idle* state then will progress through the *deploying* then *running* state, and finally end up in the *completed* state.
If the job is no longer visible from `prominence list` it means that the job has completed. In this case the command `prominence list --completed` will enable the job status to be seen.

Once the job has finished running you can look at the job's standard output:
```
$ prominence stdout 22071
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

