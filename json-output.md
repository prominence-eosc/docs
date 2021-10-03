---
layout: single
classes: wide
title: "Generating a JSON job description"
permalink: /generating-json
sidebar:
  nav: "docs"
---

## Generating JSON job descriptions

When `prominence create` is run with the `--dry-run` option, the job will not be submitted but the JSON description of the job will be printed to standard output. For example:
```
prominence create --dry-run --name test1 --cpus 4 --memory 8 --disk 20 busybox
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

If the JSON output is saved in a file it be submitted to PROMINENCE using the `prominence run` command, e.g.:
```
prominence run <filename.json>
```
A workflow can similarly be submitted in the same way using `prominence run`.

## Tips

### Input files
To include small input files in a job you can list files prepended with `file://`. When the job is submitted using `prominence run` the appropriate file(s) will be included correctly in the JSON and uploaded. This results in cleaner JSON files but with the limitation that they now depend on the
existence of the appropriate input files.

For example:
```json
{
  "resources": {
    "nodes": 1,
    "cpus": 4,
    "memory": 8,
    "disk": 20
  },
  "inputs": [
    "file://input.txt"
  ],
  "tasks": [
    {
      "image": "busybox",
      "runtime": "singularity"
    }
  ],
  "name": "test1"
}
```
Absolute or relative paths can be specified.

### Submission using JSON from URLs
Instead of specifying a local file, `prominence run` also works with URLs. For example:
```
prominence run https://raw.githubusercontent.com/prominence-eosc/examples/master/lammps-bench-single.json
```
