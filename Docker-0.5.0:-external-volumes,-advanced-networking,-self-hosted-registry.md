Introducing Docker 0.5.0
========================

Dear Dockers,

Today we are happy to introduce a new release of Docker. In addition to numerous stability and usability fixes, this release adds support for external volumes, advanced networking options, a vastly improved self-hosted registry, and dozens of other improvements.

Contents
========

* What is Docker?
* 0.5.0 summary
  * External volumes
  * Advanced networking
  * Self-hosted registry
* What's next?
  * Broader kernel support
  * Cross architecture support
  * Even more integrations
  * Plugin API
  * Better documentation
  * Production-ready
* How you can help
* Hack day on July 30


What is Docker?
===============

[Docker](http://docker.io) is a open-source application container engine. It gives developers a way to package their app and all its dependencies into a portable container which can be deployed on any modern Linux machine, virtualized or not. Containers are completely sandboxed and do not interfere with each other (think "iPhone apps for the server"), have virtually no performance overhead, and can easily be moved across machines and datacenters. Best of all, they don't depend on any language, framework or packaging system.

0.5.0 summary
=============


Externally mounted volumes
--------------------------

In 0.3 we [introduced data volumes](https://github.com/dotcloud/docker/wiki/Docker-0.3.0-release-note%2C-May-6-2013#data-volumes), a great mechanism for manipulating persistent data such as database files, log files, etc.

In addition to [sharing volumes between containers](http://docs.docker.io/en/latest/examples/couchdb_data_volumes.html), you can now share volumes between a container and the underlying host. This makes certain scenarios much easier, such as using a high-performance storage backend for your production database, making live development changes available to a container, etc.

You can also initialize volumes in your container at build time, by using the VOLUME instruction in a Dockerfile.

Advanced networking
-------------------

### Static port allocation

When redirecting a port to a container, you can now specify *which* public port to redirect. If you don't specify a public port, docker will revert to allocating a random public port.

For example:

- `-p 80:5000` redirects public port 80 to private port 80
- `-p 80` redirects a random public port to private port 80



With 0.5 you can now specify the public port using:
- `-p 5000:80` to bind the port 80 of the container to the port 5000 on the host
- `-p 800:2000/udp` to bind the port 2000 of the container to the port 800 on the host using udp

### UDP port allocation

You can now redirect both UDP ports and TCP ports to your container. To specify a UDP port, simply add the suffix '/udp' to your port specification. For example:

- `-p 2000/udp` redirects a random public UDP port to private UDP port 8000
- `-p 80:80/tcp` redirects public port 80/TCP to private port 80/TCP
- `-p 80:80` also redirects public port 80/TCP to private port 80/TCP

Self-hosted registry
---------------------

This release includes major improvements to the [self-hosted registry](https://github.com/dotcloud/docker-registry). It is now much easier to store and distribute docker images privately within your organization. You can also combine multiple registries to reflect your organization: for example, you might store your open-source images on the public registry, company-wide base images on a company-wide registry, application nightly builds in a team-specific registry, and so on.

Self-hosted registries are completely independent of the [public registry](http://index.docker.io): images stored on it are completely private and not publicly searchable or accessible in any way. You can control access to your registry using standard HTTP proxying and authentication.

To find out which registry an image comes from, simply look at its name: the URL of the registry is used as a prefix. For example, `images.myorganization.com/ubuntu` refers to an image in the `images.myorganization.com` registry. Image names with no URL, like `ubuntu` or `shykes/couchdb` always refer to the public registry.

To pull an image from a registry, simply include the registry url in the image name:

    docker pull myregistry.mydomain.com/myapp

Conversely, to push an image to a registry, you must first tag it with the appropriate name:

    # Tag to create a repository with the full registry location.
    # The location (e.g. myregistry.mydomain.com) becomes
    # a permanent part of the repository name
    docker tag 0u812deadbeef myregistry.mydomain.com/myapp

    # Push the new repository to its home location
    docker push myregistry.mydomain.com/myapp

And that's it! No other configuration necessary.

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

Let us know if you want to start playing with the API before it's generally available. And if you're in the San Francisco area, make sure to drop by [our next hack day](http://www.meetup.com/Docker-meetups/events/127801022/) for a sneak preview!


Better documentation
--------------------

We believe that great documentation is worth 10 features. We are often told that "Docker's documentation is great for a 2-month old project". Our goal is to make it great, period.

If you have feedback on how to improve our documentation, please get in touch by replying to this email, or by [filing an issue](https://github.com/dotcloud/docker/issues). We always appreciate it!


Production-ready
----------------

Docker is still alpha software, and not suited for production. We are working hard to get there, and we are confident that it will be possible within a few months.




How you can help
==============

* Contribute! Docker is growing faster than we can keep up! We are looking for volunteers to help improve the various components of the project - everything from documentation, packaging, project infrastructure to plugins and core components. Check out the [contribution guidelines](https://github.com/dotcloud/docker/blob/master/CONTRIBUTING.md) as a start.

* Make screencasts and articles. If you do anything cool and useful with docker, record a screencast and tell us about it! This could be dockerizing an application, installing it in a specific environment, cool usage tricks, etc. We recommend ascii.io, it's insanely easy to use.

* Dockerize your favorite tools. Docker plays well with other tools in the devops toolbox. Got a tool you want to integrate with Docker? Create a github issue and we'll help you out.

* Join the conversation. There are insane volumes of interesting conversations going on on irc
(#docker@freenode), twitter (#docker) and [the google group](https://groups.google.com/forum/?fromgroups#!forum/docker-club). Whether you have a beginner question or want to discuss a point of design, never hesitate to speak up!

* And of course all the usual ways of spreading the word - tweets, github follows, etc. etc. are always welcome.



Hack day on July 30
====================

The next Docker hack day is on Tuesday, July 30 at the dotCloud HQ in San Francisco. RSVP now at http://www.meetup.com/Docker-meetups/events/127801022/


Happy hacking!
The Docker team