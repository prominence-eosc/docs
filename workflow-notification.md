---
layout: default
title: "Workflow notifications"
permalink: /workflow-notifications
parent: Workflows
nav_order: 7
---
# Workflow notifications
The `notifications` section of workflow's JSON description can be used to enable emails to be sent when workflows finish. Add the following lines
to the workflow description:
```
"notifications":[
   {
      "event":"workflowFinished",
      "type":"email"
   }
]
```
Currently the only supported `event` is `workflowFinished` and the only supported notification `type` is `email`.
