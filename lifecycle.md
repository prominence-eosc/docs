---
layout: default
title: "Job lifecycle"
permalink: /job-lifecycle
parent: Overview
nav_order: 6
---
# Job lifecycle
The state diagram below shows the possible job states.

![Job lifecycle](job-states.png)

List of all possible job states:
* __idle__: the job is not yet running.
* __deploying__: the infrastructure to run the job is being deployed.
* __waiting__: the job is waiting for appropriate resources to become available.
* __running__: the job is runing.
* __deleted__: the job has been deleted by the user.
* __killed__: the job has been forcefully terminated, for example it had been running for too long.
* __completed__: the job has completed, however note that the exit status may or may not be 0.
* __failed__: the job failed, for example the infrastructure could not be deployed successfully or the container image could not be pulled.

Note that jobs can transition from the __idle__, __deploying__ , and __waiting__ states directly to the __failed__ state in the event of problems.

