# Schedule

* Freeze: 2015-09-xx
* Release: 2015-10-xx

# 1.9.0 - Release goals

## User namespaces

User namespaces are a long awaited feature, on which [Phil Estes (@estesp)](https://github.com/estesp) did some amazing work (see [#12648](https://github.com/docker/docker/pull/12648)). Unfortunately, the integration unexpectedly conflicted with the recent introduction of [`libnetwork`](https://github.com/docker/libnetwork).

We want to fix this for 1.9.0 and be able to merge Phil's work, which means:
- All containers have their own user namespace (effectively preventing the use of `--net=host` or `--net=container:<id>`).
- A daemon-wide setting remaps the root user for all containers.

## Client-side builder

As explained in the [roadmap document](https://github.com/docker/docker/blob/master/ROADMAP.md#122-builder), we want to enable client-side build.

## #14212 - Progress towards the OCI integration

The Engine currently has several compiled-in `execdriver` implementations, the default one being [`libcontainer`](https://github.com/opencontainers/runc/blob/master/libcontainer). We the apparition of the [Open Containers Initiative](https://github.com/opencontainers/specs), we want the Engine to have a single execution backend capable of shelling-out to an OCI compliant binary. Currently, the standard and default implementation of such binary is [runC](https://runc.io).

It's unlikely that this will ship as the sole, default execution backend for Docker 1.9.0, but we intend to show some progress in a separate branch.