---
layout: single
classes: wide
title: "Job factories"
permalink: /job-factories
sidebar:
  nav: "docs"
---

## Overview

### Parametric sweep
Here is simple example of a pametric sweep.
```
{
  "name": "ps-workflow",
  "jobs": [
    {
      "resources": {
        "nodes": 1,
        "cpus": 1,
        "memory": 1,
        "disk": 10
      },
      "tasks": [
        {
          "image": "busybox",
          "runtime": "singularity",
          "cmd": "echo $frame"
        }
      ],
      "name": "render"
    }
  ],
  "factory": {
    "type": "parametricSweep",
    "parameterSets":[
      {
        "name": "frame",
        "start": 1,
        "end": 4,
        "step": 1
      }
   ]
  }
}
```
