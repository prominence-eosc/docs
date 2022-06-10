---
layout: default
title: "Example workflows"
permalink: /example-workflows
parent: Workflows
nav_order: 9
---
# Example workflows

## Identical jobs with different input files

### Small files
Suppose you have a collection of small input files, e.g. `input-0.txt`, `input-1.txt`, `input-2.txt`, `input-3.txt`, and `input-4.txt`,
and want to run the same command for each input file. One way of doing this is to use a workflow with a parameter sweep job factory.

A tarball can easily be created containing all the input files, e.g.
```
tar czvf inputs.tgz input-*.txt
```
and uploaded to object storage:
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
               "cmd":"/bin/bash -c \"cat input-${id}.txt > output-${id}.out\""
            }
         ],
         "outputFiles": [
             "output-${id}.out"
         ],
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
Here we just cat each file as a simple example, but the same idea can easily be applied to more complex use cases. Here we also
specify a unique output file per job, where the filename depends on the parameter used. However, note that it is possible to use
the same output filename for all jobs in a workflow if wanted.

### Large files
For the situation of large input files it would not make sense to make a tarball containing all files. Instead, only the required
input files should be provided to each job.

One method would be to upload each file to object storage, for example:
```
prominence upload --filename large-input-0.txt --name large-input-0.txt 
prominence upload --filename large-input-1.txt --name large-input-1.txt
prominence upload --filename large-input-2.txt --name large-input-2.txt
```
Note that the names can be arbitrary and don't need to contain an integer which increments, as in this example we make use of a
zip job factory rather than a parameter sweep. This means we can specify an explicit list of filenames. For example:
```
{
   "name":"running-different-large-input-files",
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
               "url":"$filename"
            }
         ],
         "tasks":[
            {
               "image":"centos:7",
               "runtime":"singularity",
               "cmd":"cat $filename"
            }
         ]
      }
   ],
   "factories":[
      {
         "type":"zip",
         "name":"zip",
         "jobs":[
            "job"
         ],
         "parameters":[
            {
               "name":"filename",
               "values":[
                  "large-input-0.txt",
                  "large-input-1.txt",
                  "large-input-2.txt"
               ]
            }
         ]
      }
   ]
}
```
