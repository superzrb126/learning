# Schedule

* Freeze: 2016-03-17
* Release: 2016-04-07

# 1.11.0 - Release goals

#### [#20361](https://github.com/docker/docker/issues/20361) Containerd integration

Replace execdrivers with containerd.

#### [#20362](https://github.com/docker/docker/issues/20362) CLI separation

Separate the docker client cli from the docker daemon.

#### [#20363](https://github.com/docker/docker/issues/20363) Better Plugins Framework

Bake plugin management capabilities inside docker.

#### Miscellaneous

- [#20356](https://github.com/docker/docker/issues/20356) Label for non-container resources

# WIP: Changelog

* Output of `docker volume ls` is now sorted by volume name ([#20389](https://github.com/docker/docker/pull/20389))
* Output of `docker volume ls` is now sorted by network name ([#20383](https://github.com/docker/docker/pull/20383))
* Docker now supports external credential stores ([#20107](https://github.com/docker/docker/pull/20107))
* `docker ps` now supports displaying the list of volumes mounted inside a container ([#20017](https://github.com/docker/docker/pull/20017))
* Docker will now try to resume layer download where it left timeout after a network error/timeout ([#19840](https://github.com/docker/docker/pull/19840))
* Colorized logs can now be disabled with the new `--raw-logs` daemon flag ([#19794](https://github.com/docker/docker/pull/19794))
* Docker,on Windows platform, now includes an ETW (Event Tracing in Windows) logging driver named `etwlogs` ([#19689](https://github.com/docker/docker/pull/19689))
* Journald log driver now support the log command tag option ([#19564](https://github.com/docker/docker/pull/19564))
