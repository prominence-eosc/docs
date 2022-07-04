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

**class Task()**

This class represents a task.

##### *property* image
The name of the container image.

##### *property* runtime
The container runtime, either `singularity` or `udocker`.

##### *property* command
The command to execute in the container.

##### *property* type
The type of job, one of `basic`, `openmpi`, `intelmpi`, `mpich`, `sidecar`.

##### *property* workdir
The working directory.

##### *property* env
Environment variables.

##### *property* procs_per_node
The processes required per node for the case of multi-node MPI jobs.

##### json()
Returns a dict representing the task.

## Resources

**class Resources(cpus=1, memory=1, disk=10, nodes=1, walltime=43200)**

This class represents the resources required for a job.

##### *property* cpus
The number of CPUs.

##### *property* memory
The memory in GB.

##### *property* disk
The disk required in GB.

##### *property* nodes
The number of nodes.

##### *property* walltime
The walltime required for the job in minutes.

##### *property* memory_per_cpu
Memory in GB per CPU.

##### json()
Returns a dict representing the resources.

## InputFile

## JobPolicies

##### *property* maximum_task_retries

##### *property* maximum_retries

##### *property* maximum_time_in_queue

##### *property* priority

##### *property* leave_in_queue

##### *property* ignore_task_failures

##### *property* run_serial_tasks_on_all_nodes

##### *property* report_job_success_on_task_failure

##### json()
Returns a dictionary representing the policies.

## Notification

##### *property* event

##### *property* type

##### json()
Returns a dictionary representing the notification.

## Job

**class Job()**

This class represents a job.

##### *property* name

##### *property* id

##### *property* status

##### *property* labels

##### *property* resources

##### *property* tasks

##### *property* input_files

##### *property* artifacts

##### *property* output_files

##### *property* output_directories

##### *property* policies

##### *property* notifications

##### create()

##### wait(timeout=0)

##### done()

##### json()
Returns a dict representation of the job.

##### stdout(node=0)
Retrieves the job's standard output.

##### stderr(node=0)
Retrieves the job's standard error.

##### get_input_file(name)

##### get_output_file(name)

##### get_output_directory(name)
