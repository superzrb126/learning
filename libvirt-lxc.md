# Using libvirt-lxc instead of lxc-start

While Debian and Ubuntu systems tend to favor `lxc-start`, and provide AppArmor
profiles "optimized" for that purpose, other systems (specifically, those based
on Red Hat) seem to prefer `libvirt-lxc` to start containers.

Docker 1.0 will provide a modular architecture, and very probably allow to switch
between those different LXC helpers.

This document lists what you need to do to start your containers with libvirt-lxc.


## Config file

To start "something" (VM, container...) with libvirt, you need a XML config file
to describe it.

Here is a sample file. The comments are detailed below.

```xml
<domain type='lxc'>
  <!-- must by unique? -->
  <name>my-lovely-container</name>
  <!-- in MB -->
  <memory>524288</memory>
  <os>
    <type>exe</type>
    <!-- the following doesn't work because dockerinit complains;
         maybe because it has the wrong PID; didn't investigate much,
         but I bet it will be an easy fix.
    <init>/.dockerinit</init>
    <initarg>-g</initarg>
    <initarg>127.17.42.1</initarg>
    <initarg>-e</initarg>
    <initarg>HOME=/</initarg>
    <initarg>-e</initarg>
    <initarg>PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin</initarg>
    <initarg>- -</initarg>
    <initarg>bash</initarg>
    -->
    <!-- this, however, will work like a charm :-) -->
    <init>/bin/bash</init>
  </os>
  <vcpu>1</vcpu>
  <clock offset='utc'/>
  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>destroy</on_crash>
  <devices>
    <emulator>/usr/lib/libvirt/libvirt_lxc</emulator>
    <filesystem type='mount'>
      <source dir='/var/lib/docker/containers/d16f....e0f8/rootfs'/>
      <target dir='/'/>
    </filesystem>
    <!-- note: the following section doesn't work,
         because libvirt-lxc doesn't support binding files!
    <filesystem type='mount'>
      <source dir='/etc/resolv.conf'/>
      <target dir='/etc/resolv.conf'/>
    </filesystem>
    -->
    <!-- note: unless I'm wrong, libvirt-lxc can't allocate IP addresses
         directly, and expects the container to run a DHCP client -->
    <interface type='network'>
      <source network='default'/>
    </interface>
    <console type='pty' />
  </devices>
</domain>
```

## Starting it up

1. Use Docker to create a container and start a dummy long-running process
   in it; e.g. `docker run -i -d ubuntu bash`.
2. Note the container ID.
3. Create the XML file; make sure that the rootfs path is correct
   (replace `d16f....e0f8` with the *full ID* of the container).
4. Make sure that `libvirt-bin` is installed (or the packages providing `virsh`
   and `libvirtd`) and that `libvirtd` is running.
5. Start it up: `virsh -c lxc:/// create --console my-little-container.xml`.

You should get a shell inside the container.

   
## IP address allocation

I didn't find a way to tell libvirt-lxc "hey, give *that* exact IP address to the
container, will you?".

Work around 1: pass the IP address to the dockerinit wrapper, so that it can
configure it directly.

Work around 2: use an even more generic configuration system to assign the IP
"from outside".

Both approaches have their pros/cons. Method 1 can easily be translated to other
isolation/virtualization systems. Method 2 requires less work from dockerinit
(actually obviates the need for dockerinit).


## Bind mounts (/etc/resolv.conf and .dockerinit)

It looks like libvirt-lxc cannot bind-mount files.

A possible workaround (with LXC) is to use an AUFS ro layer for those files.
We already use an AUFS ro layer to make sure that those files exist; so we
could as well bite the bullet and use that AUFS layer to set the content of
those files as well. It would require a different layer for each container
(since the resolv.conf file can be different for each container) but it should
not be a problem.