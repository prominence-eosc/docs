---
layout: single
classes: wide
title: "Job policies"
permalink: /job-policies
sidebar:
  nav: "docs"
---

The `policies` section of a job's JSON description enables jo
* `maximumRetries`: maximum number of times a failing job will be retried
* `maximumTimeInQueue`: maximum time in minutes the job will remain in the queue
* `leaveInQueue`: when set to `true` (default is `false`) completed, failed and deleted jobs will remain in the queue until the user specifies that they can be removed
* `placement`: allows users to specify requirements and preferences to influence where jobs will run
