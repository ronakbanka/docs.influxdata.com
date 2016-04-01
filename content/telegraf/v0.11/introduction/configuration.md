---
title: Configuring Telegraf

menu:
  telegraf_011:
    name: Configuring Telegraf
    weight: 10
    parent: introduction
---

## Configuring Telegraf

### Create a configuration file with a default set of plugins.

> Every plugin will be defined, but most will be commented out.
```
telegraf -sample-config > telegraf.conf
```

### Create a configuration file with specific inputs and outputs
```
telegraf -sample-config -input-filter <pluginname>[:<pluginname>] -output-filter <outputname>[:<outputname>] > telegraf.conf
```

For more advanced configuration options, see the [Telegraf config doc](https://github.com/influxdata/telegraf/blob/master/docs/CONFIGURATION.md)

> **Note:** In most cases, you will need to edit the configuration file to match your needs.
