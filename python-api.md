---
layout: default
title: "Python API"
permalink: /python-api
parent: Overview
nav_order: 8
---
# Python API
The CLI also provides Python classes which can be used for creating and managing jobs and data, and can be installed easily from PyPI:
```
pip install prominence
```
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

**class Resources(cpus=1, memory=1, disk=10, nodes=1, walltime=43200)**

**Resources** represents the resources required for a job.

#### *property* cpus
The number of CPUs.

#### *property* memory
The memory in GB.

#### *property* disk
The disk in GB.

#### *property* nodes
The number of nodes.

#### *property* walltime
The walltime required for the job in mins.

## InputFile

## Policies

## Notification

## Job

