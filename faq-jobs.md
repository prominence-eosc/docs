---
layout: single
classes: wide
title: "Frequently asked questions (jobs)"
permalink: /faq-jobs
sidebar:
  nav: "faq-jobs"
---

## Nested containers
The simplest way to have a containerised job execute containers itself is to use the Singularity runtime and
make use of udocker inside the Singularity container.
Below is a trivial example demonstrating installing and running a udocker container inside a Singularity container:
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

