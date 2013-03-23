## Quick Install on Linux

Assuming root on Ubuntu 12.10 you can just run:

    curl -Ls https://raw.github.com/dotcloud/docker/master/install.sh | bash

As you can see by inspecting the URL, this will:

1. Use apt-get to install LXC
1. Download the latest binary
1. Install client and server into /usr/local/bin
1. Create simple Upstart script if one does not exist
1. Start the server

You have no images to bootstrap a container from, but you can download a public base image of Ubuntu 12.10 easily with:

    docker pull base


Vagrant Usage
-------------
Assuming you are in a directory with the docker sources. (e.g. by git clone this repo).

1. Install Vagrant from http://vagrantup.com
2. Run `vagrant up`. This will take a few minutes as it does the following:
    - Download Quantal64 base box
    - Kick off Puppet to do:
        - Download & untar most recent docker binary tarball to vagrant homedir.
        - Debootstrap to /var/lib/docker/images/ubuntu.
        - Install & run dockerd as service.
        - Put docker in /usr/local/bin.
        - Put latest Go toolchain in /usr/local/go.

Sample run output:

```bash
$ vagrant up
[default] Importing base box 'quantal64'...
[default] Matching MAC address for NAT networking...
[default] Clearing any previously set forwarded ports...
[default] Forwarding ports...
[default] -- 22 => 2222 (adapter 1)
[default] Creating shared folders metadata...
[default] Clearing any previously set network interfaces...
[default] Booting VM...
[default] Waiting for VM to boot. This can take a few minutes.
[default] VM booted and ready for use!
[default] Mounting shared folders...
[default] -- v-root: /vagrant
[default] -- manifests: /tmp/vagrant-puppet/manifests
[default] -- v-pp-m0: /tmp/vagrant-puppet/modules-0
[default] Running provisioner: Vagrant::Provisioners::Puppet...
[default] Running Puppet with /tmp/vagrant-puppet/manifests/quantal64.pp...
stdin: is not a tty
notice: /Stage[main]//Node[default]/Exec[apt_update]/returns: executed successfully

notice: /Stage[main]/Docker/Exec[fetch-docker]/returns: executed successfully
notice: /Stage[main]/Docker/Package[lxc]/ensure: ensure changed 'purged' to 'present'
notice: /Stage[main]/Docker/Exec[fetch-go]/returns: executed successfully

notice: /Stage[main]/Docker/Exec[copy-docker-bin]/returns: executed successfully
notice: /Stage[main]/Docker/Exec[debootstrap]/returns: executed successfully
notice: /Stage[main]/Docker/File[/etc/init/dockerd.conf]/ensure: defined content as '{md5}78a593d38dd9919af14d8f0545ac95e9'

notice: /Stage[main]/Docker/Service[dockerd]/ensure: ensure changed 'stopped' to 'running'

notice: Finished catalog run in 329.74 seconds
```

When this has successfully completed, you should be able to get into your new system with `vagrant ssh` and use `docker`:

```bash
$ vagrant ssh
Welcome to Ubuntu 12.10 (GNU/Linux 3.5.0-17-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

Last login: Sun Feb  3 19:37:37 2013
vagrant@vagrant-ubuntu-12:~$ docker help
Usage: docker COMMAND [arg...]

A self-sufficient runtime for linux containers.

Commands:
    run       Run a command in a container
    ps        Display a list of containers
    pull      Download a tarball and create a container from it
    put       Upload a tarball and create a container from it
    rm        Remove containers
    wait      Wait for the state of a container to change
    stop      Stop a running container
    logs      Fetch the logs of a container
    diff      Inspect changes on a container's filesystem
    commit    Save the state of a container
    attach    Attach to the standard inputs and outputs of a running container
    info      Display system-wide information
    tar       Stream the contents of a container as a tar archive
    web       Generate a web UI
    attach    Attach to a running container
```
    
## Proof of concept: building a python web app

```bash
$ docker import shykes/pybuilder

$ URL=http://github.com/shykes/helloflask/archive/master.tar.gz
$ BUILD_JOB=$(docker run -t shykes/pybuilder:11d4f58638a72935 /usr/local/bin/buildapp $URL)

$ docker attach $BUILD_JOB
[...]

$ BUILD_IMG=$(docker commit $BUILD_JOB _/builds/github.com/hykes/helloflask/master)

$ WEB_WORKER=$(docker run -p 5000 $BUILD_IMG /usr/local/bin/runapp)

$ docker logs $WEB_WORKER
 * Running on http://0.0.0.0:5000/
```