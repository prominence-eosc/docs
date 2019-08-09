---
layout: single
classes: wide
title: "Labels"
permalink: /labels
sidebar:
  nav: "docs"
---

## Defining labels 
Arbitrary labels in the form of key-value pairs (separated by "=") can be set using the `--label` option. This option can be used multiple times to set multiple labels. For example:
```
prominence create --label experiment=MASTU --label env=dev test/container
```
Each key and value must be a string of less than 64 characters. Keys can only contain alphanumeric characters (`[a-z0-9A-Z]`) while values can also contain dashes (`-`), underscores (`_`), dots (`.`) and forward slashes (`/`).

## Using labels
It is possible to specify a constraint when using the `list` command. For example, to list all active jobs which have a label `experiment` set to `MASTU`:
```
prominence list --constraint experiment=MASTU
```
To list all jobs, i.e. both active and completed jobs, with a specific label:
```
prominence list --constraint experiment=MASTU --all
```

It is also possible to download the output files from all jobs which satisfy a particular constraint, for example:
```
prominence download --constraint experiment=MASTU
```

