# Schedule

* Freeze: 2016-01-14
* Release: 2016-02-04

# 1.10.0 - Release goals

#### [#17142](https://github.com/docker/docker/issues/17142) Seccomp integration

#### [#15188](https://github.com/docker/docker/issues/15188) Better API implementation

We would like to rewrite our API, not to change the endpoint in a first phase, but at least to have a better, typed, self-documented, versioned version of the Docker API that we could share between Engine and Swarm.

#### [#14298](https://github.com/docker/docker/issues/14298) Progress towards a client-side builder

As explained in the [roadmap document](https://github.com/docker/docker/blob/master/ROADMAP.md#122-builder), we want to enable client-side build.

#### Distribution refactoring

Refactor client-side push/pull code:
- Separate images from layers
- End-to-end content addressibility.

#### DNS-based name resolution

Avoid relying on `/etc/hosts` for container name resolution, and switch to a DNS based solution.

#### Stable user namespaces

Continue the work on [#15187](https://github.com/docker/docker/issues/15187) (currently experimental).