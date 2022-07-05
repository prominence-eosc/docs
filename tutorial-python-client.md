---
layout: default
title: "Using the Python client"
permalink: /tutorial-python-client
parent: Tutorials
nav_order: 4
---
# Python client
See [here](/docs/python-client) for full documentation on the Python client.

## Simplest possible job
In this first example we execute a command in a container and print the standard output:
```
task = Task()
task.image = 'docker/whalesay'
task.runtime = 'singularity'
task.command = 'cowsay boo'

job = Job()
job.resources = Resources()
job.tasks.append(task)
job.create()
job.wait()
print(job.stdout())
```
Here `job.create()` creates the job and `job.wait()` blocks until the job enters a terminal state.
