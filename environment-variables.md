---
layout: default
title: "Environment variables"
permalink: /environment-variables
parent: Jobs
nav_order: 5
---
# Environment variables

## Making environment variables available to jobs
It is a common technique to use environment variables to pass information, such as configuration options, into a container. The option `--env` can be used to specify an environment variable in the form of a key-value pair separated by "=". This option can be specified multiple times to set multiple environment variables. For example:
```
prominence create --env LOWER=4.5 --env UPPER=6.7 test/container
```

## Default environment variables
The following environment variables will be set by default:
* PROMINENCE_CPUS: the number of CPUs available (which could be larger than what was requested)
* PROMINENCE_MEMORY: the amount of memory in GB available (which could be larger than what was requested)
* PROMINENCE_NODES: the number of nodes in the job
* PROMINENCE_NODE_NUM: the id of the node in a multi-node job, starting from 0
* PROMINENCE_CONTAINER_RUNTIME: the container runtime in use, either `singularity` or `udocker`
* PROMINENCE_JOB_ID: the id of the job
* PROMINENCE_WORKFLOW_ID: the id of the associated workflow, if applicable
* PROMINENCE_URL: URL of the PROMINENCE REST API
* PROMINENCE_TOKEN: token which can be used to authenticate against the PROMINENCE REST API (a unique token is generated per job, and is valid
for the lifetime of the job)
