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

_(last update: 2016/03/08 08.29am)_

* `docker login` now handles token using the implementation found in [docker/distribution](https://github.com/docker/distribution) ([#20832](https://github.com/docker/docker/pull/20832))
* `--device` option now correctly resolves symlinks ([#20684](https://github.com/docker/docker/pull/20684))
* Post processing is no longer enabled for linux-cgo terminals ([#20587](https://github.com/docker/docker/pull/20587))
* `docker login` will no longer prompt for an email ([#20565](https://github.com/docker/docker/pull/20565))
* Output of `docker volume ls` is now sorted by volume name ([#20389](https://github.com/docker/docker/pull/20389))
* Output of `docker network ls` is now sorted by network name ([#20383](https://github.com/docker/docker/pull/20383))
* Docker now supports external credential stores ([#20107](https://github.com/docker/docker/pull/20107))
* `docker ps` now supports displaying the list of volumes mounted inside a container ([#20017](https://github.com/docker/docker/pull/20017))
* Docker will now try to resume layer download where it left timeout after a network error/timeout ([#19840](https://github.com/docker/docker/pull/19840))
* Colorized logs can now be disabled with the new `--raw-logs` daemon flag ([#19794](https://github.com/docker/docker/pull/19794))
* Docker,on Windows platform, now includes an ETW (Event Tracing in Windows) logging driver named `etwlogs` ([#19689](https://github.com/docker/docker/pull/19689))
* Journald log driver now support the log command tag option ([#19564](https://github.com/docker/docker/pull/19564))
* `docker network inspect` now returns whether a network is internal or not ([#19357](https://github.com/docker/docker/pull/19357))
* `docker update` learned how to change a container restart policy ([#19116](https://github.com/docker/docker/pull/19116))
* The `fluentd` log driver learned to process the new `fail-on-startup-error` log option preventing it to fail a container startup if it can't connect to the backend ([#18041](https://github.com/docker/docker/pull/18041))
* Docker learned to create ipv6 enabled networks (`docker network create --ipv6`). This show up as a new  `EnableIPv6` field in `docker inspect`. ([#17513](https://github.com/docker/docker/pull/17513))