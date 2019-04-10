---
layout: single
classes: wide
title: "Containers"
permalink: /containers
sidebar:
  nav: "docs"
---

In PROMINENCE all jobs are run in unprivileged containers using user-specified images. It is possible to use either the Singularity or udocker container runtimes.

The image can be specified in the following ways:
* `<hub-user>/<repo-name>:<tag>` (Docker Hub)
* URL for a tarball created by `docker save`
* URL for a Singularity image

Container registries other than Docker Hub should also work, provided authentication is not required.

Note that if a Docker tarball (with a filename ending in ".tar") is specified udocker will automatically be selected as the container runtime, and if a Singularity image is specified (with a filename ending in ".simg") Singularity will automatically be selected.

## Tips for creating containers
Some important tips for creating containers to be used with PROMINENCE:
* Do not put any software or required files in /root, since containers are run as an unprivileged user
* Do not put any software or required files in /home or /tmp, as these directories in the container image will be replaced when the container is executed
* Do not specify USER in your Dockerfile when creating the container image
* The environment variables HOME, TMP and TEMP will be set to a scratch directory visible inside the container when the container is executed. For the case of multi-node MPI jobs this scratch directory is accessible across all nodes running the job.
* Do not expect to be able to write inside the container's filesystem. Write any files into the default current working directory, or into the directory specified by the environment variables HOME, TMP and TEMP.

### MPI jobs
Some are some additional requirements on the container images for MPI jobs:
* "mpirun" should be in available inside the container and in the PATH
* The "ssh" command should be installed inside the container

There is no reason to set an entrypoint as it will not be used. A command (and any required arguments) must be specified.

A simple minimal starting point for a Dockerfile for a CentOS 7 container image for OpenMPI is:
```
FROM centos:7
RUN yum -y install openssh-clients openmpi openmpi-devel

ENV PATH            /usr/lib64/openmpi/bin:${PATH}
ENV LD_LIBRARY_PATH /usr/lib64/openmpi/lib:${LD_LIBRARY_PATH}
```
and for MPICH:
```
FROM centos:7
RUN yum -y install openssh-clients mpich mpich-devel

ENV PATH            /usr/lib64/mpich/bin:${PATH}
ENV LD_LIBRARY_PATH /usr/lib64/mpich/lib:${LD_LIBRARY_PATH}
```

