---
layout: single
classes: wide
title: "Standard output and error"
permalink: /stdout-stderr
sidebar:
  nav: "docs"
---

The standard output and error from a job can be seen using the `stdout` and `stderr` commands. For example, to get the standard output for the job with id 299:
```
prominence stdout 299
```

> output

```
 _______
< hello >
 -------
    \
     \
      \
                    ##        .
              ## ## ##       ==
           ## ## ## ##      ===
       /""""""""""""""""___/ ===
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
       \______ o          __/
        \    \        __/
          \____\______/

```

**Note:** The standard output and error can be seen while jobs are running as well as once they have completed, allowing users to check the status of long-running jobs. This is not quite realtime, so there can be short delays in the standard output and error being updated. It is recommended not to pipe standard output into a file, as this will make it more difficult to understand what the job is doing while running. Use `tee` if necessary to
pipe standard output into a file.
{: .notice--warning}

