---
layout: single
classes: wide
title: "Storage options"
permalink: /storage-options
sidebar:
  nav: "docs"
---

There are currently several options to access data in PROMINENCE, and more are under development.

## Cloud storage
Here we make use of central Ceph-based storage using the Swift API. Users can upload data which can be accessed by jobs (including container images), and output files from jobs can be copied to the storage. PROMINENCE is able to provide temporary URLs to access the data.

## POSIX-like access to data
It is possible for jobs to access data in a POSIX-like way, i.e. like a filesystem. Users can specify the mountpoint to be used inside the container.

**Note**: Currently this is only possible using the udocker container runtime.
{: .notice--warning}

### B2DROP
In order to mount your B2DROP storage in jobs firstly an app username and password needs to be created. This can be done by going to https://b2drop.eudat.eu/settings/user/security clicking *Create new app password*.

### OneData
In order to mount your OneData storage in jobs firstly an access token needs to be created using the *Access tokens* menu in the OneData web interface.

