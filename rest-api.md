# REST API

This is the specification of the PROMINENCE REST API. This is still considered a work in progress, so things could change or break with every update.

## Jobs API

The Jobs API exposes endpoints for managing jobs.

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
| `image` | `string` | Yes | Container image name. |
| `cmd` | `string` | No | Command to run inside the container. |
| `args` | `string` | No | Arguments of command run inside the container. |
| `nodes` | `integer` | No | Number of nodes required. |
| `cpus` | `integer` | No | Number of CPU cores per node required. |
| `memory` | `integer` | No | Memory per node required in GB. |
| `disk` | `integer` | No | Disk space required in GB. |
| `runtime` | `integer` | No | Maximum runtime of the job in minutes. |
| `inputs` | | No | List of filenames and their base64-encoded content to be made available to jobs. |
| `outputFiles` | `array[string]` | No | List of output filenames to be uploaded to storage. |
| `outputDirs` | `array[string]` | No | List of output directories to be uploaded to storage. |
| `artifacts` | `array[string]` | No | List of URLs to fetch before the job starts. |
| `env` | `array[string]` | No | List of environment variables in the form of name-value pairs, e.g. `name=value`. |
| `labels` | `array[string]` | No | List of arbitrary labels in the form of name-value pairs, e.g. `name=value`. |
| `type` | `string` | No | Type of job. By default `basic` is used. For an MPI job use `mpi`. |
| `instances` | `integer` | No | Number of instances of this job. |
| `parallelism` | `integer` | No | Maximum number of idle and running instances of this job. |

#### Status Codes

- **201** - no error
- **400** - bad request
- **401** - unauthorized

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

- **200** - no error - stderr, if any, wil be returned
- **400** - bad request - job does not exist
- **401** - unauthorized
- **403** - forbidden - user does not have permission to access the job
- **404** - not found - stderr does not exist


## Workflows API

The Workflows API exposes endpoints for managing workflows.

