---
layout: default
title: "Python client"
permalink: /python-client
parent: Command line interface
nav_order: 8
---
# Python client

*Under development, will be available in 0.18.0 onwards*

The CLI also provides Python classes which can be used for creating and managing jobs and data, and can be installed easily from PyPI:
```
pip install prominence
```
A valid access token is of course required. A token can be obtained in the usual way for the CLI (i.e. `prominence login`), or alternatively
the Python client will work easily from within a PROMINENCE job, as all jobs are provided with a token valid for the lifetime of the job. This
is picked up automatically by the Python client.

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

##### to_dict()
Returns a dictionary representing the task.

## Resources

**class Resources(cpus=1, memory=1, disk=10, nodes=1, walltime=720)**

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

##### to_dict()
Returns a dictionary representing the resources.

## Artifact

**class Artifact(name, directory_name=None, mount_point=None)**

This class represents an artifact.

* Parameters:
    * **name**: (str) Name of artifact.
    * **directory_name**: (optional str) Name of directory when uncompressed.
    * **mount_point**: (optional str) Name of mount point

##### upload(filename)
Uploads the file `filename`.

* Parameters
    * **filename**: (str) Name of file to upload.

##### to_dict()
Returns a dictionary representing the artifact.

## InputFile

**class InputFile(filename, content=None)**

This class represents a small input file to be included in a job description.

* Parameters:
    * **filename**: (str) Filename.
    * **content**: (optional str) Create the input file using this content, either ASCII or binary.

##### to_dict()
Returns a dictionary representing the input file.

## JobPolicies

**class JobPolicies()**

##### *property* maximum_task_retries
Maximum number of times a task will be retried in the event of failures. By default there will be no retries.

##### *property* maximum_retries
Maximum number of times a job will be retried in the event of failures. By default there will be no retries.

##### *property* maximum_time_in_queue
Maximum time in minutes the job will remain idle in the queue. If a job cannot be run immediately it will wait in the queue (up to the specified time limit) until resources become available. The value -1 means that the job will remain in the queue until it starts running. The default value 0 means that the job will remain in the queue until it starts running or there is a failure.

##### *property* priority
Integer enabling users to sort their jobs to determine which will be run first. Large values denote better priority. Note that this is used for influencing the order in which jobs are run, rather than guaranteeing the order.

##### *property* leave_in_queue
By default completed, failed, deleted and killed jobs are only visible from the CLI when `--completed` is specified. When `leaveInQueue` is set to `True` these jobs will remain visible without needing `--completed` and need to be explicitly removed from the queue. If a job isn’t removed from the queue manually it will be automatically removed after 90 days.

##### *property* ignore_task_failures
Normally if a task fails (i.e. exit code non-zero) no further tasks will be executed in a job. If set to `True`, all tasks will be executed.

##### *property* run_serial_tasks_on_all_nodes
By default serial tasks are only run on one node for the case of multi-node jobs. Setting this to `True` results in serial tasks being run on all nodes.

##### *property* report_job_success_on_task_failure
When the job is run as part of a workflow, if the exit code of any tasks are non-zero the job will be reported as running successfully. This means that if retries are enabled within a workflow or a workflow is re-run, only jobs which failed because of infrastructure problems will be retried (e.g. problems pulling the container image or staging files in or out). The default value is `False`.

##### to_dict()
Returns a dictionary representing the policies.

## Notification

**class Notification(event=None, type=None)**

This class represents a notification, i.e. an action triggered by a change in a job or workflow's state.

##### *property* event
When to send the notification. Currently the options are `jobFinished` or `workflowFinished`.

##### *property* type
Type of notification. Currently the only option is `email`.

##### to_dict()
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
Submit the job.

##### wait(timeout=0)
Wait for job to enter a terminal state.

* Parameters:
    * **timeout**: (optional int) Maximum time to wait in seconds. By default there is no timeout (`timeout = 0`).

##### done()
Check if the job has completed.

* Returns: `False` if the job is still running or idle, or `True` if the job is in a terminal state.
* Return type: bool

##### delete()
Delete the job.

* Returns: `True` if the job was successfully deleted.
* Return type: bool

##### remove()
Removes the job from the queue.

* Returns: `True` if the job was successfully removed.
* Return type: bool

##### execute(command)
Execute the specified command in a running job.

* Parameters:
    * **command**: (str) Command and arguments to execute.

##### get_snapshot(path, save_as=None)
Create and retrieve a snapshot of a file within a running job.

* Parameters:
    * **path**: (str) Name of file or directory in job.
    * **save_as**: (optional str) Save the snapshot in a file with this name.

##### to_dict()
Returns a dictionary representing the job.

##### stdout(node=0)
Retrieves the job's standard output.
* Parameters:
    * **node** (optional int): for multi-node jobs this specifies the node
* Returns: the job's standard output
* Return type: str

##### stderr(node=0)
Retrieves the job's standard error.
* Parameters:
    * **node** (optional int): for multi-node jobs this specifies the node
* Returns: the job's standard error
* Return type: str

##### get_input_file(name)
* Parameters:
    * **name**: (str) name of input file
* Returns: the content of the input file
* Return type: str
   
##### get_output_file(name, save_as=None)
* Parameters:
    * **name**: (str) name of output file
    * **save_as**: (optional str) save as a file with this name
* Returns: the content of the output file
* Return type: str

##### get_output_directory(name, save_as=None)
* Parameters:
    * **name**: (str) name of input file
    * **save_as**: (optional str) save as a file with this name
* Returns: the content of the output directory
* Return type: str

## Workflow

**class Workflow()**

This class represents a workflow.

##### *property* name

##### *property* id

##### *property* status

##### *property* labels

##### *property* jobs

##### *property* dependencies

##### *property* factories

##### *property* policies

##### *property* notifications

##### create()
Submit the workflow.

##### wait(timeout=0)
Wait for workflow to enter a terminal state.

* Parameters:
    * **timeout**: (optional int) Maximum time to wait in seconds. By default there is no timeout (`timeout = 0`).

##### done()
Check if the job has completed.

* Returns: `False` if the workflow is still running or idle, or `True` if the workflow is in a terminal state.
* Return type: bool

##### to_dict()
Returns a dictionary representing the workflow.

##### delete()
Delete the workflow.

* Returns: `True` if the workflow was successfully deleted.
* Return type: bool

##### remove()
Removes the workflow from the queue.

* Returns: `True` if the workflow was successfully removed.
* Return type: bool

##### rerun()
Reruns any failed jobs in the workflow.

* Returns: id of the new workflow.
* Return type: integer

## WorkflowPolicies

**class WorkflowPolicies()**

##### *property* maximum_retries
Maximum number of times a job in the workflow will be retried in the event of failures. By default there will be no retries.

##### *property* leave_in_queue
By default completed, failed, deleted and killed workflows are only visible from the CLI when `--completed` is specified. When `leaveInQueue` is set to `True` these workflows will remain visible without needing `--completed` and need to be explicitly removed from the queue. If a workflow isn’t removed from the queue manually it will be automatically removed after 90 days.
