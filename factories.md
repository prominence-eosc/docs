---
layout: single
classes: wide
title: "Job factories"
permalink: /job-factories
sidebar:
  nav: "docs"
---

Currently there are the following types of job factories:
* parameteric sweep: a set of jobs is created by substituting a range of values into a template

Each will be describe below.

## Parametric sweep
A et of jobs is created by substituting a range of values into a template job. Substitutions can be made in the command to be executed or the values obtained using environment variables. Here's an example fragment which would need to be included in a workflow description:
```json
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
```
Here we specify the factory to be of type `parametericSweep`. The range of values used to create the jobs is defined in `parameterSets`.
The name of the parameter is given by `name`. In this example the parameter `frame` is varied between the value `start` and `end` in increments of `step`.

Jobs can obtain the value of the parameter through the use of substitutions or environment variables.
If job's command was to include `$frame`, this would be substituted by the appropriate value. An environment variable `PROMINENCE_PARAMETER_frame"
would also be available to the job containing this value.

Here is simple example of a parametric sweep.
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

When listing jobs, the job names will be of the form `<workflow name>/<job name>/<id>` where `<id>` is an integer.

