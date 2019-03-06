# Tutorials

Here we present some example use cases using the Command Line Interface (CLI). It is assumed that you have access to an existing PROMINENCE instance or you have deployed your own.

***this page is under development***

## Prerequisites
Before using the CLI the following 4 environment variables need to be defined:
```
PROMINENCE_URL
PROMINENCE_IAM_URL
PROMINENCE_IAM_CLIENT_ID
PROMINENCE_IAM_CLIENT_SECRET
```

Before you can run any commands you need to login. This retrieves a time-limited token which is used to authenticate against the PROMINENCE RESTful API.
```
prominence login
```

> output

```
To login, use a web browser to open the page https://<PROMINENCE_IAM_URL>/device and enter the code ABCDEF when requested
```
The instructions here should be followed, i.e. open the specified page in a web browser, login with your username and password, then type in the 6 character code when requested. After you have given approval to PROMINENCE, the following should appear from the CLI:
```
Authentication successful
```

