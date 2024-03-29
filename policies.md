---
layout: default
title: "Job policies"
permalink: /job-policies
parent: Jobs
nav_order: 12
---
# Job policies
The `policies` section of a job's JSON description enables users to have more control of how jobs are managed and influence where they will be executed. The available options are:
* `maximumRetries`: maximum number of times a job will be retried in the event of failures. By default there will be no retries.
* `maximumTaskRetries`: maximum number of times a task will be retried in the event of failures. By default there will be no retries.
* `maximumTimeInQueue`: maximum time in minutes the job will remain idle in the queue. If a job cannot be run immediately it will wait in the queue (up to the specified time limit) until resources become available. The value `-1` means that the job will remain in the queue until it starts running. The default value `0` means that the job will remain in the queue until it starts running or there is a failure.
* `leaveInQueue`: by default completed, failed, deleted and killed jobs are only visible from the CLI when `--completed` is specified. When
`leaveInQueue` is set to `True` these jobs will remain visible without needing `--completed` and need to be explicitly removed from the queue. If a job isn't removed from the queue manually it will be automatically removed after 90 days.
* `placement`: allows users to specify requirements and preferences to influence where jobs will run.
* `ignoreTaskFailures`: normally if a task fails (i.e. exit code non-zero) no further tasks will be executed in a job. If set to `True`, all tasks will be executed.
* `reportJobSuccessOnTaskFailure`: When the job is run as part of a workflow, if the exit code of any tasks are non-zero the job will be reported as running successfully. This means
that if retries are enabled within a workflow or a workflow is re-run, only jobs which failed because of infrastructure problems will be retried (e.g. problems pulling
the container image or staging files in or out).
The default value is `False`.
* `autoScalingType`: if set to `null` or the explicit string value `none` only existing resources will be considered to run the job and no additional resources will be provisioned.
* `runSerialTasksOnAllNodes`: by default serial tasks are only run on one node for the case of multi-node jobs. Setting this to `True` results in serial tasks
being run on all nodes.
* `priority`: integer enabling users to sort their jobs to determine which will be run first. Large values denote better priority. Note that this is used for influencing the order in which jobs are run, rather than guaranteeing the order. Use a DAG workflow in order to guarantee the order
in which jobs are run.

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
It's also possible to specify preferences. In the example below we specify that we want the job to run on any of the
sites `OpenStack-Alpha`, `OpenStack-Beta` or `OpenStack-Delta`, and furthermore we want `OpenStack-Alpha` to be used in 
preference (if possible), followed by `OpenStack-Beta` and finally followed by `OpenStack-Delta`.
```
"policies": {
  "placement": {
    "requirements": {
      "sites": [
        "OpenStack-Alpha", "OpenStack-Beta", "OpenStack-Delta"
      ]
    },
    "preferences": {
      "sites": [
        "OpenStack-Alpha", "OpenStack-Beta", "OpenStack-Delta"
      ]
    }
  }
}
```
Preferences can also be specified for regions.
