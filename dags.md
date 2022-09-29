---
layout: default
title: "Directed acyclic graphs"
permalink: /dags
parent: Workflows
nav_order: 3
---
# Directed acyclic graphs

## Overview
In order to submit a workflow the first step is to write a JSON description of the workflow. This is just a list of the definitions of the individual jobs (which can be created easily using `prominence create --dry-run`, see [here](/docs/job-description-files)) along with the dependencies between them. Each dependency defines a parent and its children. The basic structure is:
```json
{
  "name": "test-workflow-1",
  "jobs": [
    {...},
    {...}
  ],
  "dependencies": {
    "parent_job": ["child_job_1", ...],
    ...
  }
}
```
Each of the individual jobs must have defined names as these are used in order to define the dependencies. 
Unlike some workflow languages like [CWL](https://www.commonwl.org/) or [WDL](https://github.com/openwdl/wdl), in PROMINENCE
_abstract dependencies_ are used rather than dependencies derived from data flow. This means that
dependencies need to be defined in terms of job names rather than being based on job inputs and outputs. Parent jobs are run before children.

It is important to note that the resources requirements for the individual jobs can be (and must be!) specified. This will mean that each step in a workflow will only use the resources it requires. Jobs within a single workflow can of course request very different resources, which makes it possible for workflows to have both HTC and HPC steps.

By default the number of retries is zero, which means that if a job fails the workflow will fail. Any jobs which depend on a failed
job will not be attempted.
If the number of retries is set to one
or more, if an individual job fails (i.e. exit code is not 0) it will be retried up to the specified number of times.
To set a maximum number of retries, include `maximumRetries` in the workflow definition, e.g.
```json
"policies": {
  "maximumRetries": 2
}
```

## Examples
It is worthwhile to look at some simple examples in order to understand how to define workflows.

### Multiple steps
Here we consider a simple workflow consisting of multiple sequential steps, e.g.

![Multi-step workflow](multi-step-workflow.png)

In this example `job_A` will run first, followed by `job_B`, finally followed by `job_C`. A basic JSON description is shown below:
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
          "runtime": "singularity",
          "cmd": "date"
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
          "runtime": "singularity",
          "cmd": "date"
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
          "runtime": "singularity",
          "cmd": "date"
        }
      ],
      "name": "job_C"
    }
  ],
  "dependencies": {
    "job_A": ["job_B"],
    "job_B": ["job_C"]
  }
}
```
See [here](https://jsoncrack.com/editor?json=%5B%5B%22name%22%2C%22jobs%22%2C%22dependencies%22%2C%22a%7C0%7C1%7C2%22%2C%22multi-step-workflow%22%2C%22resources%22%2C%22tasks%22%2C%22a%7C5%7C6%7C0%22%2C%22nodes%22%2C%22cpus%22%2C%22memory%22%2C%22disk%22%2C%22a%7C8%7C9%7CA%7CB%22%2C%22n%7C1%22%2C%22n%7CA%22%2C%22o%7CC%7CD%7CD%7CD%7CE%22%2C%22image%22%2C%22runtime%22%2C%22cmd%22%2C%22a%7CG%7CH%7CI%22%2C%22busybox%22%2C%22singularity%22%2C%22date%22%2C%22o%7CJ%7CK%7CL%7CM%22%2C%22a%7CN%22%2C%22job_A%22%2C%22o%7C7%7CF%7CO%7CP%22%2C%22job_B%22%2C%22o%7C7%7CF%7CO%7CR%22%2C%22job_C%22%2C%22o%7C7%7CF%7CO%7CT%22%2C%22a%7CQ%7CS%7CU%22%2C%22a%7CP%7CR%22%2C%22a%7CR%22%2C%22a%7CT%22%2C%22o%7CW%7CX%7CY%22%2C%22o%7C3%7C4%7CV%7CZ%22%5D%2C%22a%22%5D) for a visualisation of the above JSON.

### Scatter-gather
He we consider the common type of workflow where a number of jobs can run in parallel. Once these jobs have completed another job will run. Typically this final step will take output generated from all the previous jobs. For example:

![Scatter-gather workflow](scatter-gather-workflow.png)

A basic JSON description is shown below:
```json
{
  "name": "scatter-gather-workflow",
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
          "cmd": "date"
        }
      ],
      "name": "job_A1"
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
          "runtime": "singularity",
          "cmd": "date"
        }
      ],
      "name": "job_A2"
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
          "runtime": "singularity",
          "cmd": "date"
        }
      ],
      "name": "job_A3"
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
          "runtime": "singularity",
          "cmd": "date"
        }
      ],
      "name": "job_B"
    }
  ],
  "dependencies": {
    "job_A1": ["job_B"],
    "job_A2": ["job_B"],
    "job_A3": ["job_B"]
  }
}
```
See [here](https://jsoncrack.com/editor?json=%5B%5B%22name%22%2C%22jobs%22%2C%22dependencies%22%2C%22a%7C0%7C1%7C2%22%2C%22scatter-gather-workflow%22%2C%22resources%22%2C%22tasks%22%2C%22a%7C5%7C6%7C0%22%2C%22nodes%22%2C%22cpus%22%2C%22memory%22%2C%22disk%22%2C%22a%7C8%7C9%7CA%7CB%22%2C%22n%7C1%22%2C%22n%7CA%22%2C%22o%7CC%7CD%7CD%7CD%7CE%22%2C%22image%22%2C%22runtime%22%2C%22cmd%22%2C%22a%7CG%7CH%7CI%22%2C%22busybox%22%2C%22singularity%22%2C%22date%22%2C%22o%7CJ%7CK%7CL%7CM%22%2C%22a%7CN%22%2C%22job_A1%22%2C%22o%7C7%7CF%7CO%7CP%22%2C%22job_A2%22%2C%22o%7C7%7CF%7CO%7CR%22%2C%22job_A3%22%2C%22o%7C7%7CF%7CO%7CT%22%2C%22job_B%22%2C%22o%7C7%7CF%7CO%7CV%22%2C%22a%7CQ%7CS%7CU%7CW%22%2C%22a%7CP%7CR%7CT%22%2C%22a%7CV%22%2C%22o%7CY%7CZ%7CZ%7CZ%22%2C%22o%7C3%7C4%7CX%7Ca%22%5D%2C%22b%22%5D) for a visualisation of the above JSON.
