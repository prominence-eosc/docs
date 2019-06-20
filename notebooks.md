---
layout: single
classes: wide
title: "Using PROMINENCE from notebooks"
permalink: /notebooks
sidebar:
  nav: "docs"
---

Since all interaction with PROMINENCE is via a REST API it is straightforward to use PROMINENCE from a Jupyter notebook.

## Simple example
Firstly install the PROMINENCE CLI:
```python
!pip install prominence
```
Set the two required environment variables:
```
%env PROMINENCE_URL=https://prominence.local/prominence/v1
%env PROMINENCE_OIDC_URL=https://prominence.local
```
Import the required modules:
```python
from prominence import auth
from prominence import ProminenceClient
```
Obtain a token:
```python
auth.authenticate_user()
```
As usual, you will be asked to visit a web page in your browser to authenticate.

Contruct the JSON job description. In this example we use OSPRay to render an image:
```python
# Required resources
resources = {'cpus':16,
             'memory':16,
             'disk':10,
             'nodes':1}

# Define a task
task = {'image':'alahiff/ospray',
        'runtime':'singularity',
        'cmd':'/opt/ospray-1.7.1.x86_64.linux/bin/ospBenchmark --file NASA-B-field-sun.osx --renderer scivis -hd --filmic -sg:spp=8 -i NASA'}

# Output files
output_files = ['NASA.ppm']

# Input files (artifacts)
artifact = {'url':'http://www.sdvis.org/ospray/download/demos/NASA-B-field-sun/NASA-B-field-sun.osx'}

# Create a job
job = {'name':'NASAstreamlines',
       'resources':resources,
       'outputFiles':output_files,
       'artifacts':[artifact],
       'tasks':[task]}
```
Now submit the job:
```python
client = ProminenceClient()
id = client.create_job(job)
print('Job submitted with id', id)
```
See https://github.com/prominence-eosc/examples/blob/master/ospray-streamlines-demo.ipynb for the complete notebook for this job, including displaying the output rendered image.
