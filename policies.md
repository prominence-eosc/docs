---
layout: single
classes: wide
title: "Job policies"
permalink: /job-policies
sidebar:
  nav: "docs"
---

The `policies` section of a job's JSON description enables users to have more control of how jobs are managed and influence where they will be executed. The available options are:
* `maximumRetries`: maximum number of times a failing job will be retried. By default a failing job will not be retried.
* `maximumTimeInQueue`: maximum time in minutes the job will remain in the queue. If a job cannot be run immediately it will wait in the queue (up to the specified time limit) until resources become available. The value `-1` means that the job will remain in the queue until it starts running. The default value `0` means that the job will remain in the queue until it starts running or there is a failure.
* `placement`: allows users to specify requirements and preferences to influence where jobs will run.

For example:
```
{
  "tasks": [
    {
      "image": "centos:7", 
      "runtime": "singularity",
      "cmd": date
    }
  ], 
  "name": "test", 
  "resources": {
    "nodes": 1, 
    "disk": 10, 
    "cpus": 1, 
    "memory": 1
  },
  "policies": {
    "maximumRetries": 4,
    "maximumTimeInQueue": 600
  }
}
```

## Placement policies
Job placement policies enable users to influence where jobs will be executed. This consists of requirements and preferences.

To force a job to run at a particular site (`OpenStack-Alpha` in this example):
```
"policies": {
  "placement": {
    "requirements": {
      "sites": [
        "OpenStack-Alpha"
      ]
    }
  }
}
```
To force a job to run at any site in a particular region (`Alpha` in this example):
```
"policies": {
  "placement": {
    "requirements": {
      "regions": [
        "Alpha"
      ]
    }
  }
}
```

