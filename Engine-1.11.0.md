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

_(last update: 2016/03/21 22:51, Commit: d82ad12df813824d9166478068536d7d78cf0b69)_

**IMPORTANT**: With Docker 1.11, a docker installation is now made of 4 binaries (`docker`, `containerd`, `containerd-shim` and `runc`). If you have scripts relying on docker being a single static binaries, please make sure to update them. Interaction with the daemon stay the same otherwise, the usage of the other binaries should be transparent.

### Builder

- Fix a bug where Docker would not used the correct uid/gid when processing the `WORKDIR` command ([#21033](https://github.com/docker/docker/pull/21033))
- Fix a bug where copy operations would not use the proper uid/gid ([#20782](https://github.com/docker/docker/pull/20782), [#21162](https://github.com/docker/docker/pull/21162))

### Client

* Usage of the `:` separator for security option has been deprecated. `=` should be used instead ([#21232](https://github.com/docker/docker/pull/21232))
+ The client user agent is now passed to the registry on `pull`, `build`, `push`, `login` and `search` operations ([#21306](https://github.com/docker/docker/pull/21306), [#21373](https://github.com/docker/docker/pull/21373))
* Value passed to `--hostname` are now treated as FQDN ([#20200](https://github.com/docker/docker/pull/20200))
- After a brief memory loss, Docker once more learned to report a container PID statistics ([#21150](https://github.com/docker/docker/pull/21150))
* Docker info will now warn users if it can not detect the kernel version or the operating system ([#21128](https://github.com/docker/docker/pull/21128))
- Fix an issue where one letter directory name could not be used as source for volumes ([#21106](https://github.com/docker/docker/pull/21106))
- Fix a bug where credentials were retained after a user logging out ([#20860](https://github.com/docker/docker/pull/20860))
- Fix an issue where `docker stats --no-stream` output could be all 0s ([#20803](https://github.com/docker/docker/pull/20803))
- Fix a bug where some newly started container would not appear in a running `docker stats` command ([#20792](https://github.com/docker/docker/pull/20792))
* Post processing is no longer enabled for linux-cgo terminals ([#20587](https://github.com/docker/docker/pull/20587))
- Values to `--hostname` are now refused if they do not comply with [RFC1123](https://tools.ietf.org/html/rfc1123) ([#20566](https://github.com/docker/docker/pull/20566))
+ Docker learned how to use a SOCKS proxy ([#20366](https://github.com/docker/docker/pull/20366), [#18373](https://github.com/docker/docker/pull/18373))
* `docker ps` now supports displaying the list of volumes mounted inside a container ([#20017](https://github.com/docker/docker/pull/20017))
* `docker info` now also report Docker's root directory location ([#19986](https://github.com/docker/docker/pull/19986))
- Docker now prohibits login in with an empty username (spaces are trimmed) ([#19806](https://github.com/docker/docker/pull/19806))
* Docker events attributes are now sorted by key ([#19761](https://github.com/docker/docker/pull/19761))
* `docker ps` no longer show exported port for stopped containers ([#19483](https://github.com/docker/docker/pull/19483))
- Docker now cleans after itself if a save/export command fails ([#17849](https://github.com/docker/docker/pull/17849))
* Docker load learned how to display a progress bar ([#17329](https://github.com/docker/docker/pull/17329), [#120078](https://github.com/docker/docker/pull/20078))

### Distribution

- Fix a panic that occurred when pulling an images with 0 layers ([#21222](https://github.com/docker/docker/pull/21222))
- Fix a panic that could occur on error while pushing to a registry with a misconfigured token service ([#21212](https://github.com/docker/docker/pull/21212))
+ All first-level delegation roles are now signed when doing a trusted push ([#21046](https://github.com/docker/docker/pull/21046)) 
+ OAuth support for registries was added ([#20970](https://github.com/docker/docker/pull/20970))
* `docker login` now handles token using the implementation found in [docker/distribution](https://github.com/docker/distribution) ([#20832](https://github.com/docker/docker/pull/20832))
* `docker login` will no longer prompt for an email ([#20565](https://github.com/docker/docker/pull/20565))
* Docker will now fallback to registry V1 if no basic auth credentials are available ([#20241](https://github.com/docker/docker/pull/20241))
* Docker will now try to resume layer download where it left timeout after a network error/timeout ([#19840](https://github.com/docker/docker/pull/19840))
- Fix generated manifest mediaType when pushing cross-repository ([#19509](https://github.com/docker/docker/pull/19509))

### Logging

- Fix a race in the journald log driver ([#21311](https://github.com/docker/docker/pull/21311))
* The JSON Log driver got a speed boost ([#20605](https://github.com/docker/docker/pull/20605))
* Docker syslog driver now uses the RFC-5424 format when emitting logs ([#20121](https://github.com/docker/docker/pull/20121)) 
* Docker GELF log driver now allows to specify the compression algorithm and level via the `gelf-compression-type` and `gelf-compression-level` options ([#19831](https://github.com/docker/docker/pull/19831))
* Docker daemon learned to output uncolorized logs via the `--raw-logs` options ([#19794](https://github.com/docker/docker/pull/19794))
+ Docker, on Windows platform, now includes an ETW (Event Tracing in Windows) logging driver named `etwlogs` ([#19689](https://github.com/docker/docker/pull/19689))
* Journald log driver learned how to handle tags ([#19564](https://github.com/docker/docker/pull/19564))
+ The fluentd log driver learned the following options: `fluentd-address`, `fluentd-buffer-limit`, `fluentd-retry-wait`, `fluentd-max-retries` and `fluentd-async-connect` ([#19439](https://github.com/docker/docker/pull/19439))
+ Docker learned to send log to Google Cloud via the new `gcplogs` logging driver. ([#18766](https://github.com/docker/docker/pull/18766))


### Misc

+ Support for building the Docker cli for OpenBSD was added ([#21325](https://github.com/docker/docker/pull/21325))
* The `dockremap` is now created as a system user ([#21266](https://github.com/docker/docker/pull/21266)) 
- Fix a few response body leaks ([#21258](https://github.com/docker/docker/pull/21258))
- Docker, when run as a service with systemd, will now properly manage its processes cgroups ([#20633](https://github.com/docker/docker/pull/20633))
* Docker info now reports the value of cgroup KernelMemory or emits a warning if it is not supported ([#20863](https://github.com/docker/docker/pull/20863))
* Docker info now also reports the cgroup driver in use ([#20388](https://github.com/docker/docker/pull/20388))
* Docker completion is now available on PowerShell ([#19894](https://github.com/docker/docker/pull/19894))
* `dockerinit` is no more ([#19490](https://github.com/docker/docker/pull/19490),[#19851](https://github.com/docker/docker/pull/19851))
+ Support for building Docker on arm64 was added ([#19013](https://github.com/docker/docker/pull/19013))
+ Experimental support for building docker.exe in a native Windows Docker installation ([#18348](https://github.com/docker/docker/pull/18348))

### Networking

- Fix a bug where IPV6 addresses were not properly handled ([#20842](https://github.com/docker/docker/pull/20842))
* `docker network inspect` will now report all endpoints whether they have an active container or not ([#21160](https://github.com/docker/docker/pull/21160))
+ Experimental support for the MacVlan and IPVlan network drivers have been added ([#21122](https://github.com/docker/docker/pull/21122)) 
* Output of `docker network ls` is now sorted by network name ([#20383](https://github.com/docker/docker/pull/20383))
- Fix a bug where Docker would allow a network to be created with the reserved `default` name ([#19431](https://github.com/docker/docker/pull/19431))
* `docker network inspect` now returns whether a network is internal or not ([#19357](https://github.com/docker/docker/pull/19357))
+ Docker learned to create ipv6 enabled networks (`docker network create --ipv6`). This show up as a new  `EnableIPv6` field in `docker inspect`. ([#17513](https://github.com/docker/docker/pull/17513))

### Plugins

- Fix a file descriptor leak that would occur every time plugins were enumerated ([#20686](https://github.com/docker/docker/pull/20686))
- Fix an issue where Authz plugin would corrupt the payload body when faced with a large amount of data ([#20602](https://github.com/docker/docker/pull/20602))

### Runtime

+ Docker Windows gained a minimal `top` implementation ([#21354](https://github.com/docker/docker/pull/21354))
* Docker learned to report the faulty exe when a container cannot be started due to its condition ([#21345](https://github.com/docker/docker/pull/21345))
+ `docker -v` now accepts a new flag `nocopy`. This tell the runtime not to copy the container path content into the volume (which is the default behavior) ([#21223](https://github.com/docker/docker/pull/21223))
* Docker with device mapper will now refuse to run if `udev sync` is not available ([#21097](https://github.com/docker/docker/pull/21097))
- Fix a bug where Docker would not validate the config file upon configuration reload ([#21089](https://github.com/docker/docker/pull/21089))
- Fix a hang that would happen on attach if initial start was to fail ([#21048](https://github.com/docker/docker/pull/21048))
- Fix an issue where registry service options in the daemon configuration file were not properly taken into account ([#21045](https://github.com/docker/docker/pull/21045))
- Fix a race between the exec and resize operations ([#21022](https://github.com/docker/docker/pull/21022))
- Fix an issue where nanoseconds were not correctly taken in account when filtering Docker events ([#21013](https://github.com/docker/docker/pull/21013))
+ pkcs11 hardware signing is no longer an experimental feature  ([#21003](https://github.com/docker/docker/pull/21003))
- Fix the handling of Docker command when passed a 64 bytes id ([#21002](https://github.com/docker/docker/pull/21002))
* Docker will now return a `204` (i.e http.StatusNoContent) code when it successfully deleted a network ([#20977](https://github.com/docker/docker/pull/20977))
- Fix a bug where the daemon would wait indefinitely in case the process it was about to killed had already exited on its own ([#20967](https://github.com/docker/docker/pull/20967)
* The devmapper driver learned the `dm.min_free_space` option. If the mapped device free space reaches the passed value, new device creation will be prohibited. ([#20786](https://github.com/docker/docker/pull/20786))
+ Docker can now prevent processes in container to gain new privileges via the `--security-opt=no-new-privileges` flag ([#20727](https://github.com/docker/docker/pull/20727))
- Starting a container with the `--device` option will now correctly resolves symlinks ([#20684](https://github.com/docker/docker/pull/20684))
+ Docker now relies on [`containerd`](https://github.com/docker/containerd) and [`runc`](https://github.com/opencontainers/runc) to spawn containers. ([#20662](https://github.com/docker/docker/pull/20662))
- Fix docker configuration reloading to only alter value present in the given config file ([#20604](https://github.com/docker/docker/pull/20604))
+ Docker now allows setting a container hostname via the `--hostname` flag ([#20177](https://github.com/docker/docker/pull/20177))
+ Docker now allows executing privileged container while running with `--userns-remap` if both `--privileged` and the new `--userns=host` flag are specified ([#20111](https://github.com/docker/docker/pull/20111))
+ Docker now supports external credential stores ([#20107](https://github.com/docker/docker/pull/20107))
- Fix Docker not cleaning up correctly old containers upon restarting after a crash ([#19679](https://github.com/docker/docker/pull/19679))
* Docker will now error out if it doesn't recognize a configuration key within the config file ([#19517](https://github.com/docker/docker/pull/19517))
- Fix container loading, on daemon startup, when they depends on a plugin running within a container ([#19500](https://github.com/docker/docker/pull/19500))
- Fix a panic that would occur when failing to load a layer on store instantiation ([#19458](https://github.com/docker/docker/pull/19458))
- Fix a panic that occurred when a `exec` was started multiple times ([#19369](https://github.com/docker/docker/pull/19369)) 
* `docker update` learned how to change a container restart policy ([#19116](https://github.com/docker/docker/pull/19116))
* `docker inspect` now also returns a new `State` field containing the container state in a human readable way (i.e. one of `created`, `restarting`, `running`, `paused`, `exited` or `dead`)([#18966](https://github.com/docker/docker/pull/18966))
+ Docker learned to limit the number of active pids (i.e. processes) within the container via the `pids-limit` flags. NOTE: This requires `CGROUP_PIDS=y` to be in the kernel configuration. ([#18697](https://github.com/docker/docker/pull/18697))

### Security

* `restart_syscall`, `copy_file_range`, `mlock2` joined the list of allowed calls in the default seccomp profile ([#21117](https://github.com/docker/docker/pull/21117), [#21262](https://github.com/docker/docker/pull/21262))
* `send`, `recv` and `x32` were added to the list of allowed syscalls and arch in the default seccomp profile ([#19432](https://github.com/docker/docker/pull/19432))

### Volumes

* Output of `docker volume ls` is now sorted by volume name ([#20389](https://github.com/docker/docker/pull/20389))
* Local volumes can now accepts options similar to the unix `mount` tool ([#20262](https://github.com/docker/docker/pull/20262))
