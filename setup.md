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
$ export PROMINENCE_URL=https://host-130-246-215-158.nubes.stfc.ac.uk/prominence/v1
$ export PROMINENCE_OIDC_URL=https://host-130-246-215-158.nubes.stfc.ac.uk
```

Register as an OIDC client:
```
$ prominence register
```
The text `Client registration successful` should appear. This step only needs to be done once per machine. For example, if you want to run the CLI from both your desktop and a laptop this will need to be run once on each.

