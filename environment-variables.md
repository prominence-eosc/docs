---
layout: single
classes: wide
title: "Environment variables"
permalink: /environment-variables
sidebar:
  nav: "docs"
---

It is a common technique to use environment variables to pass information, such as configuration options, into a container. The option `--env` can be used to specify an environment variable in the form of a key-value pair separated by "=". This option can be specified multiple times to set multiple environment variables. For example:
```
prominence create --env LOWER=4.5 --env UPPER=6.7 test/container
```
