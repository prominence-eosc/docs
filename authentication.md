---
layout: default
title: "Authentication"
permalink: /authentication
parent: Command line interface
nav_order: 5
---

Before you can run any commands you need to login. This retrieves a time-limited token which is used to authenticate against the PROMINENCE RESTful API.
```
prominence login
```

> output

```
To login, use a web browser to open the page https://<prominence-oidc-server>/device and enter the code ABCDEF when requested
```
The instructions here should be followed, i.e. open the specified page in a web browser, login with your username and password, then type in the 6 character code when requested. After you have given approval to PROMINENCE, the following should appear from the CLI:
```
Authentication successful
```
The token is stored in a JSON file in the directory `$HOME/.prominence` readable by the current user only.

