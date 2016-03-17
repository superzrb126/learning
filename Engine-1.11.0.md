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

_(last update: 2016/03/16 17:04)_

**IMPORTANT**: With Docker 1.11, a docker installation is now made of 4 binaries (`docker`, `containerd`, `containerd-shim` and `runc`). If you have scripts relying on docker being a single static binaries, please make sure to update them. Interaction with the daemon stay the same otherwise, the usage of the other binaries should be transparent.


### Distribution

* Fix a panic that occurred when pulling an images with 0 layers ([#21222](https://github.com/docker/docker/pull/21222))
* Fix a panic that could occur on error while pushing to a registry with a misconfigured token service (https://github.com/docker/docker/pull/21030)
* `docker login` will no longer prompt for an email ([#20565](https://github.com/docker/docker/pull/20565))
* Docker will now fallback to registry V1 if no basic auth credentials are available ([#20241](https://github.com/docker/docker/pull/20241))
* `docker login` now handles token using the implementation found in [docker/distribution](https://github.com/docker/distribution) ([#20832](https://github.com/docker/docker/pull/20832))
* Docker will now try to resume layer download where it left timeout after a network error/timeout ([#19840](https://github.com/docker/docker/pull/19840))

### Client

* After a brief memory loss, Docker once more learned to report a container PID statistics ([#21150](https://github.com/docker/docker/pull/21150))
* Post processing is no longer enabled for linux-cgo terminals ([#20587](https://github.com/docker/docker/pull/20587))
* `docker ps` now supports displaying the list of volumes mounted inside a container ([#20017](https://github.com/docker/docker/pull/20017))
* `docker info` now also report Docker's root directory location ([#19986](https://github.com/docker/docker/pull/19986))

### Volumes

* Output of `docker volume ls` is now sorted by volume name ([#20389](https://github.com/docker/docker/pull/20389))
* Local volumes can now accepts options similar to the unix `mount` tool ([#20262](https://github.com/docker/docker/pull/20262))

### Builder

* Fix a bug where Docker would not used the correct uid/gid when processing the `WORKDIR` command ([#21033](https://github.com/docker/docker/pull/21033))

### Runtime

* Docker with device mapper will now refuse to run if `udev sync` is not available ([#21097](https://github.com/docker/docker/pull/21097))
* Fix the handling of Docker command when passed a 64 bytes id ([#21002](https://github.com/docker/docker/pull/21002))
* Docker will now return a `204` (i.e http.StatusNoContent) code when it successfully deleted a network ([#20977](https://github.com/docker/docker/pull/20977))
* The devmapper driver learn the `dm.min_free_space` option. If the mapped device free space reaches the passed value, new device creation will be prohibited. ([#20786](https://github.com/docker/docker/pull/20786))
* Docker can now prevent processes in container to gain new privileges via the `--security-opt=no-new-privileges` flag ([#20727](https://github.com/docker/docker/pull/20727))
* Starting a container with the `--device` option will now correctly resolves symlinks ([#20684](https://github.com/docker/docker/pull/20684))
* Fix docker configuration reloading to only alter value present in the given config file ([#20604](https://github.com/docker/docker/pull/20604))
* Docker now allows setting a container hostname via the `--hostname` flag ([#20177](https://github.com/docker/docker/pull/20177))
* Docker now allows executing privileged container while running with `--userns-remap` if both `--privileged` and the new `--userns=host` flag are specified ([#20111](https://github.com/docker/docker/pull/20111))
* Docker now supports external credential stores ([#20107](https://github.com/docker/docker/pull/20107))
* Fix Docker not cleaning up correctly old containers upon restarting after a crash ([#19679](https://github.com/docker/docker/pull/19679))
* `docker update` learned how to change a container restart policy ([#19116](https://github.com/docker/docker/pull/19116))
* `docker inspect` now also returns a new `State` field containing the container state in a human readable way (i.e. one of `created`, `restarting`, `running`, `paused`, `exited` or `dead`)([#18966](https://github.com/docker/docker/pull/18966))
* Docker learned to limit the number of active pids (i.e. processes) within the container via the `pids-limit` flags. NOTE: This require `CGROUP_PIDS=y` to be in the kernel configuration. ([#18697](https://github.com/docker/docker/pull/18697))

### Logging

* Docker syslog driver now uses the RFC-5424 format when emitting logs ([#20121](https://github.com/docker/docker/pull/20121)) 
* Docker GELF log driver now allows to specify the compression algorithm and level via the `gelf-compression-type` and `gelf-compression-level` options ([#19831](https://github.com/docker/docker/pull/19831))
* Docker daemon learned to output uncolorized logs via the `--raw-logs` options ([#19794](https://github.com/docker/docker/pull/19794))
* Docker, on Windows platform, now includes an ETW (Event Tracing in Windows) logging driver named `etwlogs` ([#19689](https://github.com/docker/docker/pull/19689))
* Journald log driver now support the log command tag option ([#19564](https://github.com/docker/docker/pull/19564))
* Docker learn to send log to Google Cloud via the new `gcplogs` logging driver. ([#18766](https://github.com/docker/docker/pull/18766))
* The `fluentd` log driver learned to process the new `fail-on-startup-error` log option preventing it to fail a container startup if it can't connect to the backend ([#18041](https://github.com/docker/docker/pull/18041))

### Networking

* `docker network inspect` will now report all endpoints whether they have an active container or not ([#21160](https://github.com/docker/docker/pull/21160))
* Experimental support for the MacVlan and IPVlan network drivers have been added ([#21122](https://github.com/docker/docker/pull/21122)) 
* Output of `docker network ls` is now sorted by network name ([#20383](https://github.com/docker/docker/pull/20383))
* `docker network inspect` now returns whether a network is internal or not ([#19357](https://github.com/docker/docker/pull/19357))
* Docker learned to create ipv6 enabled networks (`docker network create --ipv6`). This show up as a new  `EnableIPv6` field in `docker inspect`. ([#17513](https://github.com/docker/docker/pull/17513))