---
layout: default
title: "Frequently asked questions"
permalink: /faq-jobs
parent: Jobs
nav_order: 18
---
# Frequently asked questions
## Nested containers
The simplest way to have a containerised job execute containers itself is to define a PROMINENCE task using
the Singularity runtime and
make use of udocker inside the Singularity container.

Below is a trivial example with a job which installs and runs a udocker container inside a Singularity container:
```
name: udocker-in-singularity
resources:
  nodes: 1
  cpus: 1
  memory: 1
  disk: 10
tasks:
  - image: centos:7
    runtime: singularity
    cmd: |
      #!/bin/bash
      curl -L https://github.com/indigo-dc/udocker/releases/download/v1.3.1/udocker-1.3.1.tar.gz > udocker-1.3.1.tar.gz
      tar zxf udocker-1.3.1.tar.gz
      export PATH=`pwd`/udocker:$PATH
      udocker -q run eoscprominence/testpi
```
Of course, for real use cases a container image can be built with udocker pre-installed.

## Resources allocated to a job
Using `prominence describe` it's possible to see the resources (CPU cores, memory, disk and number of nodes) allocated to a job. This
can be done both while a job is running and once it has completed. This information is available in the `provisionedResources`
section of `execution`, for example:
```
...
  "execution": {
    "provisionedResources": {
      "cpus": 32,
      "disk": 10,
      "memory": 64,
      "nodes": 2
    },
...
```
This can be particularly useful for jobs where fixed sets of resources were not specified.

## Singularity apps
Singularity supports providing multiple *apps* within a single container (see e.g. [this](https://docs.sylabs.io/guides/3.7/user-guide/definition_files.html#apps)) and
this is supported by PROMINENCE with a task `app` attribute.
For example, here is an example running an app called `foo`:
```
{
  "resources": {
    "nodes": 1,
    "disk": 10,
    "cpus": 1,
    "memory": 1
  },
  "name": "singularity-app-test",
  "tasks": [
    {
      "image": "multiapp.sif",
      "runtime": "singularity",
      "app": "foo"
    }
  ]
}
```
