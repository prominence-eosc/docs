---
layout: single
classes: wide
title: "Key-value store"
permalink: /kv
sidebar:
  nav: "kv"
---

A key-value store is available through the REST API using the same credentials (i.e. token) used for managing jobs and workflows.
This allows users to
store an arbitrary number of key-value pairs. Currently values are limited to 16 KB each. Jobs can access the key-value store (both read and write access)
and it can also be accessed externally. The following operations are available:
* Creating a key
* Getting the value of a key
* Deleting a key or group of keys

Example uses:
* Storing input data or input parameters for jobs
* Storing the output from a job if it's a single number or small file
* Enabling jobs to pass information to other jobs

Note that all keys are private to each user. Values can be in any ASCII or binary format (internally stored base64-encoded).

## Access from within jobs
Use the provided access token (available from the `PROMINENCE_TOKEN` environment variable to authenticate against the key-value
store rest API.

For example, here we use curl to store the string `example string` with key `/mykey/path/example`:
```
curl -s -H "Authorization: Bearer $PROMINENCE_TOKEN" -X POST -d "example string" $PROMINENCE_URL/kv/mykey/path/example
```
To retrieve the same key using curl:
```
curl -s -H "Authorization: Bearer $PROMINENCE_TOKEN" $PROMINENCE_URL/kv/mykey/path/example
```
