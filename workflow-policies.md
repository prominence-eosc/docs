---
layout: default
title: "Workflow policies"
permalink: /workflow-policies
parent: Workflows
nav_order: 6
---
# Workflow policies
The `policies` section of a worklow's JSON description enables users to have more control of how jobs in the workflow are managed and influence where they will be executed. The available options are:

* `maximumRetries`: maximum number of times a failing job will be retried. By default a failing job will not be retried.

