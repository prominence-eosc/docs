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
* Input files and artifacts
* Required resources (e.g. CPU cores, memory)
* Output files
* Storage details (e.g. mounting B2DROP or OneData)
* One or more task definitions

Tasks execute sequentially within a job, and consist of the following:
* Container image
* Container runtime
* Command to run and optionally any arguments
* Environment variables

A workflow consists of one or more jobs and the dependencies between them. Jobs within a workflow can be executed sequentially, in parallel or combinations of both.
