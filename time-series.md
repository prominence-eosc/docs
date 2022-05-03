---
layout: single
classes: wide
title: "Time-series data"
permalink: /time-series
sidebar:
  nav: "time-series"
---

A time-series database is available through the REST API using the standard credential, i.e. access token. Jobs are able to write
time-series data which can be retrieved from the REST API. Metrics are added by writing JSON to the endpoint `$PROMINENCE_URL/ts`
consisting of:
* measurement name
* one or more fields (values are integers or floating point)
* optional tags (key-value pairs)
* optional unix epoch (if not specified the server will set the timestamp as the current time)

Example JSON for writing a measurement `status` with two fields (`value1`, `value2`), two tags (`tag1`, `tag2`) and the time specified:
```json
{
  "measurement": "status",
  "fields": {
    "value1": 1.23123,
    "value2": 20.819
  },
  "tags": {
    "tag1": "tagvalue1",
    "tag2": "tagvalue2"
  },
  "time": 1651564809
}
```

Time-series data can be retrieved from the `$PROMINENCE_URL/ts/<job id>`, for example for the data above:
```json
[
  {
    "field": "value1",
    "measurement": "status",
    "tags": {
      "tag1": "tagvalue1",
      "tag2": "tagvalue2"
    },
    "times": [
      "2022-05-03 07:44:20",
      "2022-05-03 07:44:30",
      "2022-05-03 07:44:40",
      "2022-05-03 07:44:50",
      "2022-05-03 07:45:00",
      "2022-05-03 07:45:10"
    ],
    "values": [
      0.10068017758796644,
      0.32005540226272633,
      0.12300812569512609,
      0.7421689063838589,
      0.666448125742937,
      0.9360817668784445
    ]
  },
  {
    "field": "value2",
    "measurement": "status",
    "tags": {
      "tag1": "tagvalue1",
      "tag2": "tagvalue2"
    },
    "times": [
      "2022-05-03 07:44:20",
      "2022-05-03 07:44:30",
      "2022-05-03 07:44:40",
      "2022-05-03 07:44:50",
      "2022-05-03 07:45:00",
      "2022-05-03 07:45:10"
    ],
    "values": [
      0.24235730048748294,
      0.059522328669096125,
      0.2754402984107196,
      0.5548901087803849,
      0.6836715778920943,
      0.1696360893098393
    ]
  }
]
```
Time-series data can easily be retrieved and plotted using Python, for example:
```json
import matplotlib.pyplot as plt
import datetime
import numpy as np
import pandas as pd
import requests
import os
import sys

token = os.environ['PROMINENCE_TOKEN']
url = os.environ['PROMINENCE_URL']
job_id = int(sys.argv[1])

response = requests.get('%s/ts/%d' % (url, job_id), headers={'Authorization': 'Bearer %s' % token})
data = response.json()

figure, axis = plt.subplots(len(data), 1)

count = 0
for table in data:
    df = pd.DataFrame({'date': [datetime.datetime.strptime(date, "%Y-%m-%d %H:%M:%S") for date in table['times']],
                       'data': table['values']})
    axis[count].plot(df.date, df.data, label='%s/%s' % (table['measurement'], table['field']), linewidth=3)
    axis[count].legend()
    count += 1

plt.suptitle('Job: %d' % job_id)
plt.show()
```
where we have assumed that an access token and PROMINENCE URL are defined in environment variables `PROMINENCE_TOKEN` and `PROMINENCE_URL`.
The above script requires the job id to be specified as an argument.