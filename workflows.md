---
layout: single
classes: wide
title: "Introduction to workflows"
permalink: /workflows
sidebar:
  nav: "docs"
---

## Types of workflows

There are several types of workflows available in PROMINENCE.

### Groups of jobs
A workflow can consist of a group of independent jobs. Unlike directed acyclic graphs (below), there are no dependencies between the jobs. Running a group of jobs in this way allows them to be more easily managed than submitting a group of jobs independently.

### Directed acyclic graphs
A directed acyclic graph (DAG) is a collection of all the jobs you want to run organised in a way that reflects their dependencies.

### Job factories
A job factory allows many similar jobs to be created from a job template, for example a parameter sweep.

## Failing jobs in workflows
If any jobs in a workflow fail, the workflow will stop running once there are no more jobs which can be run. Any dependencies of jobs which fail will not be executed.

The `rerun` command can be used to re-run any failed jobs in a workflow, for example:
```
prominence rerun 37300
```
This will retry jobs which had previously failed, and execute any dependencies which were not run previously due to the failed jobs. This command can be used multiple times if necessary. Failing jobs include:
* Jobs which exited with an exit code of anything other than zero (except for jobs which have the policy `reportJobSuccessOnTaskFailure` set to `True`),
* Jobs which failed due to infrastructure or network problems.
