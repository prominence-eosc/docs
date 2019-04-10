---
layout: single
classes: wide
title: "Generating a JSON job description"
permalink: /generating-json
sidebar:
  nav: "docs"
---

When `prominence create` is run with the `--dry-run` option, the job will not be submitted but the JSON description of the job will be printed to standard output. For example:
```
prominence run --dry-run --name test1 --cpus 4 --memory 8 --disk 20 busybox
```

> output

```json
{
  "resources": {
    "nodes": 1,
    "cpus": 4,
    "memory": 8,
    "disk": 20
  },
  "tasks": [
    {
      "image": "busybox",
      "runtime": "singularity"
    }
  ],
  "name": "test1"
}
```
