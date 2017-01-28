---
title: Release Notes/Changelog
menu:
  enterprise_1_2:
    weight: 0
    parent: About the Project
---

<table style="width:100%">
  <tr>
    <td><a href="#clustering">Clustering Release Notes/Changelog</a></td>
    <td><a href="#web-console">Web Console Release Notes/Changelog</a></td>
  </tr>
</table>

<br>
<br>
# Clustering

## v1.2.1 [2017-01-25]

#### Bugfixes

- Fix panic: slice bounds out of range

## v1.2.0 [2017-01-24]

### Release Notes

This release builds off of the 1.2.0 release of OSS InfluxDB.
Please see the OSS [release notes](https://github.com/influxdata/influxdb/blob/master/CHANGELOG.md) for more information about the OSS release.

### Upgrading

* If you previously had [`retention-autocreate`](/enterprise/v1.2/administration/configuration/#retention-autocreate-true) set to `false` on your meta nodes, you will now also have to disable it [on the data nodes](/enterprise/v1.2/administration/configuration/#retention-autocreate-true-1) as well, since these operations are now done on the data nodes.
* The previously deprecated `influxd-ctl force-leave` command has been removed. The replacement command to remove a meta node which is never coming back online is [`influxd-ctl remove-meta -force`](/enterprise/v1.2/features/cluster-commands/).

#### Features

- Improve the meta store: any meta store changes are done via a compare and swap
- Add support for [incremental backups](/enterprise/v1.2/guides/backup-and-restore/)
- Automatically remove any deleted shard groups from the data store
- Uncomment the section headers in the default [configuration file](/enterprise/v1.2/administration/configuration/)
- Add InfluxQL support for [subqueries](/influxdb/v1.2/query_language/data_exploration/#subqueries)

#### Bugfixes

- Update dependencies with Godeps
- Fix a data race in meta client
- Ensure that the system removes the relevant [user permissions and roles](/enterprise/v1.2/features/users/) when a database is dropped
- Fix a couple typos in demo [configuration file](/enterprise/v1.2/administration/configuration/)
- Make optional the version protobuf field for the meta store
- Remove the override of GOMAXPROCS
- Remove an unused configuration option (`dir`) from the backend
- Fix a panic around processing remote writes
- Return an error if a remote write has a field conflict
- Drop points in the hinted handoff that (1) have field conflict errors (2) have [`max-values-per-tag`](/influxdb/v1.2/administration/config/#max-values-per-tag-100000) errors
- Remove the deprecated `influxd-ctl force-leave` command
- Fix issue where CQs would stop running if the first meta node in the cluster stops
- Fix logging in the meta httpd handler service
- Fix issue where subscriptions send duplicate data for [Continuous Query](/influxdb/v1.2/query_language/continuous_queries/) results
- Fix the output for `influxd-ctl show-shards`
- Send the correct RPC response for `ExecuteStatementRequestMessage`

## v1.1.1 [2016-12-06]

This release builds off of the 1.1.1 release of InfluxDB.
Please see the [release notes](https://github.com/influxdata/influxdb/blob/1.1/CHANGELOG.md#v111-unreleased) for specific changes.

This release is built with Go 1.7.4.
It resolves a security vulnerability reported in Go version 1.7.3 which impacts all
users currently running on the Mac OS X platform, powered by the Darwin operating system.

## v1.1.0 [2016-11-14]

### Release Notes

### Upgrading

* The `influxd-ctl join` command has been renamed to `influxd-ctl add-meta`.  If you have existing scripts that use `influxd-ctl join`, they will need to use `influxd-ctl add-meta` or be updated to use the new cluster setup command.

#### Cluster Setup

The `influxd-ctl join` command has been changed to simplify cluster setups.  To join a node to a cluster, you can run `influxd-ctl join <meta:8091>`, and we will attempt to detect and add any meta or data node process running on the hosts automatically.  The previous `join` command exists as `add-meta` now.  If it's the first node of a cluster, the meta address argument is optional.

#### Logging

Switches to journald logging for on systemd systems. Logs will no longer be sent to `/var/log/influxdb` on systemd systems.

#### Features

- Adds configuration option for setting gossiping frequency on data nodes.
- Add queueBytes stat to hh\_processor, to indicate bytes in hh queue.
- Adds auth to meta service API.
- Update go dependencies, fix go vet, update circle go vet command.
- Simplified join processes
- query for versions running on all nodes
- "influxd-ctl show shards" should trigger an error, if possible

#### Bugfixes

- Getting latest snapshot will now timeout with an error after 30 seconds.
- Don't include expired shards in the /show-shards output.
- Ensure pprof-enabled config option is respected and profiling on meta nodes is enabled by default.
- Respect pprof-enabled flag on data node.
- For performance, use data reference instead of clone during read-only operations.
- show stats: cluster reported twice
- Ensure meta API redirects on all ErrNotLeader errors.
- Don't overwrite Plutonium users when importing OSS users.
- Fix data race in raft store.
- Fix HH issue with large segments blocking HH progress.
- kill-copy-shard reports success but does not interrupt the copy operation.
- Adding data node with meta traffic blocked hangs forever

## v1.0.4 [2016-10-19]

#### OSS

This release builds off of the 1.0.2 release of InfluxDB.  Please see the [release notes](https://github.com/influxdata/influxdb/blob/1.0/CHANGELOG.md#v102-2016-10-05) for specific changes.

#### Bugfixes

- Respect hinted handoff configuration settings.
- Fix expanding regular expressions when not all shards exist on node handling request.

## v1.0.3 [2016-10-07]

This release builds off of the 1.0.2 release of InfluxDB.  Please see the [release notes](https://github.com/influxdata/influxdb/blob/1.0/CHANGELOG.md#v102-2016-10-05) for specific changes.

### Cluster-specific Bugfixes

- Fix panic in hinted handoff: lastModified

## v1.0.2 [2016-10-06]
This release builds off of the 1.0.2 release of InfluxDB.  Please see the [release notes](https://github.com/influxdata/influxdb/blob/1.0/CHANGELOG.md#v102-2016-10-05) for specific changes.

### Cluster-specific Features

### Cluster-specific Bugfixes

- Prevent double read-lock in Meta Client.
- Corrupted hinted handoff file prevents `influxd` from starting
- Fix systemd package upgrade symlink failure

## v1.0.1 [2016-09-28]
This release builds off of the 1.0.1 OSS release of InfluxDB.
Please see the [OSS release notes](https://github.com/influxdata/influxdb/blob/1.0/CHANGELOG.md#v101-2016-09-26)
for specific changes.

### Cluster-specific Features

### Cluster-specific Bugfixes

* Balance shards correctly with a restore
* Fix panic in hinted handoff: `runtime error: invalid memory address or nil pointer dereference`
* Ensure meta node redirects to leader when removing data node
* Fix panic in hinted handoff: `runtime error: makeslice: len out of range`
* Update sample config for data nodes

## v1.0.0 [2016-09-07]

This release builds on the latest 1.0 master branch based off of 1.0beta3 + latest.  Please see the [release notes](https://github.com/influxdata/influxdb/blob/master/CHANGELOG.md#v100-unreleased) for specific changes.

Breaking Changes:

* The keywords `IF`, `EXISTS`, and `NOT` were removed for this release. This means you no longer need to specify `IF NOT EXISTS` for `DROP DATABASE` or `IF EXISTS` for `CREATE DATABASE`.  Using these keywords will return a query error.
* `max-series-per-database` was added with a default of 1M but can be disabled by setting it to `0`. Existing databases with series that exceed this limit will continue to load, but writes that would create new series will fail.

### Hinted Handoff

A number of changes to hinted handoff are included in this release:

* Truncating only the corrupt block in a corrupted segment to minimize data loss.
* Immediately queue writes in hinted handoff if there are still writes pending to prevent inconsistencies in shards.
* Remove hinted handoff queues when data nodes are removed to eliminate manual cleanup tasks.

### Performance

* `SHOW MEASUREMENT` and `SHOW TAG VALUES` have been optimized to work better for multiple nodes and shards.
* `DROP` and `DELETE` statements run in parallel and more efficiently and should not leave the system in an inconsistent state.

### Security

The Cluster API used by `influxd-ctl` can not be protected with SSL certs.

### Cluster Management

Data nodes that can no longer be restarted can now be forcefully removed from the cluster using `influxd-ctl remove-data -force <addr>`.  This should only be run if a grace removal is not possible.

Backup and restore has been updated to fix issues and refine existing capabilities.  

### Features

- Add Users method to control client
- Force Removal Of Data Node
- Disable logging stats queries
- Optimize `SHOW MEASUREMENTS` and `SHOW TAG VALUES`
- Update go package library dependencies
- Minimize amount of data-loss in corrupted hinted handoff file by repairing it
- Log a write error when hinted handoff queue is full for a node
- Remove hinted handoff queues when data nodes are removed from the cluster
- Add tests around import for meta store
- Adds full TLS support to Plutonium's Cluster API, including the use of self-signed certificates
- Improved backup/restore
- Update shard group creation logic to be balanced
- Keep raft log to a minimum to prevent replaying large raft logs on startup

### Bugfixes

- Remove bad connection from meta executor connection pool
- Fix panic in meta store
- Fix panic caused when shard group is not found
- Corrupted Hinted Handoff
- Ensure imported OSS admin users have all privileges
- Ensure max-select-series is respected
- Handle peer already known error
- Hinted handoff panic
- Drop writes if they contain field type inconsistencies

<br>
<br>
# Web Console

## v1.2.0 [unreleased]

As of version 1.2, the `Rebalance` button on the Web Consoles `Tasks` page
is deprecated and no longer available.
We based this decision on customer and support feedback regarding the feature.
For the time being, you will need to rebalance clusters manually.
The [Cluster Rebalance](/enterprise/v1.2/guides/rebalance/) guide offers
detailed instructions for performing a manual rebalance of your InfluxEnterprise cluster.
Please contact support with any questions or concerns you may have about this
development.

### Features

* Remove the `Tasks` page from the Web Console

### Bug Fixes

* Fix the [`autologout` configuration setting](/enterprise/v1.2/administration/configuration/#autologout-false)
* Fix the series and measurement counts on the `Database Manager` page

## v1.1.1 [2016-12-06]

This release is built with Go 1.7.4.
It resolves a security vulnerability reported in Go version 1.7.3 which impacts all
users currently running on the Mac OS X platform, powered by the Darwin operating system.

### Features

- Add the [`autologout` configuration setting](/enterprise/v1.2/administration/configuration/#autologout-false) to enable a forced logout on browser close

## v1.1.0 [2016-11-14]

### Features

- Add the [`session-lifetime` configuration setting](/enterprise/v1.2/administration/configuration/#session-lifetime-24h) to configure the time after which users are automatically logged out

## v1.0.3 [2016-10-07]

There are no new features or bugfixes in version 1.0.3.
This release is for maintaining version parity with clustering.

### Features

### Bugfixes

## v1.0.2 [2016-10-06]

### Features

### Bugfixes

- Fix systemd package upgrade symlink failure

## v1.0.1

### Features

* Log error messages from the InfluxEnterprise cluster

### Bugfixes

* Fix bug where users cannot log out of the Web Console

## v1.0.0

### Features

* **Rebalancing:** Rebalancing now ensures that all existing data adhere to the relevant [replication factor](/influxdb/v1.2/concepts/glossary/#replication-factor). See [Web Console Features](/enterprise/v1.2/features/web-console-features/#cluster-rebalancing) for more information.
* **User updates:** In versions 0.7.2 and below, users were loosely synced between the cluster and web console. In version 1.0, users have web-console-specific functions and are given cluster-specific permissions by being associated with a separate cluster account. The document [InfluxEnterprise Users](/enterprise/v1.2/features/users/) describes the new user arrangement in more detail. Please note that this change requires additional steps if you are [upgrading](/enterprise/v1.2/administration/upgrading/) from a previous version of the web console.

### Bugfixes