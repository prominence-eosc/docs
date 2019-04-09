---
layout: single
classes: wide
title: "Jobs"
permalink: /jobs
sidebar:
  nav: "docs"
---

### Environment variables
It is common for environment variables to be used to pass information into containers. The option `--env` can be used to specify an environment variable in the form of a key-value pair separated by "=". This option can be specified multiple times to set multiple environment variables. For example:
```
prominence create --env LOWER=4.5 --env UPPER=6.7 test/container
```

### Labels
Arbitrary labels in the form of key-value pairs (separated by "=") can be set using the `--label` option. This option can be used multiple times to set multiple labels. For example:
```
prominence create --label experiment=MASTU --label env=dev test/container
```
Each key and value must be a string of less than 64 characters. Keys can only contain alphanumeric characters (`[a-z0-9A-Z]`) while values can also contain dashes (`-`), underscores (`_`), dots (`.`) and forward slashes (`/`).

## Checking the status of jobs
The `list` command will by default list any active jobs (i.e. jobs which are idle or running):
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

## Deleting a job
Jobs cannot be modified after they are created but they can be deleted. The `delete` command allows you to kill a single job:
```
prominence delete 164
```

> output

```
Success
```

## Viewing standard output and error

The standard output and error from a job can be seen using the `stdout` and `stderr` commands. For example, to get the standard output for the job with id 299:
```
prominence stdout 299
```

> output

```
 _______
< hello >
 -------
    \
     \
      \
                    ##        .
              ## ## ##       ==
           ## ## ## ##      ===
       /""""""""""""""""___/ ===
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
       \______ o          __/
        \    \        __/
          \____\______/

```
**Note:** The standard output and error can be seen while jobs are running as well as once they have completed, allowing users to check the status of long-running jobs. This is not quite realtime, so there can be short delays in the standard output and error being updated.
{: .notice--warning}

