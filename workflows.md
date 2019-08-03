---
layout: single
classes: wide
title: "Introduction to workflows"
permalink: /workflows
sidebar:
  nav: "docs"
---

There are several types of workflows available in PROMINENCE.

## Bags of jobs
A workflow can consist of a group of independent jobs. Unlike directed acyclic graphs (below), there are no dependencies between them.

## Directed acyclic graphs
A directed acyclic graph (DAG) is a collection of all the jobs you want to run organised in a way that reflects their dependencies.

## Job factories
A job factory allows many jobs to be created from a job template, for example a parameter sweep.
