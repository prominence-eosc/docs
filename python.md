---
layout: single
classes: wide
title: "Using PROMINENCE from Python"
permalink: /python
sidebar:
  nav: "docs"
---

Here we will assume that the PROMINENCE CLI has been installed and `prominence login` has been run to obtain a token.

## Submitting jobs
To submit a job a standard JSON job description is constructed and `create_job` can be used to submit it to PROMINENCE.
Here is a simple example submitting a single job:
```python
from __future__ import print_function
from prominence import ProminenceClient

client = ProminenceClient()

# Specify the required resources
resources = {'cpus':1,
             'memory':1,
             'disk':10,
             'nodes':1}

# Define a task
task = {'iamge':'busybox',
        'cmd':'echo Hello'}

# Define a job
job = {'name':'Test Job',
       'resources':resources,
       'tasks':[task]}

# Submit the job
client = ProminenceClient()
id = client.create_job(job)
print('Job submitted with id', id)
```
