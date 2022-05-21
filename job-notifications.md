---
layout: default
title: "Job notifications"
permalink: /job-notifications
parent: Jobs
nav_order: 11
---

The `notifications` section of job's JSON description can be used to enable emails to be sent when jobs finish. Add the following lines
to the job description:
```
"notifications":[
   {
      "event":"jobFinished",
      "type":"email"
   }
]
```
Currently the only supported `event` is `jobFinished` and the only supported notification `type` is `email`.
