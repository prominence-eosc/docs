---
layout: default
title: "Key-value store"
permalink: /kv
parent: Data
nav_order: 2
---
# Key-value store

A key-value store is available through the REST API using the standard credential, i.e. access token.
This allows users to
store an arbitrary number of key-value pairs. Currently values are limited to 16 KB each. Jobs can access the key-value store (both read and write access)
and it can also be accessed externally. The following operations are available:
* Create a key
* Get the value of a key
* List keys
* Delete a key or group of keys

Data can be stored in hierarchically organized directories, for example:
* /input/param1
* /input/param2
* /input/param3
* /output/value1
* /output/value2

Example uses:
* Storing input data or input parameters for jobs
* Storing the output from a job if it's a single number, group of numbers or small file
* Enabling jobs to pass information to other jobs

Note that all keys are private to each user. Values can be in any ASCII or binary format (they are internally stored base64-encoded).

## Using the command line interface
`prominence kv` allows users to interact with the key-value store. To list all keys:
```
prominence kv list
```
or to list all keys with a specific prefix:
```
prominence kv list /inputs
```
Get the value of a key:
```
prominence kv get /mykey/path/example
```
To delete a key:
```
prominence kv delete /mykey/path/example
```
Using the `--prefix` option a range of keys can be deleted, for example:
```
prominence kv delete --prefix /mykey
```
will delete all keys beginning with `/mykey`.

A key can be set from a string or by specifying a filename. For example:
```
prominence kv set /kv/mykey/path/example "example string"
```
If a filename is specified the value will be set using the contents of the file, for example:
```
prominence kv set /kv/mykey/path/example test.txt
```

## Access from within jobs
To access the key-value store from within jobs use the provided access token, available from the `PROMINENCE_TOKEN` environment variable,
to authenticate against the REST API.

For example, here we use curl to store the string `example string` with key `/mykey/path/example`:
```
curl -s -H "Authorization: Bearer $PROMINENCE_TOKEN" -X POST -d "example string" $PROMINENCE_URL/kv/mykey/path/example
```
To retrieve the same key using curl:
```
curl -s -H "Authorization: Bearer $PROMINENCE_TOKEN" $PROMINENCE_URL/kv/mykey/path/example
```

