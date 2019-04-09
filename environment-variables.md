---
layout: single
classes: wide
title: "Environment variables"
permalink: /environment-variables
sidebar:
  nav: "docs"
---

It is common for environment variables to be used to pass information into containers. The option `--env` can be used to specify an environment variable in the form of a key-value pair separated by "=". This option can be specified multiple times to set multiple environment variables. For example:
```
prominence create --env LOWER=4.5 --env UPPER=6.7 test/container
```
