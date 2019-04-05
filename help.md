---
layout: single
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
           {login,create,run,list,describe,delete,upload,download,stdout,stderr}
           ...

Prominence - run jobs in containers across clouds

positional arguments:
  {login,create,run,list,describe,delete,upload,download,stdout,stderr}
                        sub-command help
    login               Login
    create              Create a job or workflow from a JSON file
    run                 Run a job
    list                List jobs or workflows
    describe            Describe a job or workflow
    delete              Delete a job or workflow
    upload              Upload a file to transient storage
    download            Download output files from a completed job or workflow
    stdout              Get standard output from a running or completed job
    stderr              Get standard error from a running or completed job

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

