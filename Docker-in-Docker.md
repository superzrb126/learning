Docker restricts (a lot!) the capabilities of the containers it runs.
Some of those capabilities are needed in order to run "sub-containers".
Here is a list of the things that have to be done in order to run
Docker (and containers) within a Docker container.


## Capabilities

You need `net_raw` (for `iptables`), and `sys_admin` (for namespaces,
and possibly many other things).
required

## AppArmor

I ended up disabling AppArmor altogether, because finding the exact
permissions was cumbersome, and it looks like some permissions could
not be expressed exactly.

To disable AppArmor, but still get information about which AppArmor
mechanisms were involved, I edited `/etc/apparmor.d/lxc/lxc-default`,
and changed the flags to add `complain`. In my case, the flags became
`flags=(attach_disconnected,mediate_deleted,complain)`.

If you want, you can also try to be fine-grained; you will at least need
to enable AUFS mounts (`mount fstype=aufs`) but there will be other things
to do.

**Note:** the situation might be better (and the file names might have
changed) in more recent Ubuntu releases since more people expressed interest
in nested LXC containers.

## `/var/lib/docker`

Since AUFS branches cannot sit on an AUFS filesystem, make sure that
`/var/lib/docker` is a volume, or a bind-mount, or a local `tmpfs` mount.
Otherwise, the nested Docker won't be able to create AUFS mounts on it.


## `cgroups`

Make sure that `cgroups` are mounted within the container. You can do
this manually. You can also use the `cgroups-mount` helper, for convenience.
However, I don't remember where this helper comes from :-)

Sometimes, if `cgroups-mount` doesn't work, you can try:

    umount /sys/fs/cgroup ; cgroups-mount

It worked all the time for me.


## Patched `lxc-start`

When I tried to run Docker in Docker, the LXC userland tools were not
very cooperative. Namely, `lxc-start` tries to remove `CAP_SYS_BOOT`,
and bombs out it the capability was already removed (!). Since this
capability is unconditionally removed, fiddling with the `lxc_template.go`
file is not sufficient.

See [this changelog](
https://launchpad.net/~juju/+archive/pkgs/+sourcepub/2704379/+listing-archive-extra)
for some details.

For the record, I had to manually patch `src/lxc/start.c`; around line 599,
get rid of the `SYSERROR("failed to remove CAP_SYS_BOOT capability")` error
message.

## Final touch-ups

Make sure that:

- you have a bridge in the container
- `/var/lib/docker` is not AUFS
- cgroups are mounted
- you have a patched LXC *inside* the container
