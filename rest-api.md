---
layout: single
classes: wide
title: "REST API"
permalink: /rest-api
sidebar:
  nav: "docs"
---

This is the specification of the PROMINENCE REST API.

## Jobs API

This API exposes endpoints for managing jobs.

### List Jobs

```
GET /v1/jobs
```

List jobs.

#### Example Request

```http
GET /v1/jobs HTTP/1.1
```

#### Example Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

### Query Parameters

- **completed** - If parameter is `true`, completed jobs will be returned. The default is for only idle and running jobs to be returned.
- **num** - Number of jobs to return.
- **constraint** - Constraint in the form of key-value pairs.

#### Status Codes

- **200** - no error
- **401** - unauthorized

### Get a Job

```
GET /v1/jobs/<id>
```

Get detailed information about a job.

#### Example Request

```http
GET /v1/jobs/1 HTTP/1.1
```

#### Example Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

### Query Parameters

- **completed** - If parameter is `true`, completed jobs will be returned. The default is for only idle and running jobs to be returned.

#### Status Codes

- **200** - no error
- **401** - unauthorized
- **404** - not found

### Create a Job

```
POST /v1/jobs
Content-Type: application/json
```
Create a job.

#### Example Request

#### Example Response

```http
HTTP/1.1 201 Created
Content-Type: application/json
```

```json
{
  "id": 1
}
```
#### Request Body

The following fields are used in the request body for creating a job:

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `resources` | `resources` | Yes | CPU, memory and disk resource requirements. |
| `inputs` | | No | List of filenames and their base64-encoded content to be made available to jobs. |
| `outputFiles` | `array[string]` | No | List of output filenames to be uploaded to storage. |
| `outputDirs` | `array[string]` | No | List of output directories to be uploaded to storage. |
| `artifacts` | `array[string]` | No | List of URLs to fetch before the job starts. |
| `labels` | `array[string]` | No | List of arbitrary labels in the form of name-value pairs, e.g. `name=value`. |
| `tasks` | `array[task]` | Yes | List of tasks |
| `policies` |  | No | Job policies. |
| `notifications` |  | No | Job notifications. |

`resources`:
| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `nodes` | `integer` | No | Number of nodes required. |
| `cpus` | `integer` | No | Number of CPU cores per node required. |
| `cpusRange` | `integer`,`integer` | No | Minimum and maximum number of CPU cores per node required. |
| `cpusOptions` | `integer`,`integer` | No | Minimum and maximum number of CPU cores per node required. |
| `cpusTotalRange` | `integer`,`integer` | No | Minimum and maximum number of total CPU cores across all nodes required. |
| `memory` | `integer` | No | Memory per node required in GB. |
| `memoryPerCpu` | `integer` | No | Memory per CPU core required in GB. |
| `disk` | `integer` | No | Disk space required in GB. |§

`task`:
| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `image` | `string` | Yes | Container image name. |
| `runtime` | `string` | No | Container runtime, either `singularity` (default) or `udocker`. |
| `cmd` | `string` | No | Command to run inside the container. |
| `args` | `string` | No | Arguments of command run inside the container. |
| `env` | `dict` | No | Dictionary of key-value pairs. |
| `workDir` | `string` | No | Working directory. |
| `type` | `string` | No | Type of task, one of `basic`, `sidecar`, `openmpi`, `mpich`, or `intelmpi`. |

`policies`:
| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `maximumRetries` | `integer` | No | Maximum number of times a task will be retried in the event of failures. By default there will be no retries. |
| `maximumTimeInQueue` | `integer` | No | Maximum time in minutes the job will remain in the queue. If a job cannot be run immediately it will wait in the queue (up to the specified time limit) until resources become available. The value -1 means that the job will remain in the queue until it starts running. The default value 0 means that the job will remain in the queue until it starts running or there is a failure. |
| `leaveInQueue` | `bool` | No | 
| `ignoreTaskTailures` | `bool` | No | normally if a task fails (i.e. exit code non-zero) no further tasks will be executed in a job. If set to True, all tasks will be executed. |
| `reportJobSuccessOnTaskFailure` | `bool` | No | When the job is run as part of a workflow, if the exit code of any tasks are non-zero the job will be reported as running successfully. This means that if retries are enabled within a workflow or a workflow is re-run, only jobs which failed because of infrastructure problems will be retried (e.g. problems pulling the container image or staging files in or out). The default value is `False`. tasks in a job will be run irrespective of any failures. The default value is False. |
| `autoScalingType` | `string` | No | If set to `none` only existing resources will be considered to run the job and no additional resources will be provisioned. |
| `runSerialTasksOnAllNodes` | `bool` | No | By default serial tasks are only run on one node for the case of multi-node jobs. Setting this to `True` results in serial tasks being run on all nodes. |
| `priority` | `integer` | No | Job priority. |
| `placement` | `placement` | No |

`notifications`:
| Field | Type | Required | Description |
| --- | --- | --- | --- |

#### Status Codes

- **201** - no error
- **400** - bad request
- **401** - unauthorized

### Clone a Job

```
PUT /v1/jobs/<id>/clone
```

Create a copy of an existing job.

#### Example Request

```http
PUT /v1/jobs/2/clone HTTP/1.1
```

#### Example Response

```http
HTTP/1.1 201 Created
Content-Type: application/json
```

```json
{
  "id": 1
}
```

#### Status Codes

- **201** - no error
- **400** - bad request
- **401** - unauthorized
- **404** - not found

### Delete a Job

```
DELETE /v1/jobs/<id>
```

Delete a job.

#### Example Request

```http
DELETE /v1/jobs/2 HTTP/1.1
```

#### Example Response

#### Status Codes

- **200** - no error
- **400** - bad request
- **401** - unauthorized
- **404** - not found

### Remove a Job

```
PUT /v1/jobs/<id>/remove
```

Remove a job from the queue. This is only applicable to jobs which were created with `leaveInQueue` in `policies`
set to `True`.

#### Example Request

```http
PUT /v1/jobs/2/remove HTTP/1.1
```

#### Example Response

#### Status Codes

- **200** - no error
- **400** - bad request
- **401** - unauthorized
- **404** - not found

### Get standard output

```
GET /v1/jobs/<id>/stdout
```

Get the stdout from a completed job.

#### Example Request

```http
GET /v1/jobs/3/stdout HTTP/1.1
```

#### Example Response

```http
HTTP/1.1 200 OK
Content-Type: text/plain
```

#### Status Codes

- **200** - no error - stdout, if any, wil be returned
- **400** - bad request - job does not exist
- **401** - unauthorized
- **403** - forbidden - user does not have permission to access the job
- **404** - not found - stdout does not exist

### Get standard error

```
GET /v1/jobs/<id>/stderr
```

Get the stderr from a completed job.

#### Example Request

```http
GET /v1/jobs/4/stderr HTTP/1.1
```

#### Example Response

```http
HTTP/1.1 200 OK
Content-Type: text/plain
```

#### Status Codes

- **200** - no error - stderr, if any, will be returned
- **400** - bad request - job does not exist
- **401** - unauthorized
- **403** - forbidden - user does not have permission to access the job
- **404** - not found - stderr does not exist


## Workflows API

This API exposes endpoints for managing workflows.

## Data API

This API exposes endpoints for managing data.

## Key-value store API

This API exposes endpoints for accessing a key-value store.
