---
layout: default
title: "Quick start"
permalink: /quick-start
parent: Overview
nav_order: 2
---
# Quick start
## First steps

Here we demonstrate a quick way to start using PROMINENCE with the command line interface. We assume the user wants to run PROMINENCE on a host on which they do not have root access. For more installation options, see [here](/docs/installation).


Install the PROMINENCE CLI in a new virtual environment:
```
$ python3 -m venv venv
$ source venv/bin/activate
$ pip install prominence
```

Register as an OIDC client:
```
$ prominence register
```
The text `Client registration successful` should appear. This step only needs to be done once per machine. For example, if you want to run the CLI from both your desktop and a laptop this will need to be run once on each.

Now obtain a token in order to be able to authenticate with PROMINENCE:
```
$ prominence login
```
Here you will be asked to go to a web page provided by the OIDC server. After logging in you will need to enter the provided device code. One this is done `Successfully retrieved token` will appear. The tokens are short-lived: they currently expire after 1 hour.

You are now ready to use PROMINENCE.

For a very simple job you can just specify a container image and the command to execute (and any required arguments). For example, here we run a [container](https://hub.docker.com/r/docker/whalesay/) from the Docker demo tutorial:
```
$ prominence create docker/whalesay "cowsay boo"
Job created with id 84637
```
Here the container image name is `docker/whalesay` and the command to be executed is `cowsay boo`.

You can check the status of the job using the `prominence list` command (replace the job ID as appropriate):
```
$ prominence list                     
ID      NAME   CREATED               STATUS   ELAPSED      IMAGE             CMD       
84637          2022-05-20 17:07:35   idle                  docker/whalesay   cowsay boo
```
The job will initially be in the *idle* state then will progress to the *running* state and finally end up in the *completed* state.
If the job is no longer visible from `prominence list` it means that the job has completed. In this case the command `prominence list --completed` will enable the job status to be seen:
```
$ prominence list   --completed
ID      NAME   CREATED               STATUS      ELAPSED      IMAGE             CMD
84637          2022-05-20 17:07:35   completed   0+00:00:33   docker/whalesay   cowsay boo
```

Once the job has finished running you can look at the job's standard output:
```
$ prominence stdout 84637
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

