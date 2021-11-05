---
layout: single
classes: wide
title: "Output files in workflows"
permalink: /workflow-outputs
sidebar:
  nav: "docs"
---

When a workflow consists of jobs which have output files and/or directories defined it is possible to download the output from all jobs
in a single command by specifying the workflow id, e.g.:
```
prominence download workflow 9283
```
For many situations it might be useful for the output from each job to be put into a unique directory. For example, the following command:
```
prominence download --dir workflow 9283
```
will download the output associated with each job into directories named after each job id.

**Note:** Unless the `--force` option is specified, any existing output files/directories will not be overwritten.
{: .notice--info}
