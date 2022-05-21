---
layout: default
title: "Jobs and workflows"
permalink: /jobs-and-workflows
sidebar:
  nav: "docs"
---

By default all PROMINENCE CLI commands refer to jobs. However, a number of commands include the ability to specify a resource, which is either a `job` or a `workflow`.

Listing workflows:
```
prominence list workflows
```
Describing a workflow:
```
prominence describe workflow ...
```
Deleting a workflow:
```
prominence delete workflow ...
```
The standard output and error from a job which is part of a workflow can be viewed by specifying both the workflow id and the name of the job, i.e.
```
prominence stdout <id> <job name>
```
To list all the individual jobs from workflow `<id>`:
```
prominence list jobs <id>
```
