# Schedule

* Freeze: 2016-03-22
* Release: 2016-04-12

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

_(last update: 2016/03/16 17:04, Commit: 2731dbc7977dba00405c974c47c4f079d441a2b2)_

**IMPORTANT**: With Docker 1.11, a docker installation is now made of 4 binaries (`docker`, `containerd`, `containerd-shim` and `runc`). If you have scripts relying on docker being a single static binaries, please make sure to update them. Interaction with the daemon stay the same otherwise, the usage of the other binaries should be transparent.


### Distribution

* Fix a panic that occurred when pulling an images with 0 layers ([#21222](https://github.com/docker/docker/pull/21222))
* Fix a panic that could occur on error while pushing to a registry with a misconfigured token service ([#21212](https://github.com/docker/docker/pull/21212))
* `docker login` will no longer prompt for an email ([#20565](https://github.com/docker/docker/pull/20565))
* Docker will now fallback to registry V1 if no basic auth credentials are available ([#20241](https://github.com/docker/docker/pull/20241))
* `docker login` now handles token using the implementation found in [docker/distribution](https://github.com/docker/distribution) ([#20832](https://github.com/docker/docker/pull/20832))
* Docker will now try to resume layer download where it left timeout after a network error/timeout ([#19840](https://github.com/docker/docker/pull/19840))
* Fix generated manifest mediaType when pushing cross-repository ([#19509](https://github.com/docker/docker/pull/19509))

### Client

* After a brief memory loss, Docker once more learned to report a container PID statistics ([#21150](https://github.com/docker/docker/pull/21150))
* Fix a bug where some newly started container would not appear in a running `docker stats` command ([#20792](https://github.com/docker/docker/pull/20792))
* Post processing is no longer enabled for linux-cgo terminals ([#20587](https://github.com/docker/docker/pull/20587))
* Docker learned how to use a SOCKS proxy ([#20366](https://github.com/docker/docker/pull/20366), [#18373](https://github.com/docker/docker/pull/18373))
* `docker ps` now supports displaying the list of volumes mounted inside a container ([#20017](https://github.com/docker/docker/pull/20017))
* `docker info` now also report Docker's root directory location ([#19986](https://github.com/docker/docker/pull/19986))
* Docker now prohibits login in with an empty username (spaces are trimmed) ([#19806](https://github.com/docker/docker/pull/19806))
* Docker events attributes are now sorted by key ([#19761](https://github.com/docker/docker/pull/19761))
* `docker ps` no longer show exported port for stopped containers ([#19483](https://github.com/docker/docker/pull/19483))
* Docker now cleans after itself if a save/export command fails ([#17849](https://github.com/docker/docker/pull/17849))
* Docker load learned how to display a progress bar ([#17329](https://github.com/docker/docker/pull/17329), [#120078](https://github.com/docker/docker/pull/20078))

### Volumes

* Output of `docker volume ls` is now sorted by volume name ([#20389](https://github.com/docker/docker/pull/20389))
* Local volumes can now accepts options similar to the unix `mount` tool ([#20262](https://github.com/docker/docker/pull/20262))


### Builder

* Fix a bug where Docker would not used the correct uid/gid when processing the `WORKDIR` command ([#21033](https://github.com/docker/docker/pull/21033))
* Fix a bug where copy operations would not use the proper uid/gid to create the parent directory ([#20782](https://github.com/docker/docker/pull/20782))

### Runtime

* Docker with device mapper will now refuse to run if `udev sync` is not available ([#21097](https://github.com/docker/docker/pull/21097))
* Fix a bug where Docker would not validate the config file upon configuration reload ([#21089](https://github.com/docker/docker/pull/21089))
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
* Docker will now error out if it doesn't recognize a configuration key within the config file ([#19517](https://github.com/docker/docker/pull/19517))
* Fix container loading, on daemon startup, when they depends on a plugin running within a container ([#19500](https://github.com/docker/docker/pull/19500))
* Fix a panic that would occur when failing to load a layer on store instantiation ([#19458](https://github.com/docker/docker/pull/19458))
* Fix a panic that occurred when a `exec` was started multiple times ([#19369](https://github.com/docker/docker/pull/19369)) 
* `docker update` learned how to change a container restart policy ([#19116](https://github.com/docker/docker/pull/19116))
* `docker inspect` now also returns a new `State` field containing the container state in a human readable way (i.e. one of `created`, `restarting`, `running`, `paused`, `exited` or `dead`)([#18966](https://github.com/docker/docker/pull/18966))
* Docker learned to limit the number of active pids (i.e. processes) within the container via the `pids-limit` flags. NOTE: This requires `CGROUP_PIDS=y` to be in the kernel configuration. ([#18697](https://github.com/docker/docker/pull/18697))

### Logging

* The JSON Log driver got a speed boost ([#20605](https://github.com/docker/docker/pull/20605))
* Docker syslog driver now uses the RFC-5424 format when emitting logs ([#20121](https://github.com/docker/docker/pull/20121)) 
* Docker GELF log driver now allows to specify the compression algorithm and level via the `gelf-compression-type` and `gelf-compression-level` options ([#19831](https://github.com/docker/docker/pull/19831))
* Docker daemon learned to output uncolorized logs via the `--raw-logs` options ([#19794](https://github.com/docker/docker/pull/19794))
* Docker, on Windows platform, now includes an ETW (Event Tracing in Windows) logging driver named `etwlogs` ([#19689](https://github.com/docker/docker/pull/19689))
* Journald log driver learned how to handle tags ([#19564](https://github.com/docker/docker/pull/19564))
* Docker learn to send log to Google Cloud via the new `gcplogs` logging driver. ([#18766](https://github.com/docker/docker/pull/18766))
* The `fluentd` log driver learned to process the new `fail-on-startup-error` log option preventing it to fail a container startup if it can't connect to the backend ([#18041](https://github.com/docker/docker/pull/18041))

### Networking

* `docker network inspect` will now report all endpoints whether they have an active container or not ([#21160](https://github.com/docker/docker/pull/21160))
* Experimental support for the MacVlan and IPVlan network drivers have been added ([#21122](https://github.com/docker/docker/pull/21122)) 
* Output of `docker network ls` is now sorted by network name ([#20383](https://github.com/docker/docker/pull/20383))
* Fix a bug where Docker would allow a network to be created with the reserved `default` name ([#19431](https://github.com/docker/docker/pull/19431))
* `docker network inspect` now returns whether a network is internal or not ([#19357](https://github.com/docker/docker/pull/19357))
* Docker learned to create ipv6 enabled networks (`docker network create --ipv6`). This show up as a new  `EnableIPv6` field in `docker inspect`. ([#17513](https://github.com/docker/docker/pull/17513))

### Security

* `send`, `recv` and `x32` were added to the list of allowed syscalls and arch in the default seccomp profile ([#19432](https://github.com/docker/docker/pull/19432))

### Plugins

* Fix a file descriptor leak that would occur every time plugins were enumerated ([#20686](https://github.com/docker/docker/pull/20686))
* Fix an issue where Authz plugin would corrupt the payload body when faced with a large amount of data ([#20602](https://github.com/docker/docker/pull/20602))

### Misc

* Docker, when run as a service with systemd, will now properly manage its processes cgroups ([#20633](https://github.com/docker/docker/pull/20633))
* Docker completion is now available on PowerShell ([#19894](https://github.com/docker/docker/pull/19894))
* `dockerinit` is no more ([#19490](https://github.com/docker/docker/pull/19490),[#19851](https://github.com/docker/docker/pull/19851))
* Support for building docker on arm64 was added ([#19013](https://github.com/docker/docker/pull/19013))
* Experimental support for building docker.exe in a native Windows Docker installation ([#18348](https://github.com/docker/docker/pull/18348))