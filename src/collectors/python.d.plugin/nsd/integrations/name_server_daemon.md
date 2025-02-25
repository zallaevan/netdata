<!--startmeta
custom_edit_url: "https://github.com/netdata/netdata/edit/master/src/collectors/python.d.plugin/nsd/README.md"
meta_yaml: "https://github.com/netdata/netdata/edit/master/src/collectors/python.d.plugin/nsd/metadata.yaml"
sidebar_label: "Name Server Daemon"
learn_status: "Published"
learn_rel_path: "Collecting Metrics/DNS and DHCP Servers"
most_popular: False
message: "DO NOT EDIT THIS FILE DIRECTLY, IT IS GENERATED BY THE COLLECTOR'S metadata.yaml FILE"
endmeta-->

# Name Server Daemon


<img src="https://netdata.cloud/img/nsd.svg" width="150"/>


Plugin: python.d.plugin
Module: nsd

<img src="https://img.shields.io/badge/maintained%20by-Netdata-%2300ab44" />

## Overview

This collector monitors NSD statistics like queries, zones, protocols, query types and more.


It uses the `nsd-control stats_noreset` command to gather metrics.


This collector is supported on all platforms.

This collector only supports collecting metrics from a single instance of this integration.


### Default Behavior

#### Auto-Detection

If permissions are satisfied, the collector will be able to run `nsd-control stats_noreset`, thus collecting metrics.

#### Limits

The default configuration for this integration does not impose any limits on data collection.

#### Performance Impact

The default configuration for this integration is not expected to impose a significant performance impact on the system.


## Metrics

Metrics grouped by *scope*.

The scope defines the instance that the metric belongs to. An instance is uniquely identified by a set of labels.



### Per Name Server Daemon instance

These metrics refer to the entire monitored application.

This scope has no labels.

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| nsd.queries | queries | queries/s |
| nsd.zones | master, slave | zones |
| nsd.protocols | udp, udp6, tcp, tcp6 | queries/s |
| nsd.type | A, NS, CNAME, SOA, PTR, HINFO, MX, NAPTR, TXT, AAAA, SRV, ANY | queries/s |
| nsd.transfer | NOTIFY, AXFR | queries/s |
| nsd.rcode | NOERROR, FORMERR, SERVFAIL, NXDOMAIN, NOTIMP, REFUSED, YXDOMAIN | queries/s |



## Alerts

There are no alerts configured by default for this integration.


## Setup

### Prerequisites

#### NSD version

The version of `nsd` must be 4.0+.


#### Provide Netdata the permissions to run the command

Netdata must have permissions to run the `nsd-control stats_noreset` command.

You can:

- Add "netdata" user to "nsd" group:
  ```
  usermod -aG nsd netdata
  ```
- Add Netdata to sudoers
  1. Edit the sudoers file:
    ```
    visudo -f /etc/sudoers.d/netdata
    ```
  2. Add the entry:
    ```
    Defaults:netdata   !requiretty
    netdata ALL=(ALL)  NOPASSWD: /usr/sbin/nsd-control stats_noreset
    ```

  > Note that you will need to set the `command` option to `sudo /usr/sbin/nsd-control stats_noreset` if you use this method.



### Configuration

#### File

The configuration file name for this integration is `python.d/nsd.conf`.


You can edit the configuration file using the `edit-config` script from the
Netdata [config directory](https://github.com/netdata/netdata/blob/master/docs/netdata-agent/configuration/README.md#the-netdata-config-directory).

```bash
cd /etc/netdata 2>/dev/null || cd /opt/netdata/etc/netdata
sudo ./edit-config python.d/nsd.conf
```
#### Options

This particular collector does not need further configuration to work if permissions are satisfied, but you can always customize it's data collection behavior.

There are 2 sections:

* Global variables
* One or more JOBS that can define multiple different instances to monitor.

The following options can be defined globally: priority, penalty, autodetection_retry, update_every, but can also be defined per JOB to override the global values.

Additionally, the following collapsed table contains all the options that can be configured inside a JOB definition.

Every configuration JOB starts with a `job_name` value which will appear in the dashboard, unless a `name` parameter is specified.


<details><summary>Config options</summary>

| Name | Description | Default | Required |
|:----|:-----------|:-------|:--------:|
| update_every | Sets the default data collection frequency. | 30 | no |
| priority | Controls the order of charts at the netdata dashboard. | 60000 | no |
| autodetection_retry | Sets the job re-check interval in seconds. | 0 | no |
| penalty | Indicates whether to apply penalty to update_every in case of failures. | yes | no |
| name | Job name. This value will overwrite the `job_name` value. JOBS with the same name are mutually exclusive. Only one of them will be allowed running at any time. This allows autodetection to try several alternatives and pick the one that works. |  | no |
| command | The command to run | nsd-control stats_noreset | no |

</details>

#### Examples

##### Basic

A basic configuration example.

```yaml
local:
  name: 'nsd_local'
  command: 'nsd-control stats_noreset'

```


## Troubleshooting

### Debug Mode

To troubleshoot issues with the `nsd` collector, run the `python.d.plugin` with the debug option enabled. The output
should give you clues as to why the collector isn't working.

- Navigate to the `plugins.d` directory, usually at `/usr/libexec/netdata/plugins.d/`. If that's not the case on
  your system, open `netdata.conf` and look for the `plugins` setting under `[directories]`.

  ```bash
  cd /usr/libexec/netdata/plugins.d/
  ```

- Switch to the `netdata` user.

  ```bash
  sudo -u netdata -s
  ```

- Run the `python.d.plugin` to debug the collector:

  ```bash
  ./python.d.plugin nsd debug trace
  ```


