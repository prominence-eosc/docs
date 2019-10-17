---
layout: single
classes: wide
title: "Job factories"
permalink: /job-factories
sidebar:
  nav: "docs"
---

Currently the following types of job factories are available:
* **parametric sweep**: a set of jobs is created by substituting a range of values into a template

When a workflow using job factory is submitted to PROMINENCE individual jobs will automatically be created. The job names will be of the form `<workflow name>/<job name>/<id>` where `<id>` is an integer.

**Note:** Not all jobs will be created immediately as there is a limit to the number of idle jobs that can exist for any individual workflow. The remaining jobs will be created as the idle jobs start running.
{: .notice--info}

## Parametric sweep
A set of jobs is created by substituting a range of values into a template job. Substitutions can be made in the command to be executed or the values obtained using environment variables. Here's an example fragment which would need to be included in a workflow description:
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
The name of the parameter is given by `name`. In this example the parameter `frame` is varied between the value `start` and at most `end` in increments of `step`.

Jobs can obtain the value of the parameter through the use of substitutions or environment variables.
If job's command was to include `$frame` or `${frame}`, this would be substituted by the appropriate value. An environment variable `PROMINENCE_PARAMETER_frame`
would also be available to the job containing this value.

Additional `parameterSets` can be included in order to carry out multi-dimensional parameter sweeps. For example:
```json
"factory": {
  "type": "parametricSweep",
  "parameterSets":[
    {
      "name": "x",
      "start": 1,
      "end": 4,
      "step": 3
    },
    {
      "name": "y",
      "start": 2,
      "end": 5,
      "step": 4
    },
    {
      "name": "z",
      "start": 3,
      "end": 6,
      "step": 4
    }
 ]
}
```

Here is simple complete example of a 1D parametric sweep job:
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

