---
layout: single
classes: wide
title: "Workflows"
permalink: /defining-workflows
sidebar:
  nav: "docs"
---

## Overview
In order to submit a workflow the first step is to write a JSON description of the workflow. This is just a list of the definitions of the individual jobs (which can be created easily using `prominence create --dry-run`, see [here](/docs/generating-json)) along with the dependencies between them. The basic structure is:
```json
{
  "name":"test-workflow-1",
  "jobs":[
    {...},
    {...}
  ],
  "dependencies":[
    {"parents":[...], "children":[...]},
    ...
  ]
}
```
Each of the individual jobs must have defined names as these are used in order to define the dependencies.

It is important to note that the resources requirements for the individual jobs can be (and should be!) specified. This will mean that each step in a workflow will only use the resources it requires.

## Examples
It is worthwhile to look at some simple examples in order to understand how to define workflows.

### Multiple steps
Here we consider a simple workflow consisting of multiple sequential steps, e.g.

![Multi-step workflow](multi-step-workflow.png)

In this example `job_A` will run first, followed by `job_B`, finally followed by `job_C`.
```json
{
  "name": "multi-step-workflow",
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
          "runtime": "singularity"
        }
      ],
      "name": "job_A"
    },
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
          "runtime": "singularity"
        }
      ],
      "name": "job_B"
    },
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
          "runtime": "singularity"
        }
      ],
      "name": "job_C"
    }
  ],
  "dependencies": [
    {
      "parents": [
        "job_A"
      ],
      "children": [
        "job_B"
      ]
    },
    {
      "parents": [
        "job_B"
      ],
      "children": [
        "job_C"
      ]
    }
  ]
}
```

### Scatter-gather
He we consider the common type of workflow where a number of jobs can run in parallel. Once these jobs have completed another job will run. Ttypically this final step will take output generated from all the previous jobs. For example:

![Scatter-gather workflow](scatter-gather-workflow.png)

