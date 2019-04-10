---
layout: single
classes: wide
title: "Running jobs"
permalink: /running-jobs
sidebar:
  nav: "docs"
---

## A basic single-node job
In order to run an instance of a container, running the command defined in the image’s entrypoint, all you need to do is to specify the Docker Hub image name:
```
prominence create alahiff/testpi
```

> output

```
Job created with id 3101
```
When a job has been successfully submitted an (integer) ID will be returned. Alternatively, a command (and arguments) can be specified. For example:
``` 
prominence create alahiff/testpi "/bin/sleep 100"
``` 
The command of course should exist within the container. If arguments need to be specified you should put the command and any arguments inside a single set of double quotes, as in the example above.

As alternatives to a Docker Hub image name, a URL pointing to a Singularity image or Docker tarball can be specified.

## MPI jobs

To run an MPI job, you need to specify either `--openmpi` for OpenMPI or `--mpich` for MPICH. For multi-node jobs the number of nodes required should also be specified. For example:
```
prominence create --openmpi --nodes 4 alahiff/openmpi-hello-world:latest /mpi_hello_world
```

## Resources
By default a job will be run with 1 CPU and 1 GB memory but this can easily be changed using the `--cpus` and `--memory` options. A disk size can also be specified using `--disk`. Here is an example running an MPI job on 4 nodes where each node has 2 CPUs and 8 GB memory, there is a shared 20 GB disk accessible by all 4 nodes, and the maximum runtime is 1000 minutes:
```
prominence create --openmpi --nodes 4 --cpus 2 --memory 8 --disk 20 --runtime 1000 alahiff/geant4mpi:1.3a3
```
By default a 10 GB disk is available to jobs, which is located on separate block storage, i.e. not on a VM’s OS disk. For MPI jobs the disk is available across all nodes running the job. The default maximum runtime is 720 minutes.
