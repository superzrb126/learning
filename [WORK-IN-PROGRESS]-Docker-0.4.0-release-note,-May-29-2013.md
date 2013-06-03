# THIS RELEASE NOTE IS WORK IN PROGRESS. DOCKER 0.4.0 IS NOT YET RELEASED

Dear Dockers,

Today I am happy to introduce a new release of Docker. In addition to numerous stability and usability fixes, this release introduces 2 highly anticipated features: Remote API and Build.

# Contents

* What is Docker?
* 0.4.0 summary
  * Remote API
  * Build
  * Reliability improvements
* What's next?
  * Broader kernel support
  * Better documentation
  * Plugin API
  * Production-ready
  * Play nice with other tools
* How you can help
* Hack day on June 11


# What is Docker?


[Docker](http://docker.io) is an open-source engine which automates the deployment of applications as highly portable, self-sufficient containers.

Docker containers are both hardware-agnostic and platform-agnostic. This means that they can run anywhere, from your laptop to the largest EC2 compute instance and everything in between - and they don't require that you use a particular language, framework or packaging system. That makes them great building blocks for deploying and scaling web apps, databases and backend services without depending on a particular stack or provider.

Docker is an open-source implementation of the deployment engine which powers [dotCloud](http://www.dotcloud.com), a popular Platform-as-a-Service. It benefits directly from the experience accumulated over several years of large-scale operation and support of hundreds of thousands of applications and databases.

# 0.4.0 summary

## Remote API

The whole point of Docker is to be a building block for complex automation. We have made the CLI as scriptable as possible, but at some point many of you found it difficult to write code that interacts with docker... until now.

Docker can now be remotely controlled via a simple HTTP/JSON protocol. This remote API is properly versioned and [fully documented](http://docs.docker.io/en/latest/api/docker_remote_api.html), so you can use it to integrate your favorite software project with Docker. It is also used by the official client, which means it supports all docker features.

In addition to the reference implementation, the community has already started working on client libraries for [Go](https://github.com/dotcloud/docker/pull/640), [Python](https://github.com/dotcloud/docker-py) and [Ruby](https://github.com/ActiveState/docker-ruby). Want to implement a client for your favorite language? [Get in touch!](https://github.com/dotcloud/docker/issues/new?title=Remote%20API%20client%20in%20MY_FAVORITE_LANGUAGE&body=Hi%2C%0aI'm+interested+in+implementing+a+client+library+for+the+docker+remote+API.+Is+anyone+else+interested+in+doing+this+with+me%3F%0a%0aI'm+also+not+sure+why+the+Docker+team+took+the+time+to+urlencode+this+ridiculously+long+message+in+their+link,+especially+considering+I'm+about+to+erase+the+whole+thing.+But+hey%2C+thanks+anyway,+I+guess!)

We look forward to discovering the amazing things you'll build with this!

# Build

Docker now features a *build* command, which can automatically build containers from your application's source code. All you need to do is add a simple *Dockerfile*.

A *Dockerfile* is a very simple text file - usually no longer than 5 to 10 lines - which expresses all your application's dependencies in one place and streamlines the process of assembling them - complete from base system to language-specific packages. *docker build* uses this file to assemble a new container, layer by layer.

For example, here's the Dockerfile [of the Hipache project](https://github.com/dotcloud/hipache/blob/master/Dockerfile):

```
from	ubuntu:12.04
run	echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
run	apt-get -y update
run	apt-get -y install wget git redis-server supervisor
run	wget -O - http://nodejs.org/dist/v0.8.23/node-v0.8.23-linux-x64.tar.gz | tar -C /usr/local/ --strip-components=1 -zxv
run	npm install hipache -g
run	mkdir -p /var/log/supervisor
add	supervisord.conf	/etc/supervisor/conf.d/supervisord.conf
```

And here's how you can build it:

```
$ git clone http://github.com/dotcloud/hipache
$ cd hipache
$ docker build -t dotcloud/hipache .
```

If the build is successful, you can use your newly built container with:

```bash
$ docker run dotcloud/hipache
```


# What's next?

## Broader kernel support

Our goal is to make Docker run everywhere, but currently Docker requires [Linux version 3.8 or higher with lxc and aufs support](http://docs.docker.io/en/latest/installation/kernel.html). If you're deploying new machines for the purpose of running Docker, this is a fairly easy requirement to meet. However, if you're adding Docker to an existing deployment, you may not have the flexibility to update and patch the kernel.

Expanding Docker's kernel support is a priority. This includes running on older kernel versions, but also on kernels with no AUFS support, or with incomplete lxc capabilities.

## Cross-architecture support

Our goal is to make Docker run everywhere. However currently Docker only runs on x86_64 systems. We plan on expanding architecture support, so that Docker containers can be created and used on more architectures.

## Even more integrations

We want Docker to be the secret ingredient that makes your existing tools more awesome. Thanks to this philosophy, Docker has already been integrated with [Puppet](http://forge.puppetlabs.com/garethr/docker),  [Chef](), [Openstack Nova](https://github.com/dotcloud/openstack-docker), [Jenkins](https://github.com/georgebashi/jenkins-docker-plugin), [DotCloud sandbox](http://github.com/dotcloud/sandbox), [Strider CI](http://blog.frozenridge.co/next-generation-continuous-integration-deployment-with-dotclouds-docker-and-strider/) and even [Heroku buildpacks](https://github.com/progrium/buildstep).

Expect Docker to integrate with even more of your favorite tools going forward, including:

* Alternative storage backends such as ZFS, LVM or [BTRFS](github.com/dotcloud/docker/issues/443)
* Alternative containerization backends such as [OpenVZ](http://openvz.org), Solaris Zones, BSD Jails and even plain Chroot.
* Process managers like [Supervisord](http://supervisord.org/), [Runit](http://smarden.org/runit/), [Gaffer](https://gaffer.readthedocs.org/en/latest/#gaffer) and [Systemd](http://www.freedesktop.org/wiki/Software/systemd/)
* Build and integration tools like Make, Maven, Scons, Jenkins, Buildbot and Cruise Control.
* Configuration management tools like [Puppet](http://puppetlabs.com), [Chef](http://www.opscode.com/chef/) and [Salt](http://saltstack.org)
* Personal development environments like [Vagrant](http://vagrantup.com), [Boxen](http://boxen.github.com/), [Koding](http://koding.com) and [Cloud9](http://c9.io).
* Orchestration tools like [Zookeeper](http://zookeeper.apache.org/), [Mesos](http://incubator.apache.org/mesos/) and [Galaxy](https://github.com/ning/galaxy)

## Plugin API

In order to make Docker easy to run everywhere and easy to integrate with all your favorite tools, we need to make it extremely easy to customize.

We are working on a plugin API which will greatly facilitate the integrations listed above - and many more we didn't even think about.

Let us know if you want to start playing with the API before it's generally available.


## Externally mounted volumes

In 0.3 we [introduced data volumes](https://github.com/dotcloud/docker/wiki/Docker-0.3.0-release-note%2C-May-6-2013#data-volumes), a great mechanism for manipulating persistent data such as database files, log files, etc.

Data volumes can be shared between containers, a powerful capability [which allows many advanced use cases](http://docs.docker.io/en/latest/examples/couchdb_data_volumes.html). In the future it will also be possible to share volumes between a container and the underlying host. This will make certain scenarios much easier, such as using a high-performance storage backend for your production database, making live development changes available to a container, etc.


## Better documentation

We believe that great documentation is worth 10 features. We are often told that "Docker's documentation is great for a 2-month old project". Our goal is to make it great, period.

If you have feedback on how to improve our documentation, please get in touch by replying to this email, or by [filing an issue](https://github.com/dotcloud/docker/issues). We always appreciate it!

## Production-ready

Docker is still alpha software, and not suited for production. We are working hard to get there, and we are confident that it will be possible within a few months.

# How you can help


# Hack day on June 11


HACK DAY ON JUNE 11
===================

The next Docker hack day is on Tuesday, June 11 at the dotCloud HQ in San Francisco. RSVP now at http://www.meetup.com/Docker-meetups/events/118026552/


Happy hacking!