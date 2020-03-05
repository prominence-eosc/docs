---
layout: single
classes: wide
title: "Job policies"
permalink: /job-policies
sidebar:
  nav: "docs"
---

The `policies` section of a job's JSON description enables users to have more control of how jobs are managed and influence where they will be executed. The available options are:
* `maximumRetries`: maximum number of times a failing job will be retried
* `maximumTimeInQueue`: maximum time in minutes the job will remain in the queue
* `leaveInQueue`: when set to `true` (default is `false`) completed, failed and deleted jobs will remain in the queue until the user specifies that they can be removed
* `placement`: allows users to specify requirements and preferences to influence where jobs will run

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
    "maximumTimeInQueue": 600,
    "leaveInQueue": true
  }
}
```

## Placement policies
Job placement policies enable users to influence where jobs will be executed. This consists of requirements and preferences.

To force a job to run at a particular site:
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
To force a job to run at any site in a particular region:
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

