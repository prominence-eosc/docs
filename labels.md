---
layout: single
classes: wide
title: "Labels"
permalink: /labels
sidebar:
  nav: "docs"
---

Arbitrary labels in the form of key-value pairs (separated by "=") can be set using the `--label` option. This option can be used multiple times to set multiple labels. For example:
```
prominence create --label experiment=MASTU --label env=dev test/container
```
Each key and value must be a string of less than 64 characters. Keys can only contain alphanumeric characters (`[a-z0-9A-Z]`) while values can also contain dashes (`-`), underscores (`_`), dots (`.`) and forward slashes (`/`).

