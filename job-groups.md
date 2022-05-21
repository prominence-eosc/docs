---
layout: default
title: "Groups of jobs"
permalink: /groups-of-jobs
sidebar:
  nav: "docs"
---

In some situations it can be useful to be able to manage a group of jobs as a single entity (a workflow). 
In order to submit a group of indendepent jobs as a workflow the first step is to write a JSON description of the workflow. This is just a list of the definitions of the individual jobs (which can be created easily using `prominence create --dry-run`, see [here](/docs/job-description-files)). The basic structure is:
```json
{
  "name": "test-workflow-1",
  "jobs": [
    {...},
    {...}
  ]
}
```

To run this example, type:
```
prominence run https://raw.githubusercontent.com/prominence-eosc/examples/master/jobs-blender-render.json
```

If we list workflows:
```
$ prominence list workflows
ID    NAME               CREATED               STATUS    ELAPSED      PROGRESS
927   render-classroom   2019-08-15T06:46:41   running   0+00:00:09   0/4
```

Listing jobs:
```
$ prominence list
ID    NAME                      CREATED               STATUS    ELAPSED      IMAGE                    CMD                                                                      
928   render-classroom/frame1   2019-08-15T06:46:45   created                ikester/blender:latest   /usr/local/blender/blender -b classroom/classroom.blend -o frame_### -f 1
929   render-classroom/frame2   2019-08-15T06:46:46   created                ikester/blender:latest   /usr/local/blender/blender -b classroom/classroom.blend -o frame_### -f 2
930   render-classroom/frame3   2019-08-15T06:46:47   created                ikester/blender:latest   /usr/local/blender/blender -b classroom/classroom.blend -o frame_### -f 3
931   render-classroom/frame4   2019-08-15T06:46:48   created                ikester/blender:latest   /usr/local/blender/blender -b classroom/classroom.blend -o frame_### -f 4
```
