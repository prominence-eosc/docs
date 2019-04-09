---
layout: single
classes: wide
title: "Input files"
permalink: /input-files
sidebar:
  nav: "docs"
---

## Input files
Files can be uploaded and made available to jobs using the `--input` option. For example:
```
prominence create --input /home/alahiff/README alahiff/testpi:latest "cat README"
```
The files will be written in the job's current directory, also referred to by the HOME, TMP or TEMP environment variables. Note that large data files should not be provided to jobs this way, and there is a total size limit of 1 MB per file.

## Fetching files and archives
Artifacts can either be obtained from remote URLs before jobs are executed. Using the `--artifact` option you can specify a URL. For example:
```
prominence create --artifact https://raw.githubusercontent.com/giovtorres/docker-centos7-slurm/master/Dockerfile busybox /bin/ls
```
Archives with filenames ending in the following are automatically unpacked:
* .tar
* .tar.gz
* .tgz
* .gz
* .tar.bz2
* .bz2
* .zip

