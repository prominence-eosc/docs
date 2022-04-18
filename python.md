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
from prominence import ProminenceClient

# Specify the required resources
resources = {
    'cpus': 1,
    'memory': 1,
    'disk': 10,
    'nodes': 1
}

# Define a task
task = {
    'image': 'busybox',
    'cmd': 'echo Hello'
}

# Define a job
job = {
    'name':'Test Job',
    'resources': resources,
    'tasks': [task]
}

# Submit the job
client = ProminenceClient(authenticated=True)
id = client.create_job(job)
print('Job submitted with id', id)
```
Note that the `authenticated=True` means that it is assumed that retrieval of a token will be handled externally (e.g. by the CLI), and that the token should be obtained from a file.

## Listing jobs
Here are some common examples:
```python
from prominence import ProminenceClient

client = ProminenceClient(authenticated=True)

# List currently active jobs
print(client.list_jobs())

# List last completed job
print(client.list_jobs(completed=True))

# List the last 4 completed jobs
print(client.list_jobs(completed=True, num=4))

# List all jobs with label app=hello
print(client.list_jobs(all=True, constraint='app=hello'))
```

## Job descriptions
Job descriptions can easily be obtained using `describe_job`, for example:
```python
from prominence import ProminenceClient

client = ProminenceClient(authenticated=True)

# Get a job description
job = client.describe_job(387)
print('Job status is', job['status'])
```

## Standard output and error
The standard output and error from a job can be obtained using `stdout_job` and `stderr_job`, for example:
```python
from prominence import ProminenceClient

client = ProminenceClient(authenticated=True)

# Get the stdout from a job
print(client.stdout_job(387))
```

