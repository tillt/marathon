<!-- AUTO-GENERATED FILE; DO NOT MODIFY -->
# RESTful Marathon

## API Version 2

* [Apps](#apps)
  * [POST /v2/apps](#post-v2apps): Create and start a new app
  * [GET /v2/apps](#get-v2apps): List all running apps
  * [GET /v2/apps/{app_id}](#get-v2appsapp_id): List the app `app_id`
  * [GET /v2/apps?cmd={command}](#get-v2appscmdcommand): List all running
    apps, filtered by `command`
  * [PUT /v2/apps/{app_id}](#put-v2appsapp_id): Change config of the app
    `app_id`
  * [DELETE /v2/apps/{app_id}](#delete-v2appsapp_id): Destroy app `app_id`
  * [GET /v2/apps/{app_id}/tasks](#get-v2appsapp_idtasks): List running tasks
    for app `app_id`
  * [DELETE /v2/apps/{app_id}/tasks?host={host}&scale={true|false}](#delete-v2appsapp_idtaskshosthostscaletruefalse):
    kill tasks belonging to app `app_id`
  * [DELETE /v2/apps/{app_id}/tasks/{task_id}?scale={true|false}](#delete-v2appsapp_idtaskstask_idscaletruefalse):
    Kill the task `task_id` that belongs to the application `app_id`
* [Tasks](#tasks)
  * [GET /v2/tasks](#get-v2tasks): List all running tasks

### _Apps_

#### POST `/v2/apps`

Create and start a new application.

The full JSON format of an application resource is as follows:

```json
{
    "cmd": "(env && sleep 300)",
    "constraints": [
        ["attribute", "OPERATOR", "value"]
    ],
    "container": {
        "image": "docker:///zaiste/postgresql",
        "options": ["-e", "X=7"]
    },
    "cpus": 2,
    "env": {
        "LD_LIBRARY_PATH": "/usr/local/lib/myLib"
    },
    "executor": "",
    "id": "myApp",
    "instances": 3,
    "mem": 256.0,
    "ports": [
        8080,
        9000
    ],
    "uris": [
        "https://raw.github.com/mesosphere/marathon/master/README.md"
    ]
}
```

_Constraints:_ Valid constraint operators are one of ["UNIQUE", "CLUSTER",
"GROUP_BY"].  For additional information on using placement constraints see
[Marathon, a Mesos framework, adds Placement Constraints](http://mesosphere.io/2013/11/22/marathon-a-mesos-framework-adds-placement-constraints).

_Container:_ Additional data passed to the container on application launch.
These consist of an "image" and an array of string options.  The meaning of
this data is fully dependent upon the executor.  Furthermore, _it is invalid to
pass container options when using the default command executor_.

_Ports:_ An array of required port resources on the host.  To generate one or
more arbitrary free ports for each application instance, pass zeros as port
values.  Each port value is exposed to the instance via environment variables
`$PORT0`, `$PORT1`, etc.  Ports assigned to running instances are also
available via the task resource.

##### Example

**Request:**


```
POST /v2/apps HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Length: 273
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2

{
    "cmd": "env && sleep 60", 
    "constraints": [
        [
            "hostname", 
            "UNIQUE", 
            ""
        ]
    ], 
    "cpus": "0.1", 
    "env": {
        "LD_LIBRARY_PATH": "/usr/local/lib/myLib"
    }, 
    "id": "myApp", 
    "instances": "3", 
    "mem": "5", 
    "ports": [
        0, 
        0
    ], 
    "uris": [
        "https://raw.github.com/mesosphere/marathon/master/README.md"
    ]
}
```

**Response:**


```
HTTP/1.1 204 No Content
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)


```

#### GET `/v2/apps`

List all running applications.

##### Example

**Request:**

```
GET /v2/apps HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

{
    "apps": [
        {
            "cmd": "env && sleep 60", 
            "constraints": [
                [
                    "hostname", 
                    "UNIQUE", 
                    ""
                ]
            ], 
            "container": null, 
            "cpus": 0.1, 
            "env": {
                "LD_LIBRARY_PATH": "/usr/local/lib/myLib"
            }, 
            "executor": "", 
            "id": "myApp", 
            "instances": 3, 
            "mem": 5.0, 
            "ports": [
                13971, 
                12162
            ], 
            "taskRateLimit": 1.0, 
            "tasksRunning": 0, 
            "tasksStaged": 1, 
            "uris": [
                "https://raw.github.com/mesosphere/marathon/master/README.md"
            ]
        }
    ]
}
```

#### GET `/v2/apps/{app_id}`

List the application with id `app_id`.

##### Example

**Request:**

```
GET /v2/apps/myApp HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

{
    "app": {
        "cmd": "env && sleep 60", 
        "constraints": [
            [
                "hostname", 
                "UNIQUE", 
                ""
            ]
        ], 
        "container": null, 
        "cpus": 0.1, 
        "env": {
            "LD_LIBRARY_PATH": "/usr/local/lib/myLib"
        }, 
        "executor": "", 
        "id": "myApp", 
        "instances": 3, 
        "mem": 5.0, 
        "ports": [
            13971, 
            12162
        ], 
        "taskRateLimit": 1.0, 
        "tasks": [
            {
                "host": "mesos.vm", 
                "id": "myApp_0-1393024649759", 
                "ports": [
                    31877, 
                    31878
                ], 
                "stagedAt": "2014-02-21T23:17:29.762Z", 
                "startedAt": "2014-02-21T23:17:35.065Z"
            }
        ], 
        "tasksRunning": 1, 
        "tasksStaged": 0, 
        "uris": [
            "https://raw.github.com/mesosphere/marathon/master/README.md"
        ]
    }
}
```

#### GET `/v2/apps?cmd={command}`

List all running applications, filtered by `command`.

##### Example

**Request:**

```
GET /v2/apps?cmd=sleep%2060 HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

{
    "apps": [
        {
            "cmd": "env && sleep 60", 
            "constraints": [
                [
                    "hostname", 
                    "UNIQUE", 
                    ""
                ]
            ], 
            "container": null, 
            "cpus": 0.1, 
            "env": {
                "LD_LIBRARY_PATH": "/usr/local/lib/myLib"
            }, 
            "executor": "", 
            "id": "myApp", 
            "instances": 3, 
            "mem": 5.0, 
            "ports": [
                13971, 
                12162
            ], 
            "taskRateLimit": 1.0, 
            "tasksRunning": 1, 
            "tasksStaged": 0, 
            "uris": [
                "https://raw.github.com/mesosphere/marathon/master/README.md"
            ]
        }
    ]
}
```

#### PUT `/v2/apps/{app_id}`

Change parameters of a running application.  The new application parameters
apply only to subsequently created tasks, and currently running tasks are
__not__ pre-emptively restarted.

##### Example

**Request:**

```
PUT /v2/apps/myApp HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Length: 126
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2

{
    "cmd": "sleep 55", 
    "constraints": [
        [
            "hostname", 
            "UNIQUE", 
            ""
        ]
    ], 
    "cpus": "0.3", 
    "instances": "2", 
    "mem": "9", 
    "ports": [
        9000
    ]
}
```

**Response:**

```
HTTP/1.1 204 No Content
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)


```


#### DELETE `/v2/apps/{app_id}`

Destroy an application. All data about that application will be deleted.

##### Example

**Request:**

```
DELETE /v2/apps/myApp HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Length: 0
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```


**Response:**

```
HTTP/1.1 204 No Content
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)


```


#### GET `/v2/apps/{app_id}/tasks`

List all running tasks for application `app_id`.

##### Example (as JSON)

**Request:**

```
GET /v2/apps/myApp/tasks HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

{
    "tasks": [
        {
            "host": "mesos.vm", 
            "id": "myApp_1-1393024693819", 
            "ports": [
                31965, 
                31966
            ], 
            "stagedAt": "2014-02-21T23:18:13.821Z", 
            "startedAt": null
        }, 
        {
            "host": "mesos.vm", 
            "id": "myApp_0-1393024688811", 
            "ports": [
                31816, 
                31817
            ], 
            "stagedAt": "2014-02-21T23:18:08.813Z", 
            "startedAt": "2014-02-21T23:18:14.214Z"
        }
    ]
}
```

##### Example (as text)

**Request:**

```
GET /v2/apps/myApp/tasks HTTP/1.1
Accept: text/plain
Accept-Encoding: gzip, deflate, compress
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: text/plain
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

myApp	13631	mesos.vm:31965	mesos.vm:31463	mesos.vm:31816	
myApp	10150	mesos.vm:31966	mesos.vm:31464	mesos.vm:31817	

```

#### DELETE `/v2/apps/{app_id}/tasks?host={host}&scale={true|false}`

Kill tasks that belong to the application `app_id`, optionally filtered by the
task's `host`.

The query parameters `host` and `scale` are both optional.  If `host` is
specified, only tasks running on the supplied slave are killed.  If
`scale=true` is specified, then the application is scaled down by the number of
killed tasks.  The `scale` parameter defaults to `false`.

##### Example

**Request:**

```
DELETE /v2/apps/myApp/tasks?host=mesos.vm&scale=false HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Length: 0
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

{
    "tasks": [
        {
            "host": "mesos.vm", 
            "id": "myApp_0-1393024710845", 
            "ports": [
                31782, 
                31783
            ], 
            "stagedAt": "2014-02-21T23:18:30.846Z", 
            "startedAt": null
        }
    ]
}
```

#### DELETE `/v2/apps/{app_id}/tasks/{task_id}?scale={true|false}`

Kill the task with ID `task_id` that belongs to the application `app_id`.

The query parameter `scale` is optional.  If `scale=true` is specified, then
the application is scaled down one if the supplied `task_id` exists.  The
`scale` parameter defaults to `false`.

##### Example

**Request:**

```
DELETE /v2/apps/myApp/tasks/myApp_3-1389916890411 HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Length: 0
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2
```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

{
    "task": {
        "host": "mesos.vm",
        "id": "myApp_3-1389916890411",
        "ports": [
            31509,
            31510
        ],
        "stagedAt": "2014-01-17T00:01+0000",
        "startedAt": "2014-01-17T00:01+0000"
    }
}
```

### _Tasks_

#### GET `/v2/tasks`

List tasks of all running applications.

##### Example (as JSON)

**Request:**

```
GET /v2/tasks HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

{
    "tasks": [
        {
            "appId": "myApp", 
            "host": "mesos.vm", 
            "id": "myApp_1-1393024719875", 
            "ports": [
                31384, 
                31385
            ], 
            "stagedAt": "2014-02-21T23:18:39.877Z", 
            "startedAt": null
        }, 
        {
            "appId": "myApp", 
            "host": "mesos.vm", 
            "id": "myApp_0-1393024714858", 
            "ports": [
                31928, 
                31929
            ], 
            "stagedAt": "2014-02-21T23:18:34.859Z", 
            "startedAt": "2014-02-21T23:18:40.060Z"
        }
    ]
}
```

##### Example (as text)

**Request:**

```
GET /v2/tasks HTTP/1.1
Accept: text/plain
Accept-Encoding: gzip, deflate, compress
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: text/plain
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

myApp	13631	mesos.vm:31112	mesos.vm:31384	mesos.vm:31928	
myApp	10150	mesos.vm:31113	mesos.vm:31385	mesos.vm:31929	

```

## API Version 1 _(DEPRECATED)_

### _Apps_

#### POST `/v1/apps/start`

##### Example

**Request:**


```
POST /v1/apps/start HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Length: 169
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2

{
    "cmd": "sleep 60", 
    "cpus": "0.1", 
    "id": "myApp", 
    "instances": "3", 
    "mem": "5", 
    "ports": [
        0, 
        0
    ], 
    "uris": [
        "https://raw.github.com/mesosphere/marathon/master/README.md"
    ]
}
```

**Response:**


```
HTTP/1.1 204 No Content
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)


```

#### GET `/v1/apps`

##### Example

**Request:**

```
GET /v1/apps HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

[
    {
        "cmd": "sleep 60", 
        "constraints": [], 
        "container": null, 
        "cpus": 0.1, 
        "env": {}, 
        "executor": "", 
        "id": "myApp", 
        "instances": 3, 
        "mem": 5.0, 
        "ports": [
            12374, 
            10971
        ], 
        "taskRateLimit": 1.0, 
        "tasksRunning": 0, 
        "tasksStaged": 2, 
        "uris": [
            "https://raw.github.com/mesosphere/marathon/master/README.md"
        ]
    }
]
```

#### POST `/v1/apps/stop`

##### Example

**Request:**

```
POST /v1/apps/stop HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Length: 15
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2

{
    "id": "myApp"
}
```


**Request:**

```
HTTP/1.1 204 No Content
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)


```


#### POST `/v1/apps/scale`

##### Example

**Request:**

```
POST /v1/apps/scale HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Length: 33
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2

{
    "id": "myApp", 
    "instances": "4"
}
```

**Response:**

```
HTTP/1.1 204 No Content
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)


```


#### GET `/v1/apps/search?id={app_id}&cmd={command}`

##### Example

**Request:**

```
GET /v1/apps/search?id=myApp HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

[
    {
        "cmd": "sleep 60", 
        "constraints": [], 
        "container": null, 
        "cpus": 0.1, 
        "env": {}, 
        "executor": "", 
        "id": "myApp", 
        "instances": 3, 
        "mem": 5.0, 
        "ports": [
            11056, 
            12101
        ], 
        "taskRateLimit": 1.0, 
        "uris": [
            "https://raw.github.com/mesosphere/marathon/master/README.md"
        ]
    }
]
```

##### Example

**Request:**

```
GET /v1/apps/search?cmd=sleep%2060 HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

[
    {
        "cmd": "sleep 60", 
        "constraints": [], 
        "container": null, 
        "cpus": 0.1, 
        "env": {}, 
        "executor": "", 
        "id": "myApp", 
        "instances": 3, 
        "mem": 5.0, 
        "ports": [
            11056, 
            12101
        ], 
        "taskRateLimit": 1.0, 
        "uris": [
            "https://raw.github.com/mesosphere/marathon/master/README.md"
        ]
    }
]
```

#### GET `/v1/apps/{app_id}/tasks`

##### Example

**Request:**

```
GET /v1/apps/myApp/tasks HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

{
    "myApp": [
        {
            "host": "mesos.vm", 
            "id": "myApp_1-1393024768944", 
            "ports": [
                31266, 
                31267
            ]
        }, 
        {
            "host": "mesos.vm", 
            "id": "myApp_3-1393024779962", 
            "ports": [
                31037, 
                31038
            ]
        }, 
        {
            "host": "mesos.vm", 
            "id": "myApp_2-1393024774954", 
            "ports": [
                31413, 
                31414
            ]
        }, 
        {
            "host": "mesos.vm", 
            "id": "myApp_0-1393024762936", 
            "ports": [
                31289, 
                31290
            ]
        }
    ]
}
```

### _Endpoints_

#### GET `/v1/endpoints`

##### Example (as JSON)

**Request:**

```
GET /v1/endpoints HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

[
    {
        "id": "myApp", 
        "instances": [
            {
                "host": "mesos.vm", 
                "id": "myApp_1-1393024768944", 
                "ports": [
                    31266, 
                    31267
                ]
            }, 
            {
                "host": "mesos.vm", 
                "id": "myApp_3-1393024779962", 
                "ports": [
                    31037, 
                    31038
                ]
            }, 
            {
                "host": "mesos.vm", 
                "id": "myApp_2-1393024774954", 
                "ports": [
                    31413, 
                    31414
                ]
            }, 
            {
                "host": "mesos.vm", 
                "id": "myApp_0-1393024762936", 
                "ports": [
                    31289, 
                    31290
                ]
            }
        ], 
        "ports": [
            11056, 
            12101
        ]
    }
]
```

##### Example (as text)

**Request:**

```
GET /v1/endpoints HTTP/1.1
Accept: text/plain
Accept-Encoding: gzip, deflate, compress
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: text/plain
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

myApp_11056 11056 mesos.vm:31266 mesos.vm:31289 mesos.vm:31413 mesos.vm:31037 
myApp_12101 12101 mesos.vm:31267 mesos.vm:31290 mesos.vm:31414 mesos.vm:31038 

```

#### GET `/v1/endpoints/{app_id}`

##### Example (as JSON)

**Request:**

```
GET /v1/endpoints/myApp HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

{
    "id": "myApp", 
    "instances": [
        {
            "host": "mesos.vm", 
            "id": "myApp_3-1393024779962", 
            "ports": [
                31037, 
                31038
            ]
        }, 
        {
            "host": "mesos.vm", 
            "id": "myApp_2-1393024774954", 
            "ports": [
                31413, 
                31414
            ]
        }, 
        {
            "host": "mesos.vm", 
            "id": "myApp_0-1393024762936", 
            "ports": [
                31289, 
                31290
            ]
        }
    ], 
    "ports": [
        11056, 
        12101
    ]
}
```

##### Example (as text)

**Request:**

```
GET /v1/endpoints/myApp HTTP/1.1
Accept: text/plain
Accept-Encoding: gzip, deflate, compress
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: text/plain
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

myApp_11056 11056 mesos.vm:31289 mesos.vm:31413 mesos.vm:31037 
myApp_12101 12101 mesos.vm:31290 mesos.vm:31414 mesos.vm:31038 

```

### Tasks

#### GET `/v1/tasks`

##### Example

**Request:**

```
GET /v1/tasks HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

{
    "myApp": [
        {
            "host": "mesos.vm", 
            "id": "myApp_3-1393024779962", 
            "ports": [
                31037, 
                31038
            ]
        }, 
        {
            "host": "mesos.vm", 
            "id": "myApp_2-1393024774954", 
            "ports": [
                31413, 
                31414
            ]
        }, 
        {
            "host": "mesos.vm", 
            "id": "myApp_2-1393024829079", 
            "ports": [
                31050, 
                31051
            ]
        }
    ]
}
```


#### POST `/v1/tasks/kill?appId={app_id}&host={host}&id={task_id}&scale={true|false}`

##### Example

**Request:**

```
POST /v1/tasks/kill?appId=myApp HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Length: 0
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

[]
```

### _Debug_

#### GET `/v1/debug/isLeader`

##### Example

**Request:**

```
GET /v1/debug/isLeader HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

true
```

#### GET `/v1/debug/leaderUrl`

##### Example

**Request:**

```
GET /v1/debug/leaderUrl HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate, compress
Content-Type: application/json; charset=utf-8
Host: localhost:8080
User-Agent: HTTPie/0.7.2


```

**Response:**

```
HTTP/1.1 200 OK
Content-Type: application/json
Server: Jetty(8.y.z-SNAPSHOT)
Transfer-Encoding: chunked

mesos:8080
```
