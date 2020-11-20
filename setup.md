---
layout: single
classes: wide
title: "Setup"
permalink: /setup
sidebar:
  nav: "docs"
---

Two environment variables need to be set which define the URLs of the PROMINENCE and OpenID Connect (OIDC) servers:
```
$ export PROMINENCE_URL=https://<prominence hostname>
$ export PROMINENCE_OIDC_URL=https://<oidc hostname>
```
The URLs above will need to be replaced as appropriate for the PROMINENCE server you are using.

Register as an OIDC client:
```
$ prominence register
```
The text `Client registration successful` should appear. This step only needs to be done once per machine.

