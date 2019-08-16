---
layout: single
classes: wide
title: "Jobs, tasks and workflows"
permalink: /jobs-tasks-workflows
sidebar:
  nav: "docs"
---

A job in PROMINENCE consists of the following:
* Name
* Labels
* Input files
* Output files
* Required resources (e.g. CPU cores, memory, disk)
* One or more task definitions
* Policies (e.g. how many times should failing tasks should be retried)

Tasks execute sequentially within a job, and consist of the following:
* Container image
* Container runtime
* Command to run and optionally any arguments
* Environment variables
* Working directory

A workflow consists of one or more jobs and any dependencies between them. Jobs within a workflow can be executed sequentially, in parallel or combinations of both.

An example workflow, including how it is made up of jobs and tasks, is shown below:

![Tasks and jobs within a workflow](prominence-tasks-jobs-workflows.png)

## Environment variables available to jobs
The following environment variables will be set by default:
* PROMINENCE_CPUS: the number of CPUs available (which could be larger than what was requested)
* PROMINENCE_MEMORY: the amount of memory in GB available (which could be larger than what was requested)
* PROMINENCE_CONTAINER_RUNTIME: the container runtime in use
