---
layout: default
title: "Frequently asked questions (jobs)"
permalink: /faq-jobs
parent: Jobs
nav_order: 16
---

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
