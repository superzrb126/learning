Introducing Docker 0.4.0
========================

Dear Dockers,

Today we are happy to introduce a new release of Docker. In addition to numerous stability and usability fixes, this release introduces 2 highly anticipated features: Remote API and Build, as well as a very cool Openstack integration.

Contents
========

* What is Docker?
* 0.4.0 summary
  * Remote API
  * Build
  * Openstack integration
* What's next?
  * Broader kernel support
  * Cross architecture support
  * Even more integrations
  * Plugin API
  * Externally mounted volumes 
  * Better documentation
  * Production-ready
* Community news
  * Contributor of the month: Backjlack
  * New maintainer: Victor Vieux
  * How you can help
* Hack day on June 11


What is Docker?
===============

[Docker](http://docker.io) is an open-source engine which automates the deployment of applications as highly portable, self-sufficient containers.

Docker containers are both hardware-agnostic and platform-agnostic. This means that they can run anywhere, from your laptop to the largest EC2 compute instance and everything in between - and they don't require that you use a particular language, framework or packaging system. That makes them great building blocks for deploying and scaling web apps, databases and backend services without depending on a particular stack or provider.

Docker is an open-source implementation of the deployment engine which powers [dotCloud](http://www.dotcloud.com), a popular Platform-as-a-Service. It benefits directly from the experience accumulated over several years of large-scale operation and support of hundreds of thousands of applications and databases.


0.4.0 summary
=============

Remote API
----------

The whole point of Docker is to be a building block for complex automation. We have made the CLI as scriptable as possible, but at some point many of you found it difficult to write code that interacts with docker... until now.

Docker can now be remotely controlled via a simple HTTP/JSON protocol. This remote API is properly versioned and [fully documented](http://docs.docker.io/en/latest/api/docker_remote_api.html), so you can use it to integrate your favorite software project with Docker. It is also used by the official client, which means it supports all docker features.

In addition to the reference implementation, the community has already started working on client libraries for [Go](https://github.com/dotcloud/docker/pull/640), [Python](https://github.com/dotcloud/docker-py), [Ruby](https://github.com/ActiveState/docker-ruby), [Java](https://github.com/andreaturli/jdocker), [Clojure]() and NodeJS [1](https://github.com/appersonlabs/docker.io) [2](https://github.com/FrozenRidge/docker.js). Want to implement a client for your favorite language? [Get in touch!](https://github.com/dotcloud/docker/issues/new?title=Remote%20API%20client%20in%20MY_FAVORITE_LANGUAGE&body=Hi%2C%0aI'm+interested+in+implementing+a+client+library+for+the+docker+remote+API.+Is+anyone+else+interested+in+doing+this+with+me%3F%0a%0aI'm+also+not+sure+why+the+Docker+team+took+the+time+to+urlencode+this+ridiculously+long+message+in+their+link,+especially+considering+I'm+about+to+erase+the+whole+thing.+But+hey%2C+thanks+anyway,+I+guess!)

We look forward to discovering the amazing things you'll build with this!


Build
-----

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


Openstack integration
---------------------

We're excited to announce an early stage [integration between Openstack and Docker](http://github.com/dotcloud/openstack-docker). Openstack is an open-source IaaS implementation which is rapidly growing into a standard, with both heavyweight corporate backing and an active development community.

Like all major IaaS implementations, Openstack relies heavily on virtual machines. Although there will always be a case for VMs in certain applications, we believe lightweight containers are a great alternative in many scenarios, especially for payloads which are CPU- and memory-intensive and suffer from the performance overhead of VMs.

Openstack-docker is an experimental backend for Nova, Openstack's deployment scheduler. It allows you to deploy lightweight containers using the standard Openstack APIs. And because it is compatible with the [Docker public index](http://index.docker.io), any docker-compatible container in the World can now be deployed on your Openstack cluster within seconds.

Stay tuned for more Openstack integration scenarios!



What's next?
============

Broader kernel support
----------------------

Our goal is to make Docker run everywhere, but currently Docker requires [Linux version 3.8 or higher with lxc and aufs support](http://docs.docker.io/en/latest/installation/kernel.html). If you're deploying new machines for the purpose of running Docker, this is a fairly easy requirement to meet. However, if you're adding Docker to an existing deployment, you may not have the flexibility to update and patch the kernel.

Expanding Docker's kernel support is a priority. This includes running on older kernel versions, but also on kernels with no AUFS support, or with incomplete lxc capabilities.


Cross-architecture support
--------------------------

Our goal is to make Docker run everywhere. However currently Docker only runs on x86_64 systems. We plan on expanding architecture support, so that Docker containers can be created and used on more architectures.


Even more integrations
----------------------

We want Docker to be the secret ingredient that makes your existing tools more awesome. Thanks to this philosophy, Docker has already been integrated with [Puppet](http://forge.puppetlabs.com/garethr/docker),  [Chef](http://www.opscode.com/chef), [Openstack Nova](https://github.com/dotcloud/openstack-docker), [Jenkins](https://github.com/georgebashi/jenkins-docker-plugin), [DotCloud sandbox](http://github.com/dotcloud/sandbox), [Pallet](https://github.com/pallet/pallet-docker), [Strider CI](http://blog.frozenridge.co/next-generation-continuous-integration-deployment-with-dotclouds-docker-and-strider/) and even [Heroku buildpacks](https://github.com/progrium/buildstep).

Expect Docker to integrate with even more of your favorite tools going forward, including:

* Alternative storage backends such as ZFS, LVM or [BTRFS](github.com/dotcloud/docker/issues/443)
* Alternative containerization backends such as [OpenVZ](http://openvz.org), Solaris Zones, BSD Jails and even plain Chroot.
* Process managers like [Supervisord](http://supervisord.org/), [Runit](http://smarden.org/runit/), [Gaffer](https://gaffer.readthedocs.org/en/latest/#gaffer) and [Systemd](http://www.freedesktop.org/wiki/Software/systemd/)
* Build and integration tools like Make, Maven, Scons, Jenkins, Buildbot and Cruise Control.
* Configuration management tools like [Puppet](http://puppetlabs.com), [Chef](http://www.opscode.com/chef/) and [Salt](http://saltstack.org)
* Personal development environments like [Vagrant](http://vagrantup.com), [Boxen](http://boxen.github.com/), [Koding](http://koding.com) and [Cloud9](http://c9.io).
* Orchestration tools like [Zookeeper](http://zookeeper.apache.org/), [Mesos](http://incubator.apache.org/mesos/) and [Galaxy](https://github.com/ning/galaxy)
* Infrastructure deployment tools like [Openstack](http://openstack.org), [Apache Cloudstack](http://apache.cloudstack.org), [Ganeti](https://code.google.com/p/ganeti/)


Plugin API
----------

We want Docker to run everywhere, and to integrate with every devops tool. Those are ambitious goals, and the only way to reach them is with the Docker community. For the community to participate fully, we need an API which allows Docker to be deeply and easily customized.

We are working on a plugin API which will make Docker very, very customization-friendly. We believe it will facilitate the integrations listed above - and many more we didn't even think about.

Let us know if you want to start playing with the API before it's generally available.


Externally mounted volumes
--------------------------

In 0.3 we [introduced data volumes](https://github.com/dotcloud/docker/wiki/Docker-0.3.0-release-note%2C-May-6-2013#data-volumes), a great mechanism for manipulating persistent data such as database files, log files, etc.

Data volumes can be shared between containers, a powerful capability [which allows many advanced use cases](http://docs.docker.io/en/latest/examples/couchdb_data_volumes.html). In the future it will also be possible to share volumes between a container and the underlying host. This will make certain scenarios much easier, such as using a high-performance storage backend for your production database, making live development changes available to a container, etc.


Better documentation
--------------------

We believe that great documentation is worth 10 features. We are often told that "Docker's documentation is great for a 2-month old project". Our goal is to make it great, period.

If you have feedback on how to improve our documentation, please get in touch by replying to this email, or by [filing an issue](https://github.com/dotcloud/docker/issues). We always appreciate it!


Production-ready
----------------

Docker is still alpha software, and not suited for production. We are working hard to get there, and we are confident that it will be possible within a few months.




Community news
==============

Contributor of the month: Backjlack
-----------------------------------

To inaugurate our new "contributor of the month" section, we would like to send a special shout-out to [Backjlack](http://github.com/unclejack). Backjlack - although we've never met you, we want you to know we deeply appreciate your contribution to the community and the project. Your [participation on IRC](https://botbot.me/freenode/docker/search/?q=backjlack) is incredibly helpful, and it seems sometimes you know the project even better than the core contributors!

On behalf of the Docker core team, we hope we can buy you a beer one of these days.

Thanks!

New maintainer: Victor Vieux
----------------------------

We are happy to welcome a 3d core maintainer, Victor Vieux from Paris, France! This brings us up to a total of 10 maintainers across the project, including documentation, registry, packaging etc.


How you can help
----------------

* Become a maintainer. Docker is growing faster than we can keep up! We are looking for volunteers to help maintain the various components of the project - everything from documentation, packaging, project infrastructure to plugins and core components. Check out the [contribution guidelines](https://github.com/dotcloud/docker/blob/master/CONTRIBUTING.md) as a start.

* Make screencasts and articles. If you do anything cool and useful with docker, record a screencast and tell us about it! This could be dockerizing an application, installing it in a specific environment, cool usage tricks, etc. We recommend ascii.io, it's insanely easy to use.

* Dockerize your favorite tools. Docker plays well with other tools in the devops toolbox. Got a tool you want to integrate with Docker? Create a github issue and we'll help you out.

* Join the conversation. There are insane volumes of interesting conversations going on on irc
(#docker@freenode), twitter (#docker) and [the google group](https://groups.google.com/forum/?fromgroups#!forum/docker-club). Whether you have a beginner question or want to discuss a point of design, never hesitate to speak up!

* And of course all the usual ways of spreading the word - tweets, github follows, etc. etc. are always welcome.



Hack day on June 11
====================

The next Docker hack day is on Tuesday, June 11 at the dotCloud HQ in San Francisco. RSVP now at http://www.meetup.com/Docker-meetups/events/118026552/


Happy hacking!
The Docker team