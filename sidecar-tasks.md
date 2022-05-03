---
layout: single
classes: wide
title: "Sidecars"
permalink: /sidecars
sidebar:
  nav: "docs"
---

Sidecar tasks are tasks within a job which run in parallel to the standard tasks. A job can contain one or more
optional sidecar tasks which will run until they complete or until the standard tasks complete,
at which point the sidecar tasks will be killed. Sidecar tasks are run in a different container to the main
task(s) but have access to the same filesystem.
A typical use case is to monitor the main task(s), making use of a time-series database (see [here](/docs/time-series)).

In order to define a task to be run as a sidecar set the `type` as `sidecar`. For example:
```json
{
   "name":"sidecar-test-1",
   "resources":{
      "memoryPerCpu":2,
      "cpus":2,
      "disk":10
   },
   "tasks":[
      {
         "image":"centos:7",
         "runtime":"singularity",
         "cmd":"/bin/bash -c \"echo Hello1 ; sleep 5 ; echo Hello2\"",
         "type":"sidecar"
      },
      {
         "image":"centos:7",
         "runtime":"singularity",
         "cmd":"/usr/bin/sleep 30",
         "type":"basic"
      }
   ]
}
```
**Note:** Unlike standard tasks, if multiple sidecar tasks are defined they are all run in parallel rather than sequentially.
{: .notice--info}
