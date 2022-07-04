---
layout: default
title: "Python API"
permalink: /python-api
parent: Overview
nav_order: 8
---
# Python API
Installing the CLI also provides Python classes which can be used for creating and managing jobs and data.

Here is a very simple example where we submit a job, wait for it to complete then print the standard output:
```
# Define a task
task = Task()
task.image = 'centos:7'
task.runtime = 'singularity'
task.command = 'hostname'

# Define a job
job = Job()
job.tasks.append(task)
job.create()

# Wait for job to complete
job.wait()

# Print standard output
print(job.stdout())
```

## Task

## Resources

## InputFile

## Policies

## Notification

## Job

