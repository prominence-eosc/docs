---
layout: single
classes: wide
title: "Jobs with private images"
permalink: /tutorial-jobs-private-images
sidebar:
  nav: "docs"
---

Here we give an example of running a job which requires a private container image, input files and output files. We assume that the container image needs to be kept private and therefore cannot be put on Docker Hub.

## The container image

### Saving the image into a file
We will use the object storage integrate with PROMINENCE for storing the container image. The image will only be visible to the user who uploads it and to the user's jobs which require it. Because the image will be stored in object storage rather than a container registry the image needs to be in the form of a single file. This can either be a Docker archive (`.tar`) or Singularity Image Format (`.sif` or `.simg`). Note that images in the Singularity Image Format are typically much smaller than Docker archives.

A Docker archive can be created using the Docker CLI, for example:
```
docker save centos:7 > centos7.tar
```
An image built using Docker can be saved in the Singularity Image Format easily, for example:
```
singularity build centos7.sif docker-daemon://centos:7
```

**Note:** By default a job (submitted using the PROMINENCE CLI) using an image where the filename ends in `.sif` or `.simg` will be run using the Singularity runtime, and a job using an image where the filename ends in `.tar` will be run using the udocker runtime. The runtime can also be specified using the `--runtime` option to `prominence create`.
{: .notice--info}

### Uploading the image
Using the PROMINENCE CLI the container image can be uploaded to object storage. For example:
```
prominence upload --filename centos.sif --name centos7.sif
```
Here `filename` is the name of the file on your local system, and `name` is an arbitrary name which will be used later to reference the file.

## Input data
Similarly, any private input data required by jobs can be uploaded to object storage using `prominence upload`. For example, suppose you have a file `cadmesh-jet-v1.1.tgz` required by jobs:
```
prominence upload --filename cadmesh-jet-v1.1.tgz --name cadmesh-jet-v1.1.tgz
```

## Submitting the job
We now want to submit a job which runs a command inside the image created in the first step which accesses the input data. Here is a simple example:
```
prominence create --name test --artifact cadmesh-jet-v1.1.tgz centos7.sif "du -a ."
```
Here that we have used the container image name specified about (`centos7.sif`) and the name given to the input data (`cadmesh-jet-v1.1.tgz`). In this case the input file is a gzip compressed tar archive. Before the user's command is executed this file will be decompressed and the files extracted, and made available in the current working directory of the job.

### Small input data files or scripts
For the case of small input data files or scripts (< 1 MB in size) it is not necessary to upload the files to object storage. Instead they can be directly specified when the job is submitted. For example, suppose you have a file `data.txt` containing:
```
0 1 2 A
3 4 5 B
```
and a Python script `test.py` containing:
```
with open('data.txt', 'r') as input_file:
    print(input_file.readlines())
```
you can submit this directly in one line without any additional preparations:
```
prominence create --input data.txt --input test.py python:3 "python3 test.py"
```
This simple example also demonstrates that input files are placed into the current working directory inside the container.

It is of course possible for a job to use both small input files and larger files stored on object storage.

### Output data
If a job generates output data which is required by the user, either use the `--output` option to specify the name of an output file or use `--outputdir` to specify the name of an output directory. For the case of a directory, it will be automatically compressed into a tarball.

Here is a simple example with a single output file `output.nc`:
```
prominence create --name test \
                  --artifact cadmesh-jet-v1.1.tgz \
                  --output output.nc \
                  centos7.sif "touch output.nc"
```
For multiple output files use the `--output` option multiple times, e.g.
```
prominence create --name test \
                  --artifact cadmesh-jet-v1.1.tgz \ 
                  --output output1.nc \
                  --output output2.nc \
                  centos7.sif "/bin/bash -c \"touch output1.nc ; touch output2.nc\""
```
Alternatively, if the job puts all output files into a single directory
```
prominence create --name test \
                  --artifact cadmesh-jet-v1.1.tgz \ 
                  --outputdir output \
                  centos7.sif "/bin/bash -c \"mkdir output ; touch output/file1.txt ; touch output/file2.txt\""
```

## Downloading output data
Once a job has completed the `prominence download` command can be used to download any output files or directories associated with a job, e.g.
```
prominence download <job id>
```

