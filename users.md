# Users

## Containers in PROMINENCE

In PROMINENCE all jobs are run in unprivileged containers using user-specified images. It is possible to use either the Singularity or udocker container runtimes.

The image can be specified in the following ways:
* `<hub-user>/<repo-name>:<tag>` (Docker Hub)
* URL for a tarball created by `docker save` (filename should end in ".tar")
* URL for a Singularity image (filename should end in ".simg")

Container registries other than Docker Hub should also work, provided authentication is not required.

Note that if a Docker tarball (with a filename ending in ".tar") is specified udocker will automatically be selected as the container runtime, and if a Singularity image is specified (with a filename ending in ".simg") Singularity will automatically be selected.

### Tips for creating containers
Some important tips for creating containers to be used with PROMINENCE:
* Do not put any software or required files in /root, since containers are run as an unprivileged user
* Do not put any software or required files in /home or /tmp, as these directories in the container image will be replaced when the container is executed
* Do not specify USER in your Dockerfile when creating the container image
* The environment variables HOME, TMP and TEMP will be set to a scratch directory visible inside the container when the container is executed. For the case of multi-node MPI jobs this scratch directory is accessible across all nodes running the job.

#### MPI jobs
Some are some additional requirements on the container images for MPI jobs:
* "mpirun" should be in available inside the container and in the PATH
* The "ssh" command should be installed inside the container

A minimal starting point for a Dockerfile for a CentOS 7 container image for OpenMPI is:
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

# Job lifecycle
The state diagram below shows the possible job states.
![Job lifecycle](job-states.png)

List of all possible job states:
* __idle__: the job is not yet running and has not yet been considered for deployment.
* __deploying__: the infrastructure to run the job is being deployed.
* __ready__: the job is not yet runnng because the infrastructure has been deployed but has not yet been fully contextualized.
* __running__: the job is runing.
* __deleted__: the job has been deleted by the user.
* __killed__: the job has been forcefully terminated, for example it had been running for too long.
* __completed__: the job has completed, however note that the exit status may or may not be 0.
* __failed__: the job failed, for example the infrastructure could not be deployed successfully or the container image could not be pulled.

Note that jobs can transiton from the deployment or ready states directly to the failed state in the event of problems.


# PROMINENCE CLI

A CLI is provided which presents a simple batch-system style interface to PROMINENCE. It is written in Python and works with both Python 2.x and 3.x.

## Installation
The PROMINENCE CLI can be installed from PyPI, or if preferred, it can be run using containers.

### Using pip

The PROMINENCE CLI can be installed on a host by typing the following:
```
sudo pip install prominence
```
It can also be installed in a new virtual environment, e.g.
```
virtualenv ~/.virtualenvs/prominence
source ~/.virtualenvs/prominence/bin/activate
pip install prominence
```

### Using Singularity

An alternative is to use Singularity, if available, and create an alias for the command `prominence`. Firstly pull the Docker image:
```
singularity pull docker://alahiff/prominence
```
This will create a file `prominence.simg`.  An alias can be created by putting the following in your `~/.bashrc`: 
```
alias prominence="singularity run <path>/prominence.simg"
```
where the full path to the container image should be specified.

### Using udocker

Similarly to Singularity, another alternative is to use udocker. Because udocker can be installed as an unprivileged user, this method can be used to allow the PROMINENCE CLI to be used on a UI or login node which does not have Singularity installed. Firstly install udocker if necessary:
```
curl https://raw.githubusercontent.com/indigo-dc/udocker/master/udocker.py > udocker
chmod u+rx ./udocker
./udocker install
```
and move the file `udocker` to somewhere in your PATH. See [here](https://github.com/indigo-dc/udocker/blob/master/doc/installation_manual.md) for more information.

Once udocker is installed, pull the image and create a container:
```
udocker pull alahiff/prominence
udocker create --name=prominence alahiff/prominence:latest
```
An alias for the `prominence` command can be created by putting the following in your `~/.bashrc`: 
```
alias prominence="udocker run --bindhome --hostenv prominence"
```

## Help pages

The main help page gives a list of all the available commands:
```
prominence --help
```

> output

```
usage: prominence [-h] [--version]
           {login,create,run,list,describe,delete,upload,download,stdout,stderr}
           ...

Prominence - run jobs in containers across clouds

positional arguments:
  {login,create,run,list,describe,delete,upload,download,stdout,stderr}
                        sub-command help
    login               Login
    create              Create a job or workflow from a JSON file
    run                 Run a job
    list                List jobs or workflows
    describe            Describe a job or workflow
    delete              Delete a job or workflow
    upload              Upload a file to transient storage
    download            Download output files from a completed job or workflow
    stdout              Get standard output from a running or completed job
    stderr              Get standard error from a running or completed job

optional arguments:
  -h, --help            show this help message and exit
  --version             show the version number and exit
```
Help is also available for individual commands, e.g.
```
prominence describe --help
```

> output

```
usage: prominence describe [-h] [--completed] [{job,workflow}] id

positional arguments:
  {job,workflow}  Resource type
  id              Job id

optional arguments:
  -h, --help      show this help message and exit
  --completed     Describe a job or workflow in the completed state
```

## Prerequisites
Before using the CLI the following 4 environment variables need to be defined:
```
PROMINENCE_URL
PROMINENCE_OIDC_URL
PROMINENCE_OIDC_CLIENT_ID
PROMINENCE_OIDC_CLIENT_SECRET
```

## Authentication
Before you can run any commands you need to login. This retrieves a time-limited token which is used to authenticate against the PROMINENCE RESTful API.
```
prominence login
```

> output

```
To login, use a web browser to open the page https://<PROMINENCE_OIDC_URL>/device and enter the code ABCDEF when requested
```
The instructions here should be followed, i.e. open the specified page in a web browser, login with your username and password, then type in the 6 character code when requested. After you have given approval to PROMINENCE, the following should appear from the CLI:
```
Authentication successful
```
Currently the token is stored in a JSON file `$HOME/.prominence` readable by the current user only.

## Running jobs

### A basic single-node job
In order to run an instance of a container, running the command specified in the image’s entrypoint, all you need to do is to specify the Docker Hub image name:
```
prominence run alahiff/testpi
```

> output

```
Job created with id 3101
```
When a job has been successfully submitted an (integer) ID will be returned. Alternatively, a command (and arguments) can be specified. For example:
``` 
prominence run alahiff/testpi "/bin/sleep 100"
``` 
The command of course should exist within the container. If arguments need to be specified you should put the command and any arguments inside a single set of double quotes, as in the example above.

As alternatives to a Docker Hub image name, a URL pointing to a Singularity image or Docker tarball can be specified.

### MPI jobs

To run an MPI job, you need to specify either `--openmpi` for OpenMPI or `--mpich` for MPICH. For multi-node jobs the number of nodes required should also be specified. For example:
```
prominence run --openmpi --nodes 4 alahiff/openmpi-hello-world:latest /mpi_hello_world
```

### Resources
By default a job will be run with 1 CPU and 1 GB memory but this can easily be changed using the `--cpus` and `--memory` options. A disk size can also be specified using `--disk`. Here is an example running an MPI job on 4 nodes where each node has 2 CPUs and 8 GB memory, there is a shared 20 GB disk accessible by all 4 nodes, and the maximum runtime is 1000 minutes:
```
prominence run --openmpi --nodes 4 --cpus 2 --memory 8 --disk 20 --runtime 1000 alahiff/geant4mpi:1.3a3
```
By default a 10 GB disk is available to jobs, which is located on separate block storage, i.e. not on a VM’s OS disk. For MPI jobs the disk is available across all nodes running the job. The default maximum runtime is 720 minutes.

### Input files
Files can be uploaded and made available to jobs using the `--input` option. For example:
```
prominence run --input /home/alahiff/README alahiff/testpi:latest "cat README"
```
The files will be written in the job's current directory, also referred to by the HOME, TMP or TEMP environment variables. Note that large data files should not be provided to jobs this way, and there is a total size limit of 1 MB per file.

### Fetching files and archives
Artifacts can either be obtained from remote URLs before jobs are executed. Using the `--artifact` option you can specify a URL. For example:
```
prominence run --artifact https://raw.githubusercontent.com/giovtorres/docker-centos7-slurm/master/Dockerfile busybox /bin/ls
```
Archives with filenames ending in the following are automatically unpacked:
* .tar
* .tar.gz
* .tgz
* .gz
* .tar.bz2
* .bz2
* .zip

### Output files
Output files generated by jobs can be automatically copied to cloud storage. Specify the output filenames when submitting the job, for example:
```
prominence run --output testfile1.out --output testfile2.out alahiff/tester
```
The output files are assumed to be in the job’s scratch directory (i.e. the directory specified by HOME, TMP, and TEMP). If the name of the output file cannot be immediately determined, Unix style path name pattern expansion can be used, for example `out/*/testfile.out`. A current limitation is that this must correspond to a single file only.

Once the job has finished running you can easily obtain the locations of the files as temporary URLs using the `describe` command, which returns the full JSON representation of the job:
```
prominence describe --completed 481
```

> output

```json
[
  {
    "id": 481,
    "status": "completed",
    "image": "alahiff/tester",
    "cpus": 1,
    "memory": 1,
    "nodes": 1,
    "disk": 10,
    "runtime": 720,
    "outputFiles": [
      {
        "name": "testfile1.out",
        "url": "https://s3.uk/swift/v1/prominence-jobs/366075e5-35aa-4d80-8d70-15560c4f5ec9/testfile1.out?temp_url_sig=b31edb25c14b43e6e8a54a506bdf06f84861838b&temp_url_expires=1535203453"
      },
      {
        "name": "testfile2.out",
        "url": "https://s3.uk/swift/v1/prominence-jobs/366075e5-35aa-4d80-8d70-15560c4f5ec9/testfile2.out?temp_url_sig=6e9c78982057476ea1320c14e941daedd10cd317&temp_url_expires=1535203453"
      }
    ],
    "events": {
      "creationTime": "2018-08-15T13:23:42Z",    
      "containerCreationStart": "2018-08-15T13:35:43Z",
      "executionStart": "2018-08-15T13:35:49Z",
      "completionTime": "2018-08-15T13:36:20Z"
    }
  }
]
```
Currently the temporary URLs expire after 10 days, but the data itself is retained.

The `download` command enables all output files from a specific completed job to be automatically downloaded to the current directory. For example:
```
prominence download 193
```

> output

```
Downloading file "frame_001.png"
[==================================================]
```
In order to download all output files associated with a group of jobs, a constraint can be specified as an alternative to a job id. For example:
```
prominence download --constraint name=run5
```
will download the output files from all completed jobs which have a label __name=run5__. Unless for `--force` option is specified, output files will not be downloaded if there is an existing file with the same name.

### Environment variables
It is common for environment variables to be used to pass information into containers. The option `--env` can be used to specify an environment variable in the form of a key-value pair separated by "=". This option can be specified multiple times to set multiple environment variables. For example:
```
prominence run --env LOWER=4.5 --env UPPER=6.7 test/container
```

### Labels
Arbitrary labels in the form of key-value pairs (separated by "=") can be set using the `--label` option. This option can be used multiple times to set multiple labels. For example:
```
prominence run --label experiment=MASTU --label env=dev test/container
```
Each key and value must be a string of less than 64 characters. Keys can only contain alphanumeric characters (`[a-z0-9A-Z]`) while values can also contain dashes (`-`), underscores (`_`), dots (`.`) and forward slashes (`/`).

### Mounting filesystems
A B2DROP folder or OneData space can be mounted as a POSIX-like filesystem accessible by jobs.

## Checking the status of jobs
The `list` command will by default list any active jobs (i.e. jobs which are idle or running):
```
prominence list
```

> output

```
ID     STATUS   IMAGE                       CMD     ARGS
3101   idle     alahiff/testpi
3103   idle     alahiff/cherab-jet:latest   python  batch_make_sensitivity_matrix.py 0 59
3104   idle     ikester/blender:latest      blender -b classroom/classroom.blend -o frame_### -f 1
```
It's also possible to request a list of jobs using a constraint on the labels associated with each job. For example, if you submitted a group of jobs with a label __name=run5__, the following would list all such jobs:
```
prominence list --all --constraint name=run5
```
Here the `--all` option means that both active (i.e. idle or running) and completed jobs will be listed.

To get more information about an individual job, use the `describe` command, for example:
```
prominence describe 139
```

> output

```json
[
  {
    "id": 139,
    "status": "idle",
    "image": "alahiff/testpi:latest",
    "cpus": 1,
    "memory": 1,
    "nodes": 1,
    "disk": 10,
    "runtime": 720,
    "events": {
      "creation": "2018-08-29T15:40:08Z"
    }
  }
]
```
To show information about completed jobs, both the `list` and `describe` commands accept a `--completed` option. For example, to list the last 2 completed jobs:
```
prominence list --completed --num 2
```

> output

```
ID     STATUS      IMAGE                       CMD          ARGS
2980   completed   alahiff/tensorflow:1.11.0   python       models-1.11/official/mnist/mnist.py --export_dir mnist_saved_model
2982   completed   alahiff/tensorflow:1.11.0   python       models-1.11/official/mnist/mnist.py --export_dir mnist_saved_model
```
Note that jobs which are completed or have been removed for some reason may be visible briefly without using the `--completed` option.

## Deleting a job
Jobs cannot be modified after they are created but they can be deleted. The `delete` command allows you to kill a single job:
```
prominence delete 164
```

> output

```
Success
```

## Viewing standard output and error

The standard output and error from a job can be seen using the `stdout` and `stderr` commands. For example, to get the standard output for the job with id 299:
```
prominence stdout 299
```

> output

```
 _______
< hello >
 -------
    \
     \
      \
                    ##        .
              ## ## ##       ==
           ## ## ## ##      ===
       /""""""""""""""""___/ ===
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
       \______ o          __/
        \    \        __/
          \____\______/

```
Note that the standard output and error can be seen while jobs are running as well as once they have completed.
