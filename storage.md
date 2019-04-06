---
layout: single
classes: wide
title: "Storage options"
permalink: /storage-options
sidebar:
  nav: "docs"
---

## Cloud storage
There are currently several options to access data in PROMINENCE, and more are under development.

## POSIX-like access to data
It is possible for jobs to access data in a POSIX-like way, i.e. like a filesystem. Users can specify the mountpoint to be used inside the container.

**Note**: Currently this is only possible using the udocker container runtime.
{: .notice--warning}

### B2DROP
In order to mount your B2DROP storage in jobs firstly an application username and password needs to be created.

### OneData
In order to mount your OneData storage in jobs firstly a token needs to be created.

