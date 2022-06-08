---
layout: default
title: "Example workflows"
permalink: /example-workflows
parent: Workflows
nav_order: 9
---
# Example workflows

## Identical jobs with different input files
Suppose you have a collection of small input files, e.g. `input-0.txt`, `input-1.txt`, `input-2.txt`, `input-3.txt`, and `input-4.txt`,
and want to run the same command for each input file. One way of doing this is to use a workflow with a parameter sweep job factory.

A tarball can easily be created containing all the input files, e.g.
```
tar czvf inputs.tgz input-*.txt
```
and upload to object storage:
```
prominence upload --filename inputs.tgz --name inputs.tgz
```
A workflow can then be created in order to run the same job for each input file, for example:
```
{
   "name":"running-different-input-files",
   "jobs":[
      {
         "resources":{
            "nodes":1,
            "disk":10,
            "cpus":1,
            "memory":1
         },
         "name":"job",
         "artifacts":[
            {
               "url":"inputs.tgz"
            }
         ],
         "tasks":[
            {
               "image":"centos:7",
               "runtime":"singularity",
               "cmd":"echo input-${id}.txt"
            }
         ]
      }
   ],
   "factories":[
      {
         "type":"parameterSweep",
         "name":"sweep",
         "jobs":[
            "job"
         ],
         "parameters":[
            {
               "name":"id",
               "start":0,
               "end":4,
               "step":1
            }
         ]
      }
   ]
}
```

