---
layout: default
title: "Input files"
permalink: /input-files
parent: Jobs
nav_order: 3
---
# Input files
## Small input files
Files can be uploaded from the host running the CLI and made available to jobs using the `--input` option. For example:
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

## Input artifacts
Users can upload data to the object storage which can be staged-in to jobs.

### Uploading artifacts
Users can upload artifacts (including container images) using the `upload` command. The path of the file to be uploaded needs to be specified in addition to a reference name for the file which will be used later in order to access the file.
```
prominence upload --name myfile --filename /data/myfile
```
It is entirely up to the user to ensure that the names do not clash, however different users can use the same names without problems. Note that the name is arbitrary and doesn't have to be the same as the actual filename.

**Note:** There is a limit of 5 GB for a single file.
{: .notice--warning}

### Accessing data
For the case of input files, include an artifact using the name specified when the file was uploaded, e.g.
```
prominence upload --name input.txt --filename input.txt
prominence create --artifact input.txt busybox "cat input.txt"
```
Any such input files will be automatically copied from object storage before the job starts running.

For the case of a container image, define the image name using the name specified when the image was uploaded, e.g.
```
prominence upload --name myimage.sif --filename myimage.sif
prominence create myimage.sif
```

**Note:** The existence of input files and container images on object storage is checked upon job submission. Job submission will fail and an error will be returned if an input file or container image does not exist.
{: .notice--info}

### Listing files which have been uploaded
There is an `ls` command which enables files which have been uploaded to be listed:
```
prominence ls -l
```
The `-l` option results in object sizes and last modification dates being listed in addition to object names.

