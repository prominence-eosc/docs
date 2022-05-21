---
layout: default
title: "Example jobs"
permalink: /example-jobs
sidebar:
  nav: "docs"
---

## Single node jobs

### Blender
Here we render one frame from the classroom demo (see [here](https://www.blender.org/download/demo-files/)). The required Blender files are downloaded automatically before the job starts, and the output file is saved to cloud storage.
```
prominence create --cpus 8 \
                  --memory 8 \
                  --artifact https://download.blender.org/demo/test/classroom.zip \
                  --output frame_001.png \
                  ikester/blender:latest \
                  "/usr/local/blender/blender -b classroom/classroom.blend -o frame_### -f 1"
```

### OpenFOAM
Here we run a variation of the [windAroundBuildings tutorial from OpenFOAM v6](https://github.com/OpenFOAM/OpenFOAM-6/tree/master/tutorials/incompressible/simpleFoam/windAroundBuildings) from [here](https://github.com/CFDEngine/of-on-aws-tests):
```
prominence create --cpus 8 \
                  --memory 8 \
                  --artifact https://github.com/alahiff/of-on-aws-tests/archive/master.zip \
                  --workdir of-on-aws-tests-master/windAroundBuildings_02 \
                  --env OMPI_MCA_orte_tmpdir_base=/tmp openfoam/openfoam6-paraview56 \
                  "/bin/bash -c \"source /opt/openfoam6/etc/bashrc; ./Allrun; cat log.simpleFoam\""
```
We use one of the official OpenFOAM Docker images and obtain the example code from GitHub. The main script executed ("Allrun") itself runs Open MPI within the container.

### Tensorflow (CPU)
Here we train Tensorflow to classify the MNIST dataset. The output file containing the saved model (`saved_model.pb`) will be made available on cloud storage. This example demonstrates running an external code (in this case obtained from GitHub) using a generic container image.
```
prominence create --cpus 8 \
                  --memory 8 \
                  --artifact https://github.com/tensorflow/models/archive/v1.11.tar.gz \
                  --env PYTHONPATH="\$PYTHONPATH:models-1.11" \
                  --output "mnist_saved_model/*/saved_model.pb" \
                  alahiff/tensorflow:1.11.0 \
                  "python models-1.11/official/mnist/mnist.py --export_dir mnist_saved_model"

```
The Docker image used in this example is just the tensorflow/tensorflow:1.11.0 image but with the Python requests module installed, as it is needed in this example.


### LAMMPS
Here we run one of the [LAMMPS](https://lammps.sandia.gov/) benchmark problems using Intel's Singularity image:
```
prominence create --cpus 2 \
                  --memory 2 \
                  --artifact https://lammps.sandia.gov/inputs/in.lj.txt \
                  --runtime singularity \
                  shub://intel/HPC-containers-from-Intel:lammps \
                  "/lammps/lmp_intel_cpu_intelmpi -in in.lj.txt"
```
See [here](https://github.com/intel/HPC-containers-from-Intel/tree/master/containers/lammps) for more information about this container image.

## MPI jobs

### Open MPI hello world
Here we run a basic "Hello World" job using Open MPI and the udocker container runtime:
```
prominence create --openmpi --runtime=udocker --cpus 2 --nodes 2 alahiff/openmpi-hello-world:latest /mpi_hello_world
```

