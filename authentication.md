---
layout: single
classes: wide
title: "Authentication"
permalink: /authentication
sidebar:
  nav: "docs"
---

Before you can run any commands you need to login. This retrieves a time-limited token which is used to authenticate against the PROMINENCE RESTful API.
```
prominence login
```

> output

```
To login, use a web browser to open the page https://<PROMINENCE_OIDC_URL>/device and enter the code ABCDEF when requested
```
The instructions here should be followed, i.e. open the specified page in a web browser, login with your username and password, then type in the 6 character code when requested. After you have given approval to PROMINENCE, the following should appear from the CLI:
```
Authentication successful
```
Currently the token is stored in a JSON file `$HOME/.prominence` readable by the current user only.

