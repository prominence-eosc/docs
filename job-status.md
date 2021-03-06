---
layout: single
classes: wide
title: "Checking job status"
permalink: /job-status
sidebar:
  nav: "docs"
---

## Listing jobs
The `list` command by default lists any active jobs, i.e. jobs which are idle or running:
```
prominence list
```

> output

```
ID     STATUS   IMAGE                       CMD     ARGS
3101   idle     alahiff/testpi
3103   idle     alahiff/cherab-jet:latest   python  batch_make_sensitivity_matrix.py 0 59
3104   idle     ikester/blender:latest      blender -b classroom/classroom.blend -o frame_### -f 1
```

It's also possible to request a list of jobs using a constraint on the labels associated with each job. For example, if you submitted a group of jobs with a label __name=run5__, the following would list all such jobs:
```
prominence list --all --constraint name=run5
```
Here the `--all` option means that both active (i.e. idle or running) and completed jobs will be listed.

## Describing a job
To get more information about an individual job, use the `describe` command, for example:
```
prominence describe 345
```

> output

```json
[
  {
    "id": 345,
    "status": "created",
    "resources": {
      "cpus": 1,
      "disk": 10,
      "memory": 1,
      "nodes": 1,
      "walltime": 720
    },
    "tasks": [
      {
        "image": "alahiff/testpi",
        "runtime": "singularity"
      }
    ],
    "events": {
      "createTime": "2019-06-18T10:16:36"
    }
  }
]
```
To show information about completed jobs, both the `list` and `describe` commands accept a `--completed` option. For example, to list the last 2 completed jobs:
```
prominence list --completed --last 2
```

> output

```
ID     STATUS      IMAGE                       CMD          ARGS
2980   completed   alahiff/tensorflow:1.11.0   python       models-1.11/official/mnist/mnist.py --export_dir mnist_saved_model
2982   completed   alahiff/tensorflow:1.11.0   python       models-1.11/official/mnist/mnist.py --export_dir mnist_saved_model
```
Note that jobs which are completed or have been removed for some reason may be visible briefly without using the `--completed` option.

## Completed jobs
The JSON descriptions of completed jobs contain additional information. This may include:
* __status__: current job status.
* __statusReason__: for jobs in a terminal state other than the completed state this may give a reason for the current status.
* __createTime__: date & time when the job was created by the user.
* __startTime__: date & time when the job started running.
* __endTime__: date & time when the job ended.
* __site__: the site where the job was executed.
* __maxMemoryUsageKB__: the maximum total memory usage of the job, summed over all processes (note this is not available for jobs running on remote HTC or HPC resources)

The following information is also provided for each task:
* __retries__: the number of retries attempted.
* __exitCode__: the exit code returned by the user's job. This would usually would be 0 for success.
* __imagePullTime__: time taken to pull the container image. If a cached image from a previous task was used this will be -1.
* __wallTimeUsage__: wall time used by the task.
* __cpuTimeUsage__: CPU time usage by the task. For a task using multiple CPUs this will be larger than the wall time.
* __maxResidentSetSizeKB__: maximum resident size (in KB) of the largest process


For example:
```json
[
  {
    "id": 281,
    "status": "completed",
    "resources": {
      "cpus": 1,
      "disk": 10,
      "memory": 1,
      "nodes": 1,
      "walltime": 720
    },
    "tasks": [
      {
        "image": "alahiff/testpi",
        "runtime": "singularity"
      }
    ],
    "events": {
      "createTime": "2019-06-18T09:41:57",
      "startTime": "2019-06-18T09:44:41",
      "endTime": "2019-06-18T09:45:06"
    },
    "execution": {
      "site": "OpenStack-STFC",
      "tasks": [
        {
          "exitCode": 0,
          "retries": 0,
          "imagePullTime": 22.73,
          "wallTimeUsage": 0.6311,
          "cpuTimeUsage": 0.504
        }
      ]
    }
  }
]
```
