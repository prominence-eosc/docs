---
layout: single
classes: wide
title: "Help"
permalink: /help
sidebar:
  nav: "docs"
---

The main help page gives a list of all the available commands:
```
prominence --help
```

> output

```
usage: prominence [-h] [--version]
                  {register,login,run,rerun,create,list,describe,delete,exec,snapshot,upload,download,ls,rm,stdout,stderr,usage}
                  ...

PROMINENCE - run jobs in containers across clouds

positional arguments:
  {register,login,run,rerun,create,list,describe,delete,exec,snapshot,upload,download,ls,rm,stdout,stderr,usage}
                        sub-command help
    register            Register as a client with the OIDC server
    login               Get a token from the OIDC server
    run                 Create a job or workflow from JSON in a file or URL
    rerun               Re-run any failed jobs from a completed workflow
    create              Create a job
    list                List jobs or workflows
    describe            Describe a job or workflow
    delete              Delete a job or workflow
    exec                Execute a command inside a job
    snapshot            Create and download a snapshot of a file or directory
                        in a running job
    upload              Upload a file to transient storage
    download            Download output files from a completed job or workflow
    ls                  List output files
    rm                  Delete a file uploaded to object storage
    stdout              Get standard output from a running or completed job
    stderr              Get standard error from a running or completed job
    usage               Return historical usage information

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

