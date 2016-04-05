---
title: Relay

menu:
  influxdb_012:
    weight: 10
    parent: high_availability
---

InfluxDB supports open-source, high-availability configurations through mechanisms such as write duplication and sharding. To that end, we've created a project called [InfluxDB Relay](https://github.com/influxdata/influxdb-relay), which allows you to write to multiple InfluxDB servers. With the right architecture and disaster recovery processes, this achieves a highly available setup.

## Usage

Running InfluxDB Relay is simple, and requires a single binary and configuration file. We provide several sample configurations with the project. You can follow the instructions below on each host that you'll want to run it on.

```
# Install influxdb-relay to your $GOPATH/bin
$ go install github.com/influxdata/influxdb-relay

# Copy the sample config and modify to your liking
$ cp $GOPATH/github.com/influxdata/influxdb-relay/sample.toml ./relay.toml
$ vim relay.toml

# Start relay!
$ $GOPATH/influxdb-relay -config relay.toml
```

## Description

The architecture is fairly simple and consists of a load balancer, two or more InfluxDB Relay processes and two or more InfluxDB processes. The load balancer should point UDP traffic and HTTP POST requests with the path `/write` to the two relays while pointing GET requests with the path `/query` to the two InfluxDB servers.

The relay will listen for HTTP or UDP writes and write the data to both servers via their HTTP write endpoint. If the write is sent via HTTP, the relay will return a success response as soon as one of the two InfluxDB servers returns a success. If either InfluxDB server returns a 400 response, that will be returned to the client immediately. If both servers return a 500, a 500 will be returned to the client.

With this setup a failure of one Relay or one InfluxDB can be sustained while still taking writes and serving queries. However, the recovery process will require operator intervention.

## Recovery

InfluxDB organizes its data on disk into logical blocks of time called shards. We can use this to create a hot recovery process with zero downtime.

The length of time that shards represent in InfluxDB range from 1 hour to 7 days. For retention policies with an infinite duration (that is they keep data forever), their shard durations are 7 days. For the sake of our example, let's assume shard sizes of 1 day.

Let's say one of the InfluxDB servers goes down for an hour on 2016-03-10. Once the next day rolls over and we're now writing data to 2016-03-11, we can then restore things using these steps:

* Create backup of 2016-03-10 shard from server that was up the entire day
* Tell the load balancer to stop sending query traffic to the server that was down
* Restore the backup of the shard from the good server to the old server
* Tell the load balancer to resume sending queries to the previously downed server

During this entire process the Relays should be sending writes to both servers for the current shard (2016-03-11).

## Sharding

It's possible to add another layer on top of this kind of setup to shard data. Depending on your needs you could shard on the measurement name or a specific tag like `customer_id`. The sharding layer would have to service both queries and writes.
