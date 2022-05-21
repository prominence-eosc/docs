---
layout: default
title: "Workflows"
permalink: /workflows
nav_order: 5
has_children: true
has_toc: false
---
# Workflows

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

Note that when a workflow is re-run a new workflow id is created, for example:
```
prominence rerun 72444 
```

> output

```
Workflow created with id 72582
```
After re-running a workflow the new workflow id should be used for checking the status. The original workflow id will only report the original number
of succesful jobs.

## Cloning workflows

If a workflow fails it's also possible to re-run the original workflow using `prominence clone`. This creates a copy of an existing workflow, for
example, to clone a workflow with id 51243:
```
prominence clone workflow 51243
```
The cloned workflow is assinged a new id. `prominence clone` can be used for a workflow in any state.
