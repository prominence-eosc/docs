---
layout: default
title: "Running commands in running jobs"
permalink: /job-exec
parent: Jobs
nav_order: 8
---
# Running commands in jobs
The `exec` command can be used to execute an arbitrary command within a running job. Usage is of the form:
```
prominence exec <job id> <command>
```
Use cases for this could include checking for the existence of output files, or looking at the content of output or log files.

Current limitations:
* Only standard output is returned.
* The command is not executed inside the job's container.
* The current working directory of the command is the job's scratch directory.
