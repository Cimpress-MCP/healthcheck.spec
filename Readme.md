# healthcheck.spec

A specification for describing the health of your web service, including all of the required downstream components. Inspired and heavily influenced by [Beamly's SE4](https://github.com/beamly/SE4/blob/master/SE4.md) spec.

## /healthcheck

The healthcheck resource provides information about internal health and its perceived health of downstream dependencies.

Valid response code: 200 OK
Error response code: 503 Service Unavailable

Response Media Type: application/json

Element Path                      | Required? | Type          | Description                                                | Example
----------------------------------|-----------|---------------|------------------------------------------------------------|---------------------------
generated_at                      | Yes       | DateTime      | The time at which this report was generated                | "2014-03-12T20:16:55.447Z"
duration_millis                   | Yes       | String        | The number of milliseconds it took to generate the report  | "15.8"
tests                             | Yes       | Object        | Object containing a set of test results                    |
tests.{component}.duration_millis | Yes       | Float         | Number of milliseconds taken to run the test               | 1.0
tests.{component}.result          | Yes       | String (Enum) | The state of the test, may be "passed", "failed"           | "passed"
tests.{component}.tested_at       | Yes       | DateTime      | The time at which this test was executed                   | "2014-03-12T20:16:45.013Z"
tests.{component}.error           | No        | String        | An optional description of any error conditions            | "Request timed out after 2000ms"

Example:

```
< HTTP/1.1 200 OK
< Server: cpt-server-i/0.0.1/1552 (ip-10-0-1-164 HttpServer2/1552)
< X-Request-Id: req941baf96-cc86-11e3-e4b9-add31c37a536
< X-Request-Time: 11ms
< Cache-Control: no-cache
< Content-Type: application/json
< Content-Length: 418
<
{
  "generated_at": "2014-04-25T14:33:33.383Z",
  "duration_millis": "15.8",
  "tests":
    "cassandra": {
        "duration_millis": 5.6,
        "test_result": "passed",
        "tested_at": "2014-04-25T14:33:15.229Z"
    },
    "redis": {
      "duration_millis": 15.6,
      "test_result": "failed",
      "tested_at": "2014-04-25T14:33:15.286Z"
      "error": "Unable to connect to myredis.mydomain.com:6379"
    }
  }
}
```