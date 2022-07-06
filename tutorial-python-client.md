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

## Directory uploaded to job and retrieved after job execution
In this example we (1) create a tarball of an input directory, (2) upload the tarball to object storage,
(3) create a job, (4) download the updated contents of the directory.
```
import os
import tarfile
from prominence import Artifact, Job, Resources, Task

directory = '/tmp/directory-in'

# Create tarball
tarball = '/tmp/%s.tgz' % os.path.basename(directory)
os.chdir(directory)
os.chdir('../')
with tarfile.open(tarball, "w:gz") as fh:
    fh.add(os.path.basename(directory))

# Upload artifact
artifact = Artifact(os.path.basename(tarball), os.path.basename(directory), '/data')
artifact.upload(tarball)

# Create task
task = Task()
task.image = 'centos:7'
task.runtime = 'udocker'
task.command = '/bin/bash -c \"hostname > /data/hostname.txt\"'
task.workdir = '/data'

# Create task & wait for it to complete
job = Job()
job.resources = Resources(cpus=2, memory=4, disk=2)
job.tasks.append(task)
job.artifacts.append(artifact)
job.output_directories.append(os.path.basename(directory))
job.create()
job.wait()

# Download output directory
job.get_output_directory(os.path.basename(directory), save_as='/tmp/directory-out.tgz')
```
