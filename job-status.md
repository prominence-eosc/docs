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
prominence describe 139
```

> output

```json
[
  {
    "id": 139,
    "status": "idle",
    "image": "alahiff/testpi:latest",
    "cpus": 1,
    "memory": 1,
    "nodes": 1,
    "disk": 10,
    "runtime": 720,
    "events": {
      "creation": "2018-08-29T15:40:08Z"
    }
  }
]
```
To show information about completed jobs, both the `list` and `describe` commands accept a `--completed` option. For example, to list the last 2 completed jobs:
```
prominence list --completed --num 2
```

> output

```
ID     STATUS      IMAGE                       CMD          ARGS
2980   completed   alahiff/tensorflow:1.11.0   python       models-1.11/official/mnist/mnist.py --export_dir mnist_saved_model
2982   completed   alahiff/tensorflow:1.11.0   python       models-1.11/official/mnist/mnist.py --export_dir mnist_saved_model
```
Note that jobs which are completed or have been removed for some reason may be visible briefly without using the `--completed` option.

