# Submitting jobs using Python

## Prerequisites 

### Install the PROMINENCE CLI
Install the PROMINENCE CLI by running either:
```
sudo pip install prominence
```
or installing it in a new virtual environment, e.g.
```
virtualenv ~/.virtualenvs/prominence
source ~/.virtualenvs/prominence/bin/activate
pip install prominence
```
Both Python 2.x and 3.x should work.

### Setup the CLI
Before using the CLI the following 4 environment variables need to be defined:
```
PROMINENCE_URL
PROMINENCE_IAM_URL
PROMINENCE_IAM_CLIENT_ID
PROMINENCE_IAM_CLIENT_SECRET
```

### Obtain a token
The `login` command can be used to obtain a time-limited token:
```
prominence login
```

> output

```
To login, use a web browser to open the page https://<PROMINENCE_IAM_URL>/device and enter the code ABCDEF when requested
```
The instructions here should be followed, i.e. open the specified page in a web browser, login with your username and password, then type in the 6 character code when requested.

After you have given approval to PROMINENCE, the following should appear from the CLI:
```
Authentication successful
```

## Python example
Below is a simple example of submitting a job using Python. The token as obtained above is read and used for authenticating with the PROMINENCE REST API.
```
#!/usr/bin/env python
from __future__ import print_function
import json
import os
from prominence import ProminenceJob
from prominence import ProminenceTask
from prominence import ProminenceClient

# Set required resources & job name
job = ProminenceJob()
job.memory = 1
job.cpus = 1
job.nodes = 1
job.disk = 10
job.name = 'MyTestJob'

# Define task
task = ProminenceTask()
task.image = 'busybox'
task.cmd = 'echo Hello'

job.tasks = [task]

# Get token
with open(os.path.expanduser('~/.prominence')) as json_data:
    data = json.load(json_data)
if 'access_token' in data:
    token = data['access_token']
else:
    exit(1)

# Submit the job
client = ProminenceClient(url=os.environ['PROMINENCE_URL'], token=token)
response = client.create(job)

if response.return_code == 0:
    if 'id' in response.data:
        print('Job created with id %d' % response.data['id'])
else:
    if 'error' in response.data:
       print('Error: %s' % response.data['error'])
```
