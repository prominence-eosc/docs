---
layout: single
classes: wide
title: "Job lifecycle"
permalink: /job-lifecycle
sidebar:
  nav: "docs"
---

The state diagram below shows the possible job states.

![Job lifecycle](job-states.png)

List of all possible job states:
`CREATED`<br/>
the job is not yet running and has not yet been considered for deployment.

`DEPLOYING`<br/>
The infrastructure to run the job is being deployed.

* __created__: the job is not yet running and has not yet been considered for deployment.
* __deploying__: the infrastructure to run the job is being deployed.
* __idle__: the job is not yet runnng because the infrastructure has been deployed but has not yet been fully contextualized.
* __running__: the job is runing.
* __deleted__: the job has been deleted by the user.
* __killed__: the job has been forcefully terminated, for example it had been running for too long.
* __completed__: the job has completed, however note that the exit status may or may not be 0.
* __failed__: the job failed, for example the infrastructure could not be deployed successfully or the container image could not be pulled.

Note that jobs can transition from the *deploying* or *idle* states directly to the *failed* state in the event of problems.

