---
title: Configuration
menu:
  enterprise_1_0:
    weight: 20
    parent: Administration
---

<table style="width:100%">
  <tr>
    <td><a href="#meta-node-configuration">Meta Node Configuration File</a></td>
    <td><a href="#data-node-configuration">Data Node Configuration File</a></td>
    <td><a href="#web-console-configuration">Web Console Configuration File</a></td>
  </tr>
</table>
<br>
<br>
# Meta Node Configuration

## Global options

### reporting-disabled = false

InfluxData, the company, relies on reported data from running nodes primarily to
track the adoption rates of different InfluxDB versions.
These data help InfluxData support the continuing development of InfluxDB.

The `reporting-disabled` option toggles the reporting of data every 24 hours to
`usage.influxdata.com`.
Each report includes a randomly-generated identifier, OS, architecture,
InfluxDB version, and the number of databases, measurements, and unique series.
Setting this option to `true` will disable reporting.

> **Note:** No data from user databases are ever transmitted.

### bind-address = ""

### hostname = ""

The hostname of the [meta node](/enterprise/v1.0/concepts/glossary/#meta-node).

## [enterprise]

Controls the parameters for the data node's registration with the web console
and [https://portal.influxdata.com](https://portal.influxdata.com).

###  registration-enabled = true

Set to `true` to enable registration with the InfluxEnterprise web console.
Setting `registration-enabled` to `true` in the meta node configuration file
is required if using the web console with an InfluxEnterprise cluster.

###  registration-server-url = "http://IP_or_hostname:3000"

The full URL of the server that runs the InfluxEnterprise web console.
`registration-server-url` requires the protocol, IP or hostname, and port.
This setting is required if using the web console with an InfluxEnterprise
cluster.

###  license-key = ""

The license key that you created on [InfluxPortal](https://portal.influxdata.com).
The license key registers with
[https://portal.influxdata.com](https://portal.influxdata.com) and allows the
meta process to run.
See the [`license-path` setting](#license-path) if your server cannot
communicate with [https://portal.influxdata.com](https://portal.influxdata.com).

Either the `license-key` setting or the `license-path` setting is required when
installing the cluster and web console.
The `license-key` and `license-path` settings are mutually exclusive; one
must remain set to the empty string.

###  license-path = ""

The local path to the JSON license file that you received from InfluxData.
Contact support to receive a license file.
The license file allows the meta process to run.

Either the `license-key` setting or the `license-path` setting is required when
installing the cluster and web console.
The `license-key` and `license-path` settings are mutually exclusive; one
must remain set to the empty string.

## [meta]

###  dir = "/var/lib/influxdb/meta"

The location of the meta directory which contains the [metastore](/influxdb/v1.0/concepts/glossary/#metastore).

###  bind-address = ":8089"

The bind address/port for cluster-wide communication.

###  http-bind-address = ":8091"

The bind address/port for meta node communication.

###  https-enabled = false

Set to `true` to if the Cluster API is using TLS.

###  https-certificate = ""

The path of the certificate file.

###  gossip-frequency = "5s"

###  announcement-expiration = "30s"

###  retention-autocreate = true

Set to `false` to disable the autocreation of the `autogen` [retention policy](/influxdb/v1.0/concepts/glossary/#retention-policy-rp).

If set to `true`, InfluxDB automatically creates a `DEFAULT` retention policy
when a database is created.
That retention policy is called `autogen`, has an infinite
[duration](/influxdb/v1.0/concepts/glossary/#duration), and a
[replication factor](/influxdb/v1.0/concepts/glossary/#replication-factor) set
to the number of nodes in the cluster.
InfluxDB uses `autogen` if a write or query does not specify a retention policy.

###  election-timeout = "1s"

The duration a Raft candidate spends in the candidate state without a leader
before it starts an election.
The election timeout is slightly randomized on each Raft node to a value between
one to two times the election timeout duration.
The default setting should work for most systems.

###  heartbeat-timeout = "1s"

The heartbeat timeout is the amount of time a Raft follower remains in the
follower state without a leader before it starts an election.
Clusters with high latency between nodes may want to increase this parameter.

###  leader-lease-timeout = "500ms"

The leader lease timeout is the amount of time a Raft leader will remain leader
if it does not hear from a majority of nodes.
After the timeout the leader steps down to the follower state.
The default setting should work for most systems.

###  commit-timeout = "50ms"

The commit timeout is the amount of time a Raft node will tolerate between
commands before issuing a heartbeat to tell the leader it is alive.
The default setting should work for most systems.

###  cluster-tracing = false

Cluster tracing toggles the logging of Raft logs on Raft nodes.
Enable this setting when debugging Raft consensus issues.

###  raft-promotion-enabled = true

Raft promotion automatically promotes a node to a Raft node when needed.
Disabling Raft promotion is desirable only when specific meta nodes should be
participating in Raft consensus.

###  logging-enabled = true

Meta logging toggles the logging of messages from the meta service.

###  pprof-enabled = false

###  lease-duration = "1m0s"

The default duration for leases.

<br>
<br>
# Data Node Configuration

> **Note:**
The system has internal defaults for every configuration file setting.
View the default settings with the `influxd config` command.
The local configuration file (`/etc/influxdb/influxdb.conf`) overrides any
internal defaults but the configuration file does not need to include
every configuration setting.
Starting with version 1.0.1, most of the settings in the local configuration
file are commented out.
All commented-out settings will be determined by the internal defaults.

## Global options

### reporting-disabled = false

See the [OSS documentation](/influxdb/v1.0/administration/config/#reporting-disabled-false).

### bind-address = ":8088"

See the [OSS documentation](/influxdb/v1.0/administration/config/#bind-address-8088).

### hostname = "localhost"

The hostname of the [data node](/enterprise/v1.0/concepts/glossary/#data-node).

## [enterprise]

Controls the parameters for the data node's registration with the web console
and [https://portal.influxdata.com](https://portal.influxdata.com).

### registration-enabled = false

Set to `true` to enable registration with the InfluxEnterprise web console.
Setting `registration-enabled` to `true` in the data node configuration file
is **not** required when installing the cluster and web console.

### registration-server-url = "http://IP_or_hostname:3000"

The full URL of the server that runs the InfluxEnterprise web console.
`registration-server-url` requires the protocol, IP or hostname, and port.
This setting is **not** required when installing the cluster and web console.

### license-key = ""

The license key that you created on [InfluxPortal](https://portal.influxdata.com).
The license key registers with
[https://portal.influxdata.com](https://portal.influxdata.com) and allows the
data process to run.
See the [`license-path` setting](#license-path) if your server cannot
communicate with [https://portal.influxdata.com](https://portal.influxdata.com).

Either the `license-key` setting or the `license-path` setting is required when
installing the cluster and web console.
The `license-key` and `license-path` settings are mutually exclusive; one
must remain set to the empty string.

### license-path = ""

The local path to the JSON license file that you received from InfluxData.
Contact support to receive a license file.
The license file allows the data process to run.

Either the `license-key` setting or the `license-path` setting is required when
installing the cluster and web console.
The `license-key` and `license-path` settings are mutually exclusive; one
must remain set to the empty string.

## [meta]

See the [OSS documentation](/influxdb/v1.0/administration/config/#meta).

###  dir = "/var/lib/influxdb/meta"

See the [OSS documentation](/influxdb/v1.0/administration/config/#dir-var-lib-influxdb-meta).
Note that data nodes do require a local meta directory.

###  meta-tls-enabled = false

Set to `true` if the Cluster API is using TLS.

###  meta-insecure-tls = false

Set to `true` if the Cluster API is using TLS and to allow the data node to
accept self-signed certificates.

###  retention-autocreate = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#retention-autocreate-true).

###  logging-enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#logging-enabled-true).

## [data]

See the [OSS documentation](/influxdb/v1.0/administration/config/#data).

###  dir = "/var/lib/influxdb/data"

See the [OSS documentation](/influxdb/v1.0/administration/config/#dir-var-lib-influxdb-data).

###  wal-dir = "/var/lib/influxdb/wal"

See the [OSS documentation](/influxdb/v1.0/administration/config/#wal-dir-var-lib-influxdb-wal).

###  wal-logging-enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#wal-logging-enabled-true).

###  query-log-enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#query-log-enabled-true).

###  cache-max-memory-size = 524288000

See the [OSS documentation](/influxdb/v1.0/administration/config/#cache-max-memory-size-524288000).

###  cache-snapshot-memory-size = 26214400

See the [OSS documentation](/influxdb/v1.0/administration/config/#cache-snapshot-memory-size-26214400).

###  cache-snapshot-write-cold-duration = "1h0m0s"

See the [OSS documentation](/influxdb/v1.0/administration/config/#cache-snapshot-write-cold-duration-1h0m0s).

###  compact-full-write-cold-duration = "24h0m0s"

See the [OSS documentation](/influxdb/v1.0/administration/config/#compact-full-write-cold-duration-24h0m0s).

###  max-series-per-database = 1000000

See the [OSS documentation](/influxdb/v1.0/administration/config/#max-series-per-database-1000000).

###  trace-logging-enabled = false

See the [OSS documentation](/influxdb/v1.0/administration/config/#trace-logging-enabled-false).

## [cluster]

Controls how data are shared across shards and the options for [query
management](/influxdb/v1.0/troubleshooting/query_management/).

###  dial-timeout = "1s"

###  shard-writer-timeout = "5s"

###  shard-reader-timeout = "0"

###  max-remote-write-connections = 50

###  cluster-tracing = false

###  write-timeout = "10s"

See the [OSS documentation](/influxdb/v1.0/administration/config/#write-timeout-10s).

###  max-concurrent-queries = 0

See the [OSS documentation](/influxdb/v1.0/administration/config/#max-concurrent-queries-0).

###  query-timeout = "0"

See the [OSS documentation](/influxdb/v1.0/administration/config/#query-timeout-0).

###  log-queries-after = "0"

See the [OSS documentation](/influxdb/v1.0/administration/config/#log-queries-after-0).

###  max-select-point = 0

See the [OSS documentation](/influxdb/v1.0/administration/config/#max-select-point-0).

###  max-select-series = 0

See the [OSS documentation](/influxdb/v1.0/administration/config/#max-select-series-0).

###  max-select-buckets = 0

See the [OSS documentation](/influxdb/v1.0/administration/config/#max-select-buckets-0).

## [retention]

See the [OSS documentation](/influxdb/v1.0/administration/config/#retention).

###  enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#enabled-true).

###  check-interval = "30m0s"

See the [OSS documentation](/influxdb/v1.0/administration/config/#check-interval-30m0s).

## [shard-precreation]

See the [OSS documentation](/influxdb/v1.0/administration/config/#shard-precreation).

###  enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#enabled-true-1).

###  check-interval = "10m0s"

See the [OSS documentation](/influxdb/v1.0/administration/config/#check-interval-10m0s).

###  advance-period = "30m0s"

See the [OSS documentation](/influxdb/v1.0/administration/config/#advance-period-30m0s).

## [admin]

See the [OSS documentation](/influxdb/v1.0/administration/config/#admin).

###  enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#enabled-true-2).

###  bind-address = ":8083"

See the [OSS documentation](/influxdb/v1.0/administration/config/#bind-address-8083).

###  https-enabled = false

See the [OSS documentation](/influxdb/v1.0/administration/config/#https-enabled-false).

###  https-certificate = "/etc/ssl/influxdb.pem"

See the [OSS documentation](/influxdb/v1.0/administration/config/#https-certificate-etc-ssl-influxdb-pem).

## [monitor]

See the [OSS documentation](/influxdb/v1.0/administration/config/#monitor).

###  store-enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#store-enabled-true).

###  store-database = "_internal"

See the [OSS documentation](/influxdb/v1.0/administration/config/#store-database-internal).

###  store-interval = "10s"

See the [OSS documentation](/influxdb/v1.0/administration/config/#store-interval-10s).

###  remote-collect-interval = "10s"

## [subscriber]

See the [OSS documentation](/influxdb/v1.0/administration/config/#subscriber).

###  enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#enabled-true-3).

###  http-timeout = "30s"

See the [OSS documentation](/influxdb/v1.0/administration/config/#http-timeout-30s).

## [http]

See the [OSS documentation](/influxdb/v1.0/administration/config/#http).

###  enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#enabled-true-4).

###  bind-address = ":8086"

See the [OSS documentation](/influxdb/v1.0/administration/config/#bind-address-8086).

###  auth-enabled = false

See the [OSS documentation](/influxdb/v1.0/administration/config/#auth-enabled-false).

###  log-enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#log-enabled-true).

###  write-tracing = false

See the [OSS documentation](/influxdb/v1.0/administration/config/#write-tracing-false).

###  https-enabled = false

See the [OSS documentation](/influxdb/v1.0/administration/config/#https-enabled-false-1).

###  https-certificate = "/etc/ssl/influxdb.pem"

See the [OSS documentation](/influxdb/v1.0/administration/config/#https-certificate-etc-ssl-influxdb-pem-1).

###  https-private-key = ""

See the [OSS documentation](/influxdb/v1.0/administration/config/#https-private-key).

###  max-row-limit = 10000

See the [OSS documentation](/influxdb/v1.0/administration/config/#max-row-limit-10000).

###  max-connection-limit = 0

See the [OSS documentation](/influxdb/v1.0/administration/config/#max-connection-limit-0).

###  shared-secret = ""

See the [OSS documentation](/influxdb/v1.0/administration/config/#shared-secret).

###  realm = "InfluxDB"

See the [OSS documentation](/influxdb/v1.0/administration/config/#realm-influxdb).

## [[graphite]]

See the [OSS documentation](/influxdb/v1.0/administration/config/#graphite).

## [[collectd]]

See the [OSS documentation](/influxdb/v1.0/administration/config/#collectd).

## [[opentsdb]]

See the [OSS documentation](/influxdb/v1.0/administration/config/#opentsdb).

## [[udp]]

See the [OSS documentation](/influxdb/v1.0/administration/config/#udp).

## [continuous_queries]

See the [OSS documentation](/influxdb/v1.0/administration/config/#continuous-queries).

###  log-enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#log-enabled-true-1).

###  enabled = true

See the [OSS documentation](/influxdb/v1.0/administration/config/#enabled-true-5).

###  run-interval = "1s"

See the [OSS documentation](/influxdb/v1.0/administration/config/#run-interval-1s).

## [hinted-handoff]

Controls the hinted handoff feature which allows nodes to temporarily store
queued data when one node of a cluster is down for a short period of time.

###  dir = "/var/lib/influxdb/hh"

The hinted handoff directory.

###  enabled = true

Set to `false` to disable hinted handoff.

###  max-size = 10737418240

The maximum size of the hinted handoff queue for a node.
If the queue is full, new writes are rejected and an error is returned to the
client.
The queue is drained when either the writes are retried successfully or the
writes expire.

###  max-age = "168h0m0s"

The time writes sit in the queue before they are purged. The time is determined by how long the batch has been in the queue, not by the timestamps in the data.

###  retry-concurrency = 20

###  retry-rate-limit = 0

The rate (in bytes per second) per node at which the hinted handoff retries
writes.
Set to `0` to disable the rate limit.

###  retry-interval = "1s"

The initial interval at which the hinted handoff retries a write after it fails.

> **Note:** Hinted handoff begins retrying writes to down nodes at the interval defined by the `retry-interval`. If any error occurs, it will backoff exponentially until it reaches the interval defined by the `retry-max-interval`. Hinted handoff then retries writes at that interval until it succeeds. The interval resets to the `retry-interval` once hinted handoff successfully completes writes to all nodes.

###  retry-max-interval = "10s"

The maximum interval at which the hinted handoff retries a write after it fails.
It retries at this interval until it succeeds.

###  purge-interval = "1m0s"

The interval at which InfluxDB checks to purge data that are above `max-age`.
<br>
<br>
# Web Console Configuration

### url = "http://IP_or_hostname:3000" # ENV: URL

The "pretty" URL that users will see for this app.

### hostname = "localhost" # ENV: HOSTNAME

The hostname that you want to bind the application to.

### port = "3000" # ENV: PORT

The port that you want to bind the application to.

### license-key = "" # ENV: LICENSE_KEY

The license key that you created on [InfluxPortal](https://portal.influxdata.com).
The license key registers with
[https://portal.influxdata.com](https://portal.influxdata.com) and allows the
web console process to run.
See the [`license-file` setting](#license-file-env-license-file) if your server cannot
communicate with [https://portal.influxdata.com](https://portal.influxdata.com).

Either the `license-key` setting or the `license-file` setting is required when
installing the cluster and web console.
The `license-key` and `license-file` settings are mutually exclusive; one
must remain set to the empty string.

### license-file = "" # ENV: LICENSE_FILE

The local path to the JSON license file that you received from InfluxData.
Contact support to receive a license file.
The license file allows the data process to run.

Either the `license-key` setting or the `license-file` setting is required when
installing the cluster and web console.
The `license-key` and `license-file` settings are mutually exclusive; one
must remain set to the empty string.

## [influxdb]
### shared-secret = "long pass phrase used for signing tokens" # ENV: SHARED_SECRET

Allows the web console to authenticate users with the cluster.
This setting must match the
[`shared-secret` setting](/enterprise/v1.0/administration/configuration/#shared-secret)
in the data node configuration files.

## [smtp]

Controls how the web console sends emails to invite users to the application.
Note that the web console requires a functioning SMTP server to email invites to
new web console users.
If you’re working on Ubuntu 14.04 and are looking for an SMTP server to use for
development purposes, see the
[SMTP Server Setup guide](/enterprise/v1.0/guides/smtp-server/) for how to get
up and running with [MailCatcher](https://mailcatcher.me/).

### host = "localhost" # ENV: SMTP_HOST

### port = "25" # ENV: SMTP_PORT

### username = "" # ENV: SMTP_USERNAME

### password = "" # ENV: SMTP_PASSWORD

### from_email = "donotreply@example.com" # ENV: SMTP_FROM_EMAIL

## [database]

Controls the location of the web console's database.
The web console currently only supports Postgres >= 9.3 or SQLite3.

### url = "postgres://postgres:password@localhost:5432/enterprise" # ENV: DATABASE_URL

The location of the PostgreSQL database for the web console.
By default, the web console uses SQLite for installations.
See
[Step 3 - Web Console Installation](/enterprise/v1.0/introduction/web_console_installation/#install-the-influxenterprise-web-console-with-postgresql) for instructions on using PostgreSQL.

### url = "sqlite3:///var/lib/influx-enterprise/enterprise.db"

The location of the SQLite database for the web console.
By default, the web console uses SQLite for installations.
