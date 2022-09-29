---
layout: default
title: "Job description files"
permalink: /job-description-files
parent: Jobs
nav_order: 10
---
# Job description files

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## Generating JSON job descriptions

When `prominence create` is run with the `--dry-run` option, the job will not be submitted but the JSON description of the job will be printed to standard output. For example:
```
prominence create --dry-run --name test1 --cpus 4 --memory 8 --disk 20 busybox "echo Hello"
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
      "runtime": "singularity",
      "cmd": "echo Hello"
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

## YAML job and workflow descriptions
Even though the PROMINENCE REST API only accepts JSON, the CLI accepts both YAML job and workflow descriptions.
This can be useful when you want to specify complex or multiple commands in tasks or small scripts. As with JSON, the
`prominence run` command can be used to create jobs and workflows from YAML.

As a simple example, here is the above test job expressed in YAML:
```yaml
name: test1
resources:
  nodes: 1
  cpus: 4
  memory: 8
  disk: 20
tasks:
  - image: busybox
    runtime: singularity
    cmd: echo Hello
```
Unlike with JSON job descriptions, escaping is not required in the `cmd`, for example:
```yaml
name: test1
resources:
  nodes: 1
  cpus: 4
  memory: 8
  disk: 20
tasks:
  - image: busybox
    runtime: singularity
    cmd: /bin/bash -c "echo Hello; date"
```
If you want to run multiple commands in the same task the following can be done:
```yaml
name: test1
resources:
  nodes: 1
  cpus: 4
  memory: 8
  disk: 20
tasks:
  - image: busybox
    runtime: singularity
    cmd: |
      echo Hello
      date
```
The CLI will convert this into two tasks: the first running `echo Hello` and the second `date`. Alternatively, if the first line
begins with a shebang, the CLI will automatically create an input file and the task will execute this input file. For example:
```yaml
name: test1
resources:
  nodes: 1
  cpus: 4
  memory: 8
  disk: 20
tasks:
  - image: busybox
    runtime: singularity
    cmd: |
      #!/bin/bash
      echo Hello
      date
```
This avoids users having to manually create scripts and include them as input files.

## Tips

### Input files
To include small input files in a job you can list files prepended with `file://`. When the job is submitted using `prominence run` the appropriate file(s) will be included correctly in the JSON or YAML job description and uploaded. This results in cleaner job description files but with the limitation that they now depend on the
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


### Submission using JSON or YAML from URLs
Instead of specifying a local file, `prominence run` also works with URLs. For example:
```
prominence run https://raw.githubusercontent.com/prominence-eosc/examples/master/lammps-bench-single.json
```

## Examples

### Specifying resources
Here we give some example `resources` blocks.

#### Jobs with fixed resources
A job requiring 2 nodes, with 16 CPUs, 32 GB memory and 18 GB disk per node:
```json
"resources":{
   "nodes":2,
   "disk":18,
   "cpus":16,
   "memory":32
}
```
Alternatively, memory per CPU can be specified:
```json
"resources":{
   "nodes":2,
   "disk":18,
   "cpus":16,
   "memoryPerCpu":2
}
```

#### Jobs with moldable resources
A single-node job requiring anywhere between 16 and 32 CPUs, with 2 GB memory per CPU and 4 GB disk:
```json
"resources":{
   "nodes":1,
   "disk":4,
   "cpusRange":[
      16,
      32
   ],
   "memoryPerCpu":2
}
```
A single-node job requiring either 14 or 28 CPUs, with 2 GB memory per CPU and 4 GB disk:
```json
"resources":{
   "nodes":1,
   "disk":4,
   "cpusOptions":[
      14,
      28 
   ],
   "memoryPerCpu":2
}
```
A multi-node job requiring between 16 and 32 CPUs in total, with between 4 and 8 CPUs per node, 2 GB memory per CPU and 10 GB disk per node:
```json
"resources":{
   "disk":10,
   "cpusRange":[
      4,
      8
   ],
   "totalCpusRange":[
      16,
      32
   ],
   "memoryPerCpu":2
}
```
A multi-node job requiring between 16 and 32 CPUs in total, with 4 or 8 CPUs per node, 2 GB memory per CPU and 10 GB disk per node:
```json
"resources":{
   "disk":10,
   "cpusOptions":[
      4,
      8
   ],
   "totalCpusRange":[
      16,
      32
   ],
   "memoryPerCpu":2
}
```

## Visualisation

JSON job scriptions can be pasted [here](https://jsoncrack.com) for visualisation, which may be helpful for some users. This site displays
standard JSON as graphs.
