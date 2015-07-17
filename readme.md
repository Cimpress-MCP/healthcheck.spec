# healthcheck.spec

A json specification for describing the health of your web service, including all it's required components. Inspired and heavily influenced by [Beamly's SE4](https://github.com/beamly/SE4/blob/master/SE4.md) spec.

## GET /healthcheck

The healthcheck resource provides information about internal health and its perceived health of downstream dependencies.

### Response Status Codes

Status                  | Meaning
------------------------|------------------------------------------
200 OK                  | Service and all dependencies are healthy
503 Service Unavailable | Service or any dependencies are unhealthy

### Response Media Type

application/json

### Elements

Element Path                      | Required? | Type          | Description                                                     | Example
----------------------------------|-----------|---------------|-----------------------------------------------------------------|---------------------------------
generated_at                      | Yes       | DateTime      | The time at which this report was generated                     | "2014-03-12T20:16:55.447Z"
duration_millis                   | Yes       | String        | The number of milliseconds it took to generate the report       | "15.8"
tests                             | Yes       | Object        | Object containing a set of test results keyed by component name |
tests.{component}.duration_millis | Yes       | Float         | Number of milliseconds taken to run the test                    | 1.0
tests.{component}.result          | Yes       | String (Enum) | The state of the test, must be "passed" or "failed"             | "passed"
tests.{component}.tested_at       | Yes       | DateTime      | The time at which this test was executed                        | "2014-03-12T20:16:45.013Z"
tests.{component}.error           | No        | String        | An optional description of any error conditions                 | "Request timed out after 2000ms"

### Example

```
< HTTP/1.1 503 Service Unavailable
< Server: cpt-server-i/0.0.1/1552 (ip-10-0-1-164 HttpServer2/1552)
< X-Request-Id: req941baf96-cc86-11e3-e4b9-add31c37a536
< X-Request-Time: 16ms
< Cache-Control: no-cache
< Content-Type: application/json
< Content-Length: 441
<
{
  "generated_at": "2015-06-25T14:33:33.383Z",
  "duration_millis": "15.8",
  "tests": {
    "cassandra": {
      "duration_millis": 5.6,
      "result": "passed",
      "tested_at": "2015-06-25T14:33:15.229Z"
    },
    "redis": {
      "duration_millis": 15.6,
      "result": "failed",
      "tested_at": "2015-06-25T14:33:15.286Z",
      "error": "Unable to connect to myredis.mydomain.com:6379"
    }
  }
}
```

## Swagger spec
See [swagger.yaml](swagger.yaml) for a full swagger implementation of the spec described above.
