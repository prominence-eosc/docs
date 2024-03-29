---
layout: default
title: "Help"
permalink: /help
parent: Command line interface
nav_order: 3
---
# Help
The main help page gives a list of all the available commands:
```
prominence --help
```

> output

```
usage: prominence [-h] [--version]
                  {register,login,run,rerun,clone,create,list,describe,delete,remove,stdout,stderr,exec,snapshot,upload,download,ls,rm,usage,kv}
                  ...

PROMINENCE - run jobs in containers across clouds

positional arguments:
  {register,login,run,rerun,clone,create,list,describe,delete,remove,stdout,stderr,exec,snapshot,upload,download,ls,rm,usage,kv}
                        sub-command help
    register            Register as a client with the OIDC server
    login               Get a token from the OIDC server
    run                 Create a job or workflow from JSON in a file or URL
    rerun               Re-run all failed jobs from a completed workflow
    clone               Create a new job or workflow identical to a previously-submitted one
    create              Create a job
    list                List jobs or workflows
    describe            Describe a job or workflow
    delete              Delete a job or workflow
    remove              Remove a job or workflow from the queue
    stdout              Get standard output from a running or completed job
    stderr              Get standard error from a running or completed job
    exec                Execute a command inside a job
    snapshot            Create and download a snapshot of a file or directory in a running job
    upload              Upload a file to transient storage
    download            Download output files from a completed job or workflow
    ls                  List output files
    rm                  Delete a file uploaded to object storage
    usage               Return historical usage information
    kv                  Interact with the key-value store

optional arguments:
  -h, --help            show this help message and exit
  --version             show the version number and exit
```
Help is also available for individual commands, e.g.
```
prominence describe --help
```

> output

```
usage: prominence describe [-h] [--completed] [{job,workflow}] id

positional arguments:
  {job,workflow}  Resource type
  id              Job id

optional arguments:
  -h, --help      show this help message and exit
  --completed     Describe a job or workflow in the completed state
```

