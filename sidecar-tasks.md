---
layout: single
classes: wide
title: "Sidecar tasks"
permalink: /sidecar-tasks
sidebar:
  nav: "docs"
---

Sidecar tasks are tasks which run in parallel to the standard tasks. A job can contain one or more
optional sidecar tasks which will run until they complete or until the standard tasks complete,
in which case they will be killed. Sidecar tasks are run in a different container to the main
task(s) but have access to the same filesystem.
A typical use case is to monitor the main task(s).
