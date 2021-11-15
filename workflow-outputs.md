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

## Parameters in output files or directories
Parameters defined in job factories can be used in output files or directories. For example, if you have a parameter `$workdir` jobs
can make use of `$workdir` in the name of the output directory:
```json
{
   "factories":[
      {
         "jobs":[
            "scan"
         ],
         "name":"scan",
         "parameters":[
            {
               "name":"workdir",
               "values":[
                  "point_000",
                  "point_001",
                  "point_002"
               ]
            }
         ],
         "type":"zip"
      }
   ],
   "jobs":[
      {
         "name":"scan",
         "outputDirs":[
            "$workdir"
         ],
         "resources":{
            "cpus":1,
            "disk":10,
            "memory":1,
            "nodes":1,
            "walltime":43200
         },
         "tasks":[
            {
               "cmd":"/bin/sh -c \"mkdir -p $workdir ; echo $workdir > $workdir/in\"",
               "image":"centos7",
               "runtime":"singularity"
            }
         ]
      }
   ]
}
```
In order to download all output from these jobs once the workflow has completed, run:
```
prominence download workflow <workflow id>
```
