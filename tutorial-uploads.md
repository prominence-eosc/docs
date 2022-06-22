---
layout: default
title: "Uploading images"
permalink: /tutorial-uploading-images
parent: Tutorials
nav_order: 3
---
# Uploading images
A SHA256 checksum should ideally be supplied when uploading container images to object storage as this enables
images to be cached on workers and re-used if necessary. For large numbers of short jobs this potentially give
performance improvements.

When using the CLI the checksum is automatically calculated and provided, e.g.
```
prominence upload --filename myimage.sif --name myimage.sif
```
Alternatively a user-provided checksum can be used, e.g.
```
prominence upload --filename myimage.sif --name myimage.sif --checksum 190a348fa3a5187d9a6edfb8142ad24baa96ec0b629368f8f17358b87ee662f6
```

## Using the REST API
A minimal example showing how to upload file, including the checksum, using Python is shown below. Here the SHA256 checksum is
calculate first, a presigned URL is requested and finally the file is uploaded to object storage using the presigned URL.
```
import hashlib
import os
import sys
import requests

# Name of object (arbitrary)
objectname = 'image.tar'

# Filename (name of file on the filesystem)
filename = 'image.tar'

# Calculate checksum in 4MB blocks
sha256_hash = hashlib.sha256()
with open(filename, 'rb') as fh:
    for byte_block in iter(lambda: fh.read(4096), b''):
        sha256_hash.update(byte_block)
        checksum = sha256_hash.hexdigest()

# Request presigned URL
data = {'filename': objectname,
        'checksum': checksum}

url = '%s/data/upload' % os.environ['PROMINENCE_URL']
headers = {'Authorization': 'Bearer %s' % os.environ['PROMINENCE_TOKEN']}
response = requests.post(url, json=data, headers=headers)

if response.status_code != 201:
    sys.exit(1)

url = response.json()['url']
fields = response.json()['fields']

# Upload file
with open(filename, 'rb') as fh:
    files = {'file': (objectname, fh)}
    response = requests.post(url, data=fields, files=files)

if response.status_code == 204:
    print('Succcess')
```
This is slightly more complex than when a checksum is not specified. In that case `fields` is not returned when a presigned URL is
requested and a simple POST can
be used to upload the file using the provided `url`.
