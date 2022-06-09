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
* `leaveInQueue`: by default completed, failed, and deleted workflows are only visible from the CLI when `--completed` is specified. When
`leaveInQueue` is set to `True` these workflows will remain visible without needing `--completed` and need to be explicitly removed from the queue. If a workflow isn't removed from the queue manually it will be automatically removed after 90 days.
