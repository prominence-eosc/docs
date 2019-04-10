---
layout: single
classes: wide
title: "Workflows"
permalink: /defining-workflows
sidebar:
  nav: "docs"
---

In order to submit a workflow the first step is to write a JSON description of the workflow. This is just a list of the definitions of the individual jobs along with the dependencies between them. The basic structure is:
```json
{
  "name":"workflow",
  "jobs":[
    {...},
    {...}
  ],
  "dependencies":[
    {"parents":[...], "children":[...]},
    ...
  ]
}
```
