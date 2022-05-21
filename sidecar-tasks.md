---
layout: default
title: "Sidecars"
permalink: /sidecars
parent: Jobs
nav_order: 12
---

Sidecar tasks are tasks within a job which run in parallel to the standard tasks. A job can contain one or more
optional sidecar tasks which will run until they complete or until the standard tasks complete,
at which point the sidecar tasks will be killed. Sidecar tasks are run in a different container to the main
task(s) but have access to the same filesystem.

Note that if multiple sidecar tasks are defined within a single job there are all run in parallel. This behaviour
is different to standard tasks which are run sequentially.

A typical use case for sidecars is to monitor the main task(s), i.e. collecting metrics and sending them to a
time-series database (see [here](/docs/time-series)).

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
